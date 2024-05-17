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
2. perturbed(교란된) 시뮬레이션에서 실행하여 물리 파라미터에 대한 적절한 샘플링 범위를 만듦
3. 이를 통해 적절한 DR configuration 생성함

1번과정에 SOTA 알고리즘 Eureka 사용(저번 nvidia 펜돌리기 논문)
또한 보상 함수를 만들기 위한 프롬프트에 안전관련 지시를(safety term)를 넣음

2번과정을 위해 최고의 보상 candidate와 관련 정책을 갖추고, DrEureka는 다양한 교란된 시뮬레이션 dynamics에서 정책을 평가하여 환경 물리(environmnetal physics) 파라미터에 대한 reward aware physics prior(RAPP)를 구성

3번 과정이 2번과정을 통해 DR config을 LLM을 생성할 때의 효율적인 search range를

마침내 LLM이 rapp를 context로 활용해 몇개의 DR 분포들의 후보들(현실에서의 더 적합한 정책들을 다시 훈련시키는)을 생성함  

이는 사족보행, 사물조작, 원형 물체 위에서 걷기등의 실험에서 human supervised보다 뛰어난 성능을 보임. best 정책은 인간이 만든 정책보다 큐브 조립에서 300퍼센트 이상의 성능 향상이 있었음(정해진 시간)




III. 문제 설정

 sim-to-real design problem setting을 형식화
 먼저 대상의 실제 환경과 reward 함수 또는 도메인 랜덤화 구성이 설정되지 않은 시뮬레이션 환경을 가정함 sim-to-real RL의 목표는 시뮬레이션 환경에서 정책을 학습하고 추가 학습 없이 이를 대상의 실제 환경으로 직접 transfer 하는 것 
 수학적으로, 시뮬레이션 환경은 마르코프 process M = (S, A, T)로 정의될 수 있음 여기서 S는 환경 상태 공간(environment state space), A는 행동 공간(action space), T는 transition 함수
실제로 로봇과 객체 모델의 URDF 파일을 시뮬레이터에 porting하면 됨

task에 대해, 실제 task 목적을 F : Π → R로 표현
여기에서 F는 정책을 숫자값에 매핑하여 그 성능을 나타내는 지표 
sim-to-real 알고리즘 Algo는 reward 함수 설계와 DR을 위해 M(시뮬레이션 환경)과 구체적인 task를 입력으로 받아 reward 함수 R과 transition 함수 distribution(분포) T를 출력
(1)

그런 다음 정책 학습 알고리즘 A가 정책 π를 결과물로 냄
π ← A(M, T, R) (2)

이 정책은 알려지지 않은(모르는 unknown 시뮬레이션 환경 아님) 실제 MDP(실제 환경) M*에서 평가됨
(3)

sim-to-real의 목표는 위의 3의 f를 최대화하도록 T와 R을 설계하는 것
(4)
일반적으로 원래 Algo는 T(도메인 랜덤화)와 R(reward)을 설계하는 사람 엔지니어 -> 이를 LLM으로
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/018d474b-449c-4742-bad3-09aeabba80e8)

 구체적으로, 시뮬레이터에는 (물체의 질량과 마찰력 등)의 물리 파라미터 P가 있고 이 값은 분포(distribution)에 따라 설정하고 샘플링될 수 있음
 
도메인 랜덤화
(1) 물리 파라미터 집합 {p} ⊆ P를 선택
(2) 선택한 각 파라미터에 대한 랜덤화(randomization) 범위를 선택하는 것
이 가능하지만 reward 함수는 사람이 어떤 과제에 대한 코드를 짜야하기에
 trial-and-error(시행착오 발생) -> LLM

이는 시뮬레이션이 코드로 구현되어 있기 때문에 가능 (LLM)
세부사항
LLM이 문자열 형태의 T와 R을 출력하며, 이는 downstream 정책 학습을 위해 적절한 프로그램 형식으로 컴파일

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/d3dcef2d-094b-4f0a-abca-be553971fe06)

IV . 방법
A. Eureka Reward Design
B. Safety Instruction 생략


안전한 reward 함수는 환경 선택을 고정하여(fixing a choice of envirionment) 정책 행동을 정규화(regularize)할 수 있지만, 그 자체로는 시뮬레이션에서 실제 환경(sim-to-real)으로의 transfer에 충분하지 않음

Reward DrEureka와 π(정책) initial이 주어졌을 때, LLM에 효과적인 DR 구성을 생성하도록 프롬프트할 수 있는법은?

이것은 훈련 때에(simulation) 대상의 실제 환경 M*에 접근할 수 없기 때문에 어려운 문제
->
기본 시뮬레이션 물리 파라미터 값이 있는 M에는 접근가능 
BUT 기본값 그 자체로는 LLM에 대한 guidance로 충분하지 않음
파라미터 스케일과 샘플링할 기본 범위(base range)에 대한 정보를 제공하지 않기 때문
 
시뮬레이션 물리 파라미터에는 내장된 범위(즉, 최대 및 최소값)가 있지만, 이러한 범위는 너무 넓어서 정책 학습을 상당히 방해할 수 있음
  
->>>
이런 기본 범위를 제한하기 위해 간단한 RAPP 메커니즘 도입

high level에서 RAPP는 π initial(초기 정책)이 똑같이 성능을 발휘할 수 있는 환경 파라미터의 maximally diverse range를 찾음

우리의(논문에서의) 인사이트
DR이 task reward 함수에 의존하고 도메인 랜덤화 없이 학습된 정책 행동에 맞춤화되어야 한다는 것

예시로, 너무 넓은 범위에서 마찰력을 랜덤화하면 reward 함수를 만들때, 학습하기 어려운 마찰력 값을 샘플링할 가능성이 높음

실제로 RAPP는 각 도메인 랜덤화 파라미터에 대해 훈련에 "그럴듯한(feasible)" 값의 최소과 최대를 계산 

각 파라미터에 대해 다양한 잠재적 값 범위를 찾고, 각 값에 대해 시뮬레이션에서 해당 값을 설정(다른 모든 파라미터는 기본값으로 유지)하고 이 수정된 시뮬레이션에서 π Eureka를 실행

 정책의 성능이 미리 정한 기준을 충족하면 이 값을 해당 파라미터에 대해 실행 가능한 것으로 여김

각각의 물리 파라미터에서만 정책을 평가하기만 하면됨
-> 계산 비용이 적음, 효율적으로 병렬 처리 가능

D. LLM for domain randomization 생략

https://github.com/eureka-research/DrEureka/tree/main
