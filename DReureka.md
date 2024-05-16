ICRA2024에서 "생성형 AI"가 기존의 robotics의 접근을 크게 obsolete(황량) 할것이다"라는 토론

DReureka
nvidia - 2024년 5월 4일
https://eureka-research.github.io/dr-eureka/assets/dreureka-paper.pdf
EUREKA
https://arxiv.org/abs/2310.12931

simulation에서 학습된 정책들을 현실세계로 transfer시키는 방법은 로봇의 스킬을 얻기 위한 좋은 전략이다. 하지만 이 과정에서의 simulation에서의 물리적 파라미터 reward function을 튜닝 등의 수동적 디자인하는 과정은 느리고 매우 사람의 노력이 많이 필요하다. 이 논문에서는 이 과정을 LLM을 통해 자동화하고 가속하는 것을 목표로한다(sim-to-real)

DrEureka (Domain Randomization Eureka)를 제시하는데 reward 과정을 디자인 하는 것 뿐 아니라 sim-to-real 의 전이를 위한 DR 파라미터의 configuration 또한 수행한다.

이런 sim2real transfer를 위한 많은 기술들이 있지만,( Transfer learning in robotics: ´
An upcoming breakthrough?,  System identification—a survey. Automatica) 이가 가장 LLM이 자동화하기 primed 됨.

DR은 simulation 상황에서의 물리적 파라미터의 분포를 랜덤하게 적용시키는 것인데 이는 매우 중요하다. 일반적으로 사람에 의해 수동적으로 조정되는데 physics reasoning과( 다른 surface에서 걸을 때의 마찰력 고려)나 지식을 필요로하는 이 최적화과정이 매우 어렵기 때문이다

하지만 LLM이 이러한 2과정을 수행하는 것은 매우 비효율적 -> 세가지 과정으로 나눠 수행

1. LLM(대규모 언어 모델)을 활용하여 보상 함수를 합성
2. perturbed(교란된) 시뮬레이션에서 실행하여 물리 매개변수에 대한 적절한 샘플링 범위를 만듦
3. 이를 통해 적절한 DR configuration 생성함

1번과정에 SOTA 알고리즘 Eureka 사용(저번 nvidia 펜돌리기 논문)
또한 보상 함수를 만들기 위한 프롬프트에 안전관련 지시를(safety term)를 넣음

2번과정을 위해 최고의 보상 candidate와 관련 정책을 갖추고, DrEureka는 다양한 교란된 시뮬레이션 dynamics에서 정책을 평가하여 환경 물리(environmnetal physics) 파라미터에 대한 reward aware physics prior(RAPP)를 구성

3번 과정이 2번과정을 통해 DR config을 LLM을 생성할 때의 효율적인 search range를

마침내 LLM이 rapp를 context로 활용해 몇개의 DR 분포들의 후보들(현실에서의 더 적합한 정책들을 다시 훈련시키는)을 생성함  

이는 사족보행, 사물조작, 원형 물체 위에서 걷기등의 실험에서 human supervised보다 뛰어난 성능을 보임. best 정책은 인간이 만든 정책보다 큐브 조립에서 300퍼센트 이상의 성능 향상이 있었음(정해진 시간)
