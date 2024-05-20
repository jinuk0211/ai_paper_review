GPT 4o - part 1

3개 이상의 modality Mirasol3B - 2024년 4월 3일자 (구글 딥마인드)
https://arxiv.org/pdf/2311.05698

카멜레온 - 2024년 5월 16일자 (Meta)
https://arxiv.org/pdf/2309.02591 https://arxiv.org/pdf/2405.09818

vlm을 만들때 중요한것 - 2024년 5월 3일자 (huggingface)
https://arxiv.org/pdf/2405.02246

gemini 1.5 https://arxiv.org/abs/2403.05530
chameleon
기존의 vlm

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/68bdf539-8118-4957-a3fe-48174c158afb)
what's behind gpt4o 

카멜레온 - Meta 2024년 5월 16일 
idefics- huggingface 2024년 5월 3일
Mirasol3B - google Deepmind 2024년 4월 3일
gemini 1.5 - 2024년 4월 25일

카멜레온
이미지 토크나이저 
early fusion
새로운VLM
https://arxiv.org/pdf/2405.09818
humaneval에서의 GPT-4V , 제미니 pro 능가

early fusion model 

기존의 VLM처럼 vision encoder를 써서 이미지의 상태 표현 공간을 만드는것 X

토크나이져로 이미지와 텍스트를 처리해 바로 autoregressive LLM에 feed하는 방식

input말고도 output 또한 interleaved 되있는 것을 알수 잇다
밑은 gpt4v와 카멜레온의 예시인데 gpt4v는 오직 텍스트만을 결과로 내놓지만 카멜레온은 text image 토큰 둘다 내놓는 모습

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/27c49fc7-d39a-4b4d-bd98-92fd0c4d4995)

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/469001cd-f8d2-4353-a701-baff26cfff43)

토크나이져
text vocab size - 65536
img vocab size - 8192

훈련 방법

1. 먼저 텍스트 토큰(codellama와 llama2때 사용한 데이터셋) - 2.9T

2. 그 후 텍스트-이미지 토큰(유튜브 scrape 추정) - 1.5T

3. interleaved 텍스트-이미지 토큰(위키피디아, 웹사이트) - 400B

재앙적 망각 (catastrophic)을 막기 위해 위의 데이터에서 50%의 선택 후 instruction 데이터와 질 좋은 데이터로 post-훈련


아키텍쳐
RMS norm, SwiGLU, RoPE 를 사용하는데 

학습이 수렴하지 못함

다중 모달리티 훈련시 softmax의 엔트로피가 달라져 ? (?) 발산

softmax(z) = softmax(z + c)

문제 원인->translation invariant property of softmax

모델이 모든 가중치를 모달리티 간에 공유
각 모달리티는 약간씩 its norm을 증가시키고 서로 "경쟁"

https://arxiv.org/pdf/2405.09818 page 6

이는 학습 초반에는 문제가 되지 않지만, bf16의 표현 범위 벗어나면 발산 

그냥 단일 모달리티에서 이 문제를 로짓 드리프트(logit drift) 문제(Wortsman et al., 2023)라고 함


![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/964f434c-7f69-406f-b9e9-4af85c5f8b52)

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/1528691f-809c-441f-b1a0-d0c74a85f816)


이를 해결하기 위해 밑의 과정

QK-norm, attention, feed-forward 레이어 이후에 드롭아웃(dropout)의 필요성 발견 

Chameleon-34B를 안정화하기에 불충분

 추가적인 정규화 재정렬이 필요(norm reorder)

 구체적으로, Swin transformer 정규화 전략 사용

 이점은 Feed forward 블록의 norm growth을 제한한다는 점

이는 SwiGLU 활성화 함수의 multiplicate nature 때문에 추가로 문제가 될수도 있음

GPU 사용량
 global batch size 가 2의 23승으로 매우매우 큰데 이를 위해 1024개의 a100 gpu가 쓰임
 
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/dd06d7df-f490-4b8b-94c1-d217b7451624)

alignment
https://arxiv.org/abs/2305.11206

데이터

SFT 데이터셋을 다음과 같이 나눔

텍스트, 코드, visual chat, 이미지 생성, 텍스트/이미지 혼합 생성, 그리고 safety

데이터셋의 예시를 Figure 7에 있음

텍스트 SFT 데이터셋은 LLaMa-2 (Touvron et al., 2023)

코드 SFT 데이터셋은 CodeLLaMa (Roziere et al., 2023)

이미지 생성 SFT 데이터셋
 Schuhmann et al. (2022)의 aesthetic 분류기를 사용하여 기존의 데이터의 각 이미지에 적용하고 필터링하여 매우 심미적인 이미지를 큐레이팅
->
aesthetic 분류기에서 최소 6점으로 평가된 이미지를 먼저 선택한 다음, 크기와 가로 세로비가 512 × 512(이미지 토크나이저의 기본 해상도)와 가장 가까운 상위 64K 이미지를 선택

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/e5dcabc0-a186-4207-88e0-00337bf3cd72)
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/1d2419f1-cdb4-4e32-a210-74e92ad70d37)


visual chat과 텍스트/이미지 혼합 생성 SFT 데이터
we focused on very high-quality data collection using third-party vendors following a similar strategy recommended by Touvron et al. (2023);

safety 데이터 생략

3.2 SFT 전략
데이터 밸런스
우리는 SFT 단계에서 모달리티의 균형을 맞추는 것이 고품질 alignment에 중요하다는 것을 발견

 특히 SFT 단계 동안 모달리티 쌍 사이에 심각한 불균형이 있을 때,
모델은 그 모달리티의 무조건적인 우선 순위(unconditional prior)를 학습하게 되어 특정 모달리티의 생성을 무시하거나 과장할 수 있음

최적화

초기 학습률을 1e-5로 설정한 코사인 학습률 스케쥴러

0.1로 설정한 가중치 감쇠

최대 4096개의 토큰의 시퀀스를 수용하는 배치 크기를 128로 



SFT 프롬프트와 그에 대한 답변이 있는데
효율성을 높이기 위해 가능한 한 많은 프롬프트와 답변을 각 시퀀스에 포함하고, 프롬프트의 끝과 답변의 시작을 구분하기 위해 고유 토큰을 삽입

autoregressive training 목표를 사용하여 프롬프트 토큰에 대한 손실을 선택적으로 마스킹 (selectively masking the loss for the prompt tokens)
->
이 타겟팅된 접근 방식은 답변 토큰에만 기반하여 모델을 최적화할 수 있게 함
->
전체적으로 약간의 성능 향상을 제공 

드롭아웃(dropout) 비율을 0.05

pretrain 동안 동일한 zloss를 유지

프롬프트의 이미지 : 모든 정보가 이미지에 포함되도록 테두리 패딩으로 크기를 조정
답변의 이미지 : 시각적으로 좋은 이미지 생성 품질을 보장하기 위해 중앙을 자름


what matters when building VLM
idefics 2
사용한 데이터셋 cauldron
https://huggingface.co/datasets/HuggingFaceM4/the_cauldron
특이점
 pooling layer는 정보 손실이 일어나는 단점이 있다
-> perceiver resampler라는 학습가능한 트랜스포머 기반의 풀링을 사용
