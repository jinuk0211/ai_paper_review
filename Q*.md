heuristic value로 이제 optimal Q-function = h(s)
g(s)로 아래와 같은 수식을 사용하는데

이때 Agg는 보상들을 집계하는 함수로, 가능한 것은 min, max, sum, [−1] 등이 있고

min: 최소 보상
max: 최대 보상
sum: 보상의 합
[−1]: 마지막 상태의 보상을 총 효용으로 사용

-> 이를 통해 초기상태에서 t까지의 reward를 요약(summarize)

R^p는 process-based reward function, 즉 PRM으로 

 다음과 같은 방법으로 학습될 수 있다:

인간 피드백: 인간이 제공하는 피드백을 통해 학습

실제 데이터: 실제 정답 데이터를 사용하여 학습 (PRM800K로 학습했다함)

규칙: 미리 정의된 규칙을 사용하여 학습

reasoning 단계의 로짓: 로짓(logits)은 LLM(대형 언어 모델)의 reasoning 단계에서의 confidence를 반영


------------------------

이때 모든 가능한 다음 reasoning step를 열거하는 것은 실행 불가능(intractable)하므로

실제로는 LLM이 생성 모든 단계 후보(all step candidates) 중 상위 K개의 alternatives으로 제한한다. 따라서 식은 다음과 같고
![image](https://github.com/user-attachments/assets/8c98daa4-1f4e-40eb-bc93-bc2bcda24843)

결국 frozen 된 LLM 정책 를 사용할 때 상태-행동 쌍의 최적 Q-값 Q*를 추정하는 것이 목표인데

 이 LLM 정책은 주어진 추론 문제에 대해 최적이 아닐 수 있다. 따라서, 여기서는 Q-value model(QVM)를 학습하여 Q*를 근사한다.

이를 위한 손실함수는 
아래와 같은데
즉 Q-값과 y_hat(라벨)을 비교하자면

Q-값 
현재 모델이 예측한 Q-값

라벨 
이상적인 Q-값에 대한 근사( 실제 보상과 미래 보상의 합)

를 의미한다

즉 모델(QVM)은 다양한 상태와 행동에 대해 예측한 Q-값을 개선되는 방향으로 학습을 진행한다

![image](https://github.com/user-attachments/assets/78d8c337-911d-41c4-b14b-6638f75337d0)


--------------------------------------------------------------------------

이 y_hat 라벨을, 즉 최적 Q-값을 근사하기 위한 방법에 3가지를 제시한다
1. Offline reinforcement learning
2. Approximating optimal policy with stronger LLMs.
3. Learning from rollout.

![image](https://github.com/user-attachments/assets/08925a72-f56d-49a3-8f4a-3cf15c22fbe9)

![image](https://github.com/user-attachments/assets/0fb1eabc-357a-4d9a-b375-9eb51a007173)

![image](https://github.com/user-attachments/assets/8098fe7c-cc12-496c-b1a7-cb976cc27f7f)

--------------------------------------------

일단 이렇게 Q value​ 모델을 얻으면, 이를 f(n) 을 계산하는 데 사용할 수 있다. 그런 다음 A* 알고리즘을 사용하여 최상의 경로를 탐색한다

초기화: 탐색할 상태 후보를 저장하는 집합 unvisited는 입력 질문 만 포함하고, 방문한 상태를 기록하는 집합 visited를 초기화

탐색 과정: 매 단계에서 f()값이 가장 큰 상태 state를 선택하고, LLM(policy)로부터 top-K 대안을 query하여 확장

업데이트: 두 집합 visite와 unvisited를 업데이트하고, 이 과정을 terminal 상태(완전한 경로)에 도달할 때까지 반복

결과 추출: 최종 상태 s의 답변 부분을 결과로 추출

이러한 방식으로 최적의 Q-값을 근사하고, 이를 사용하여 추론 문제를 해결하는 최적의 경로를 찾는다.

![image](https://github.com/user-attachments/assets/11d289e8-de60-4059-82b2-dff78436bec1)


--------------------------------

실험 디테일
detail

GSM8K 수학 데이터셋의 경우

aggregation으로 min 

reward 측정을 위해 process reward model, PRM800K으로 훈련된 PRM 사용

LLM의 생성해낸 한줄을(line)을 action으로 생각

f(state) 값 계산시 discount factor, λ = 1

각각의 step마다 K = 6으로 확장

rollout으로부터 훈련

수학적 reasoning에는 τ = 0.9, 코드 생성에는 τ = 0.2(temperature)를 사용해 완전한 (trajectory)를 얻고 이를 "/n"을 이용해 각각의 trajectory를 일련의 state로 분할

즉 LLM을 사용하여 랜덤 롤아웃 또는 MCTS를 수행하여 trajectory Pool P를 수집

누적 보상이 가장 높은 최상의 추론 path를 선택하여 현재 state-action 쌍의 해당 Q-값 label을 구성 (Q value model 데이터셋으로 사용)









