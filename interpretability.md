page 1
------------------
해석가능한 AI

https://transformer-circuits.pub/2024/scaling-monosemanticity/index.html
Scaling Monosemanticity: Extracting Interpretable Features from Claude 3 Sonnet

AI의 해석가능함

1. AI가 무엇을 배우는가?
2. 특정결과를 생성하기 위해 입력값의 어떤 특징,패턴들이 사용됬나

이를 통해 얻을 수 있는것

1. AI자체의 디버깅, 조정( 하이퍼 파라미터가 구체적으로 미치는 영향, 왜 모델이 성능이 좋지 않을까)

2. 우리가 모르는 데이터 자체의 새로운 인사이트

예시) 암 진단을 하는 detection모델이 있다 할 때, 데이터내의 특정 세포의 특이점으로(안 알려진) 이를 판단한다던가의 모델이 학습한 내용을 알 수 있음)

3. 배포 하기 전의 상품성 강화

interpretability는 안전성 부분에서의 매우 큰 이점

참고영상 ↓
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/2b885b35-380b-474d-88f0-703037201d05)

page 2 
--------------------------------
작년에 sparese autoencoeder를 통해 monosemantic feature들을 작은 트랜스포머에서 추출해냈는데 이를 sonet으로 확장시켜 다양한 추상적인 특징을 뽑아냈다.
다국적, multimodal 적인 특징으로 
<- 언어, input 데이터 타입 상관없이 어떤 특징을 가지고 있음
여기에는 safety 에 관련된 특징들도 있었는데 이를 clamp하거나 zero 강화, 약화 시킬 수 있었다

주요한 결과
SAE로 polysemantic 특징을 monosemantic하게 추출하는게 가능함

scaling law에 따라 SAE를 훈련시키는게 가능함

거대 LLM을 조종하는데 이러한 feature를 활용가능함

또한 속임수, 아첨,편견, 위험한 콘텐츠에 관련된 feature 또한 존재함

page 3
--------------------
모든 실험들은 linear representation hypothesis와 superposition hypothesis에 기반함
쉽게 말해 
linear representation hypothesis : feature, 뉴럴넷은 feature라는 의미 있는 어떤 개념을 표현한다. (활성화 공간안(activation space)의 direction으로써)

superposition hypothesis : 중첩된 polysemanticity 

이를 dictionary learning 즉 SAE를 통해 분리하게 됨

page 4
-----------------
SAE는 encoder , decoder 레이어로 이루어짐
encoder : learned linear transformation  -> 비선형 ReLU를 통해 
maps the activity to higher-dimensional layer 
이 때 고차원 레이어가 feature
decoder : linear transformation of the feature activations를 통해 model activation을 재구성

이 모델이 훈련됨 
재구성 에러(reconstruction error)를 줄이거나 feature activation이 L1 정규화 페널티를 줄이는 방향으로(이는 sparsity에 인센티브를 부여하는 일)


page 4
------------------
이러한 훈련을 통해 모델의 activation를 "feature direction" (SAE 디코더 가중치)의 linear combination으로 근사적으로 분해하는 게 가능해짐

-> sparsity 증가
특정 input의 context의 특정 토큰에 대해, 모델 activation (활성화 가능한 많은 특징들 중(pool)에서) 활성화된 소수의 특징들에 의해 "설명"이 된다 이제.

->>> 이과정 Towards Monosemanticity 

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/cacf91e0-6e51-45ab-b8b0-cb46a4045b56)


page 5
--------------
feature들이 의미하는 것과 뉴럴넷 안에서 이것들이 무슨 역할을 하는가

feature 아래 같은게 있음

1.샌프란시스코에 있는 golden gate bridge

2.뇌과학 : 뇌과학 관련 토론과 이에 관한 연구들 ↓↓

색깔이 진할수록 그 토큰이 얼마나 강하게 activate 시키는 지를 설명

1. 가장 강한 activation은 다리에 관한 참조 문헌 그 외 약한 activation에는 다른 다리, 기념물, 관광지등이 있음

2.뇌 과학 feature는 관련 뇌과학 책, 강의 토론 등에서 가장 강한 activation 그 외에는 심리학, 인지과학, 관련철학에서 activation

feature에 관한 2가지 주장
특징이 active 할 때, 관련 개념이 context 내에 존재합니다 (specificity).
특징의 활성화에 개입시 관련된 downstream 행동이 일어남 (행동에 영향)

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/29067fe6-1da9-44b0-9682-a14dccb834e8)



![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/9abf0f6b-00e6-4450-8941-210afb43d279)

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/d98a25e3-ea87-4b26-9b3e-64ffb268cc16)


![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/72c3ca8d-3eda-469b-961e-62f45ca7439f)

page 3
----------------------
influence on behavior

이러한 feature들이 어떤 input에 강하게 activate 될수있게 clamp할 수 있음

모델의 output을 특정 방향으로 바꾸는데 매우 효과적

예시 golden gate brige 특징을 10배 clamp시킨다면
-> 모델이 자신 스스로를 golden gate bridge로 인식하기 시작함

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/f2193b64-ba6e-4fc9-a8b0-c340a8eaef1f)

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/0460d8ce-977b-464b-b226-f2b571bc36eb)

page 4
--------------------
sophisticated feature
지금까지는 세상의 shallow한 정보를 reflect하는 간단한 개념을 뉴런이 fire하는것을 살펴본것에 불과함

Sonet과 같은 더 크고 더 정교한 모델은 더 깊이있고 명백한 이해에 대한 feature를 담고 있을 것이라 생각함.
이를 위해 프로그래밍에 대한 context에서 feature가 활성화 되는 것을 탐구
예) 코드의 정확성, 변수 타입
코드 정확성에서의 어떤 feature 예시
잘못된 변수 이름을 보았을 때 fire되는 1M/1013764 feature라는 게 있다
이는 파이썬에 한정되지 않고 C에서도 유사한 feature가 활성화됨

재밌는 점은 rihgt 를 그냥 영어전문에 포함시켰을 때는 활성화되지 않음

그 외에도 1M/1013764 는 코드 상의 어떤 특정 에러가 아닌 광범위한 범위에서 활성화됨

이를 통해 모델을 컨트롤하는게 그래서 가능한가?
가능함

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/3a4f003e-a88d-4a15-8046-c76dc0646b02)
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/f5582eef-dd0e-4c80-a410-b99fdf97eeb3)
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/84b7a330-0751-4359-99cd-4811173231ea)
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/623f8421-3978-47ce-b979-339dd2090a68)

실제로 아무런 버그가 없는 코드를 입력값으로 줬을 때 위와 관련된 feature들을 clamp할시 hallucinate 현상을 일으킴을 알 수 있었음

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/60484fb6-558e-4f8b-89f4-852345dea27a)




page 5
--------------------------------------
Features vs. Neurons 생략

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/c9aee127-0a75-42ea-9ce0-8c56c9bbcda8)

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/21a37ec6-0814-45b2-b7bc-4480001bc364)

page 6
------------------------------------------
feature 이웃 
이런 feature 벡터들 간의 코사인 유사도를 계산할 수 있엇는데
예를 들어 golden bridge gate 특징은 샌프란시스코에 있는 lcatraz and the Presidio와 같은 유명한 장소의 feature와 관련이 있었는데 조금만 멀어지면 샌프란시스코에 가까운 Lake Tahoe, Yosemite National Park, and Solano Count와 관련된 덜 관련된 feature가 잇는 것을 확인할수 잇었음

또한 파라미터가 커질수록 이러한 feature들이 세분화 됫는데 1M SAE에서의 “San Francisco” feature가 4M SAE에서는 features 34M SAE에서는 eleven fine-grained features 로 나눠졌다.

밑은 IMMUNOLOGY FEATURE , INNER CONFLICT FEATURE 예시이다
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/f7f10c2a-d863-4811-a792-1cb1e64cd830)

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/e597b087-9ba0-4b79-a1fa-725e9556ef40)

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/180019d3-827e-4b57-9dce-1d850cb5bde4)

page 7
Feature Categories, Feature Completeness, 
------------------------

page 8
---------------------
feature의 다른 매우 흥미로운 적용 사례는, 어떠한 output을 생성하기 위해 중간과정의 컴퓨팅을 조사할 수 있게 한다는 것이다.
