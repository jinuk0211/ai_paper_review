1. Video Is Worth Thousands of Words - 비디오 자막생성 SOTA (비디오 생성 모델 데이터 생성용도)
2024년 6월 10일

2. Image is Worth 32 Tokens - 이미지 토크나이져 (비젼 인코더?)
2024년 6월 11일 - bytedance

3. autoregressive Model beats Diffusion - VAR (DiT x)
2024년 6월 10일

1. video is worth thousands of words
-------------------------------
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
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/8a839919-df50-4c40-a5f6-c26a751000e4)



2. Image is worth 32 tokens
   ---------------------

이미지 토크나이져의 중요성 (이미지 -> latent space)
TiTok (Transformer-based 1-Dimensional Tokenizer)라는 걸 사용

classification , object detection, segmentation, multi-modal large language
models <-- Unet 비스무리한(enco-deco) tokenizer-detokenizer 이 사용된다하면
detokenizer은 사실상 쓸모가 없지 않나??

이유 
이미지(input)에서 이미지(output)로가 아닌 이미지(input)에서 특정한 포맷, 구조로 결과값이 나옴 

https://arxiv.org/pdf/2406.06040
https://arxiv.org/pdf/2406.07550
DiT-XL/2의 SOTA 성능 능가, 74배 빨리
근본적인 한계 
원래 이미지의 패치가 latent 토큰의 이미지 정보 위치와 같음
맨 위 왼쪽 패치 = 맨 위 왼쪽 latent token 
https://arxiv.org/abs/2406.06525
