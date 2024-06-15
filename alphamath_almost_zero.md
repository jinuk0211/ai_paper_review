AlphaMath Almost Zero: Process Supervision Without Process
-----------------------
2024년 5월 23일
https://arxiv.org/pdf/2405.03553

Accessing GPT-4 level Mathematical Olympiad
Solutions via Monte Carlo Tree Self-refine with
LLaMa-3 8B: A Technical Report
----------------
2024년 6월 13일
https://arxiv.org/pdf/2406.07394
https://github.com/trotsky1997/MathBlackBox

수학적 추론 능력을 올리기 위해 LLM + Monte carlo tree search
알고리즘 과정 
Selection, self-refine, self-evaluation, and Backpropagation, Upper Confidence Bound(UCB) 
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/aefe60d5-773c-470e-9082-42d24c551234)

  전통적인 MCTS 전략은 LLM 출력(무한하고 연속적인 공간을 포함)의 확률적이고 생성적인 성질과 잘 맞지 않을 수 있다.  이러한 불일치는 LLM의 고유한 특성에 더 잘 맞는 expectation(MCTS) 계산과 Backpropagation을 위한 맞춤형 접근이 필요하게 만듭니다. 또한, 우리는 고도의 의사 결정을 위한 탐사-활용 균형을 최적화하기 위해 (UCB) 공식을 통합한 동적 프루닝 전략을 제시
평가 
GSM8K, GSM Hard, MATH,  Odyssey, AIME, and OlympiadBench 에서의 매우 큰 성능 증가

MCTS
-------------
Monte Carlo Tree Search (MCTS)는 게임 및 복잡한 결정 과정에서 널리 사용되는 의사 결정 알고리즘으로, 탐색 트리를 구축하고 결과를 시뮬레이션하여 행동의 가치를 추정하는 방식
 일반적으로 네 가지 주요 단계로 구성됨 (Browne 등, 2012):

선택 (Selection): 루트에서 시작하여 UCT(상한 신뢰 구간) 전략을 기반으로 promising 자식 노드를 탐색
리프 노드에 도달할 때까지 진행

확장 (Expansion): 리프 노드에서는 게임의 종료 상태가 아닌 경우 새로운 자식 노드를 추가하여 잠재적인 미래의 움직임을 illustrate

시뮬레이션 또는 평가 (Simulation or Evaluation): 새로 추가된 노드에서 알고리즘은 무작위 시뮬레이션(rollout이라고도 함)을 통해 게임의 종료까지 임의의 움직임을 선택하며 노드의 잠재력을 평가합니다.

역전파 (Backpropagation): 시뮬레이션 후 결과(승리, 패배 또는 무승부)가 루트로 전파되어 각 통과한 노드의 통계 데이터(예: 승리, 패배 횟수)를 업데이트하고, 이를 통해 미래의 결정에 정보를 제공

MCTS는 이러한 단계들을 반복적으로 거쳐 결정 트리를 점진적으로 구축하여, 상태 공간이 방대하여 직접적인 최적 전략 계산이 불가능한 상황에서 최적의 의사 결정을 위한 전략을 개선

Upper Confidence Bound
-----------------
j = 행동  
X = 그에 따른 평균 보상
N = father 노드의 총 방문 횟수
n = 노드 j 가 시뮬레이션 동안 방문된 횟수
C = 탐사-활용의 밸런스를 맞추기 위한 상수값

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/906c37b3-64db-45b0-80cb-b150c89173da)

P: 다루고 있는 문제 인스턴스
A: 각각이 P에 대한 잠재적인 답변을 나타내는 노드의 집합
M: 각 노드에서 가능한 행동의 집합, 가능한 self-refine modifications 를 나타냄
R: modification의 효율성과 퀄리티를 기반으로 노드의 self-보상을 샘플링하는 함수
Ra: 노드 a의 자기 보상 함수 R을 사용하여 샘플링한 모든 자기 보상 결과를 저장하는 집합


T: 탐색 프로세스의 종료를 결정하는 함수(기준 : 최대 반복횟수 달성 or 목표한 답변 품질 달성) 

Improve Mathematical Reasoning in Language
Models by Automated Process Supervision
===========================
https://github.com/MARIO-Math-Reasoning/Super_MARIO
2024년 6월 5일
https://arxiv.org/pdf/2406.06592

OmegaPRM - divide-and-conquer Monte Carlo Tree Search
 Process supervision은 reasoning의 중간 단계에 reward를 부여한다

  이진 검색을 통해 CoT에서 첫 번째 오류를 신속하게 식별하고 긍정적 및 부정적 예시를 균형 있게 조정하여 효율성과 품질을 모두 보장합니다. 그 결과, 우리는 150만 개 이상의 Process supervision 주석을 수집하여 과정 보상 모델(PRM)을 훈련시킬 수 있다. 이 완전 자동화된 과정 감독과 가중 자기 일관성 알고리즘 fully automated process supervision alongside the weighted self-consistency algorithm을 활용하여 지시 조정된 Gemini Pro의 기존 math 벤치마크에서 성능을 36%를 더 올림 (51% -> 69.4%)
CoT 프롬프팅이 reasoning task을 여러개의 중간단계들로 순차적으로 처리하도록 LLM을 했지만 greedy decoding에 의해 이는 근본적인 제한이 있을수 밖에 없다. 이와 유사하게 질문과 이에 대한 CoT 답변(솔루션) 파인튜닝도 비슷한 맥락
또다른 시도가 verifier로 마지막으로 생성된 답변을 검수하는 역할을 한다(potential mistake, 오류 조정)
하지만 결국 multi-step reasoning같은 수학에서는 별로 (verifier) <-- 모델 정렬에 사용되는 RLFH, reward model
reward model의 대표적인 두가지 타입
1. outcome reward model (ORM)
2. process reward model (PRM)
정렬에 대부분 ORM으로 reasoning의 중간단계에 보상이나 페널티를 부여할수 없음 -> 이를 위해 PRM

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/89801222-6b76-4335-b69a-4d0db65ac5bc)
edge 액션, node 상태 yellow edge가 맞음, blue edge가 틀림

하지만 위와 같은 결국 process supervision은(프로세스 중 지도) supervised 데이터가 필요함. 다단계 reasoning을 각각으로 세세하게 라벨링 -> 이러한 과정을 자동화하기 위해 monte carlo estimation 사용

이 알고리즘으로 1.5백만 데이터셋 생성, LLM reasoning 성능을 높이기 위해 verifier with weighted self-consistency

PRM이 ORM을 능가한다는 증거 (Let’s Verify Step by Step -2023 openai , Math-Shepherd: Verify and Reinforce LLMs Step-by-step without Human Annotations) PRM을 훈련하기 위해 고품질의 과정 감독 데이터 생성이 핵심이며, 이는 비용이 많이 드는 인간 주석(Lightman et al., 2023) 외에도 Math-Shepherd(Wang et al., 2024a)와 MiPS(Wang et al., 2024b)이 몬테카를로 추정을 통해 인간의 참여를 자동화한 데이터 수집 과정을 탐구했으며, 두 연구 모두 큰 성능 향상을 관찰

planning 알고리즘
Tree-of-Thought, Reasoning-via-Planing(https://arxiv.org/abs/2305.14992) ,  inference-time MCTS

메소드
------------------
 process provision : ORM보다 더 정확하고 세밀한 피드백을 제공하며, 오류의 정확한 위치를 식별, 또한 수학 문제 해결 영역에서 잘못된 추론을 완화
 
이러한 장점에도 불구하고, PRM을 훈련하기 위해 각 단계의 정확성에 대한 intermediate signal를 얻는 것은 간단하지 않음. 이전 연구(Lightman et al., 2023)에서는 도메인 전문가(domain expert)를 사용하여 수동으로 레이블을 주석 처리하는 방법에 의존했는데, 이는 스케일하기 어렵다.

구체적으로, 질문 
질문과 처음 t단계의 prefix 솔루션 (1에서 t까지) 을 받아 최종 답에 도달할 때까지 후속 단계를 완료할 수 있는 "완성기(completer)" 정책이 수립됩니다. 

그림 2(a)에 나타난 바와 같이, 솔루션의 어느 단계에서든 completion policy을 사용하여 해당 단계에서 무작위로 
k개의 rollout을 샘플링할 수 있다. 이러한 rollout의 최종 답은 황금 답(golden answer)과 비교, 
k개의 롤아웃 각각에 대한 답의 정확성 레이블이 제공됩니다. 그 후, 
t-번째 단계에서의 총 롤아웃 수에 대한 정확한 롤아웃 수의 비율은 Eq. (1)과 같이 
t까지의 prefix 단계의 "정확성 수준"을 추정합니다. 논리적 추론 시나리오에서는 하나의 롤아웃이라도 정확하다면, 
 ​x(1~t)는 정확하다고 간주

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/c9027317-8c4e-4daa-9c72-a44160533b5a)
 
한 단계 더 나아가, 솔루션의 중간 단계의 정확성을 주석하는 직관적인 전략은 처음부터 끝까지 모든 단계에 대해 rollout을 수행하는 것 이는 Math-Shepherd와 MiPS 모두에서 
그러나 이러한 brute-fore 방식은 많은 정책 call이 필요
이런 주석 효율성을 최적화하기 위해 우리는 이진 검색 기반 몬테카를로 추정을 제안

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/530eb664-a082-4061-9032-f4be93cea1e5)

