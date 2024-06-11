bitsfusion 

Stable Diffusion v1.5 의 Unet의 메모리를
FP16 (1.72 GB) ->1.99 bits (219 MB)

2024년 6월 6일 - snap

https://snap-research.github.io/BitsFusion/

시사하는바 
1. 대부분이 4bit 에 연구, 그럼에도 여기의 1.99bit이 더 좋은 퍼포먼스
2. 크기가 작은 디퓨전모델에서의 시도가 대부분

각 레이어의 서로 다른 양자화(2bit,3bit,4bit)로 인한 에러
-> quantization-aware training (QAT) 진행

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/4ca53458-f56c-4d9b-82b5-549e628b540c)


![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/96c1f96a-7b2b-4fbf-af0d-8f027a12948e)
이를 위해 balance integer 추가

e.g)
1비트 모델에서 후보 정수 집합을 {0, 1}에서 {-1, 0, 1}로 조정
-> 더 균형 잡힌 분포를 달성
balanced n-bits 가중치 = log(2n + 1)비트

Scaling Factor Initialization via Alternating Optimization.
기존 QAT에서는 Min-Max initialization 전략 사용 하지만

1-bit 같이 매우 적은 비트 양자화에서는 기존의 full-precision bit와의 가중치를 잘 고려하지 못해 큰 양자화 오류와 수렴이 어려워짐

-> 이를 위해 양자화된 가중치와 기존 정밀도의 가중치 간의 l2 error를 줄이기 위해 아래의 식 사용

스케일링 팩터는 양자화된 모델의 각 가중치에 적용되는 상수
이 값은 가중치의 값을 조절해 양자화된 모델이 전체 정밀도 모델과 유사한 성능을 발휘할 수 있도록 도와줌

(5) 생략

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/9d3e2ad4-084f-4acc-a846-eef21ee85c0a)

[3] Two-Stage Training Pipeline

3.1 CFG-aware Quantization Distillation

기존의 방식처럼 양자화 후 fine-tuning 진행

또한 기존의 전체 정밀도 모델을 teacher로써 이 학습을 진행하는 것이 그냥 noise prediction보다 성능이 좋음을 확ㄷ인 가능함(distillation)

이 distillation과정에서 CFG(classifier-free guidance
)에 대해 인식하는 것이 중요 
i.e) teacning 중에 텍스트 삭제
이를 밑의 loss N

3.2 Feature Distillation
fine-grained 레벨에서 feature 추출 진행
https://arxiv.org/abs/2305.15798

3.3 Quantization Error-aware Time Step Sampling
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/55a9990e-a0c9-4d14-a91a-95ea1b76bc3c)
