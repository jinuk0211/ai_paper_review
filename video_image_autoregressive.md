1. Video Is Worth Thousands of Words - 비디오 자막생성 SOTA (비디오 생성 모델 데이터 생성용도)
2024년 6월 10일

2. Image is Worth 32 Tokens - 이미지 토크나이져 (비젼 인코더?)
2024년 6월 11일 - bytedance

3. autoregressive Model beats Diffusion - VAR (DiT x)
2024년 6월 10일

video is worth thousands of words
-------------------------------
https://arxiv.org/pdf/2406.06040

비디오는 이미지보다 시간이라는 차원이 더해지기 때문에 더 많은 정보를 포함하고 있다
각각의 비디오마다 여러개의 이벤트를 포함하고 이 이벤트가 여러개의 scene들을 포함하기 때문에 자막처리 주석 다는게 매우 힘들다
기존의 WebVid-10M and Panda-70M 데이터셋은 15초보다 짧은 클립에 2,3개의 문장으로 묘사되어있다
vript는 상세한 묘사와 카메라 움직임 여러 기법들에 대한 묘사도 같ㄷ이 적혀있다(panning,tilling,클로즈업 등)\

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/87c0a1b5-3744-4716-9e3b-6a2f2a05ac00)

방법
각각의 연속적인 클립에 자막을 적은뒤 이를 concat
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/e37ac4cc-dd73-4dc7-9693-1a394ef8bfb9)

3가지 주안점
1. voiceover
음성이나 나레이션에 관한 캡션도 생성(비디오의 프레임 관련 자막도 함게)해서 이를 input(비디오에서 추출한 프레임들)에 concat해서 video llm의 데이터로 사용

2. timestamp
기존의 비디오 LLM은 비디오에서 프레임을 추출하기 위한 sampling 전략을 사용하는데 이는 정해진 프레임을 비디오에서 시간 간격마다 추출 하는 것이다 하지만 이러한 방식의 단점이 프레임의 순서에 대한 정보는 알 수 있지만 그 프레임들이 얼마동안 지속되는지에 대한 정보는 잘 나타내지 못한다는 것인데
이를 위해 input과 output에 timestamp (밑의 figure2) 를 추가해서 videollm이 프레임들, 즉 scenes의 시작과 끝을 예측할 수 있게 한다(timestamp)이를 통해 위의 단점을 해결함

따라서 데이터 형식이 4가지로 

e.g) 일본여행 브이로그라 치면
1. 한 관련 장면
- 숙소에 도착함 (프레임들)
2. 위의 장면과 그에 관한 오디오 자막
- 숙소에 도착함, 이 장면들에 관한 오디오 설명 (텍스트 포맷)
4. 여러개의 관련 장면
- 숙소에 도착함, 오사카성 관광
5. 여러개의 관련 장면과 그에 관한 오디오 자막
- 위의 장면들과 그에 관한 오디오 설명
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/c141f340-4917-41e7-8a6d-2aeab13a5230)

Vript-HAL 벤치마크 (hallucination)
실제로 기존의 image LLM들 평가
BLIP2, InstructBLIP, Qwen-VL, LLaVA 1.6 34B, video LLMs, VideoChatGPT, VideoLLaMA, VideoChat, VideoChat2, ST-LLM, Claude 3-Sonnet Opus, GPT-4V


![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/aa1e0c61-5d8b-420c-9055-6eb73d8960df)

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/8a839919-df50-4c40-a5f6-c26a751000e4)



2. Image is worth 32 tokens
   ---------------------
https://arxiv.org/pdf/2406.07550

이미지 토크나이져의 중요성 (이미지 -> latent space) 에서 latent space를 1차원의 어떤 시퀸스로
TiTok (Transformer-based 1-Dimensional Tokenizer)라는 걸 사용

classification , object detection, segmentation, 멀티모달 LLM (output이 이미지가 아닌 1D sequence)
 <-- Unet 비스무리한(enco-deco) 구조의 tokenizer-detokenizer 이 사용된다하면 (2D 구조로 굳이 압축 시킬 필요가 있나?)

근본적인 한계 
원래 이미지의 패치가 latent 토큰의 이미지 정보 위치와 같음
맨 위 왼쪽 패치 = 맨 위 왼쪽 latent token 
토크나이져가 이미지 rebundancy를 활용해 더 효과적으로 압축할 수 있을 수도 모르는게 사전 차단됨

예시로 object queries 나 perceiver resampler task 에서는 64개정도의 미리정해진 토큰 수로 인코딩을 한 후 이를 bounding box나 자막을 생성하기 위해 그대로 활용한다
즉 인코더(input) -> latent space (1D 시퀸스)

이유 
이미지(input)에서 이미지(output)로가 아닌 이미지(input)에서 특정한 포맷, 구조로 결과값이 나옴 

사용된 아키텍쳐
ViT 인코더, 디코더, vector quantizer
DiT-XL/2의 SOTA 성능 능가, 74배 빨리
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/145a45a6-6a3c-4499-938e-da785499c913)

메소드
기존의 vector quantizer 설명
input (높이, 너비, 채널) 이미지가 인코더 통과후 
Z = (높이/f , 너비/f, 임베딩 dim) 이후 vector quantizer 통과후
디코더를 통과해 detokenzize 됨

이는 latent representaion을 어떤 정직인 2D grid에 국한되게 함 ,위의 한계 포함
그리고 f= downsample factor는 대부분 4,8,16으로 (256 x 256 x 3)의 이미지가 잇다면
토큰의 수는 보통 (잠재 표현공간) 아래와 같다
256/4 x 256/4 = 4096
256/8 x 256/8 = 1024
256/16 x 256/16 = 256 

그래서 ViT를 통해 일단 이미지를 patchify(H/f × W/f ×D)한 후
latent token(K×D)을 concat 하고 이를 ViT 인코더에 넣는데 
이 인코더 결과값의 shape을 latent token으로 유지 -> 1차원 sequence latent representaion
그리고 이 잠재 표현공간 벡터를 quantized 시킨후 
mask token(한 이유: Masked autoencoders are scalable vision learners, All are worth words: A
vit backbone for diffusion models)을 concat하고 이를 decoder에 통과시키면 끝이난다

매우 간단함에도 별로 연구되지 않음
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/37ec8a42-5dcc-453f-bf48-c98bfd552d93)


![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/3f3a63bc-a142-4f5e-b623-a2aab84959f2)

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/868a35d8-96dc-4769-a8ae-fe628ef96ddc)

evaluation 
https://github.com/openai/guided-diffusion/tree/main/evaluations

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/4f3a5a13-e72c-44fb-af82-31defd4c9d51)


autoregressive Model beats Diffusion 
---------------------------------
https://arxiv.org/abs/2406.06525
llama 기반으로 특정한 inductive bias 없이 좋은 퍼포먼스 스케일링이 적합하게 된다면

토크나이져
downsample 비율 16 ,rFID 스코어 0.94로 재구성 퀄리티또한 높음
다른 확산모델과의 비교

1. 1M에서 3.1B 파라미터에 이르는 일련의 클래스 조건부(class conditional) 이미지 생성 모델, ImageNet 256×256 벤치마크에서 2.18 FID를 달성하여 LDM, DiT와 같은 인기 있는 확산 모델을 능가

2. LAION-COCO와 고품질의 이미지를 통해 2단계 학습을 거친 775M 파라미터의 텍스트 조건부 이미지 생성 모델, 시각적 품질과 텍스트 정렬(text alignement) 에서 경쟁력 있는 성능을 입증
3. 
vLLM 프레임워크를 통해, 326%에서 414%의 속도 향상을 달성
