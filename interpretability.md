page 1
------------------------
해석가능한 AI - anthropic

https://transformer-circuits.pub/2024/scaling-monosemanticity/index.html
Extracting Interpretable Features from Claude 3 Sonnet

모델의 interpretablity의 목표
1. AI가 무엇을 배우는가?
2. 특정결과를 생성하기 위해 입력값의 어떤 특징,패턴 들이 사용됬나

이를 통해 얻을 수 있는것

1. AI자체의 디버깅, 조정( 하이퍼 파라미터가 구체적으로 미치는 영향, 왜 모델이 성능이 좋지 않을까)

2. 우리가 모르는 데이터 자체의 새로운 인사이트
예시) 암 진단을 하는 detection모델이 있다 할때, 데이터내의 특정 세포의 특이점으로(사람이 모르는) 이를 판단한다던가의 모델이 학습한 내용을 알 수 있음)

3. 배포 하기 전의 상품성 강화

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/2b885b35-380b-474d-88f0-703037201d05)

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/29067fe6-1da9-44b0-9682-a14dccb834e8)

page 2
------------------
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


![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/9abf0f6b-00e6-4450-8941-210afb43d279)

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/d98a25e3-ea87-4b26-9b3e-64ffb268cc16)


![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/72c3ca8d-3eda-469b-961e-62f45ca7439f)
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/0460d8ce-977b-464b-b226-f2b571bc36eb)

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/3a4f003e-a88d-4a15-8046-c76dc0646b02)
