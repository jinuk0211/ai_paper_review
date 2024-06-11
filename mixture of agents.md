![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/5e7b56cb-8b72-4417-a51b-d2eadf11402f)

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/a1016d6f-bb75-48ce-b57b-6b2481529d69)

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/292d702b-5745-4988-9547-ad677a511062)

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/414e80f0-00f7-41ab-b858-1db9a08433c5)

MoE
https://www.threads.net/@rien_n_est/post/C4sNKBZSZ7h/?xmt=AQGzlHPlLpNkhfDa7OO5DkvKo2tL9ZKfwj8RTPlZqg-WbA


mixture of agents (MoA)
MoA - 모델
MoE - 활성화
여러개의 LLM을 하나로
2024년 6월 7일 - togetherAI
arxiv.org/pdf/2…
MoA 모델이 gpt4o보다
alphacaEval 2.0 벤치마크에서 매우 큰 성능 증가를 보임
(그 외 MT-bench, FLASK)
원리 :
첫 레이어의 LLM이 생성한 답변
-> 다음 레이어의 프롬프트로 활용
->이 사이클 반복
중요한점 :
MoA 레어어마다의 적절한 LLM 선택이 중요
기준 :
1.각 레이어의 성능을 metric으로 평가해 다음 레이어의 LLM을 선택
2. 모델 결과값의 다양성 중요
(한 모델의 결과값보다 여러 모델에서의 다양한 response가 결과에 크게 기여)
LLM끼리 협력해서 결과값 생성의 질을 올릴수 있음
이 협력의 장점을 최대화하기 위해 ↓

서로 다른 LLM의 협력에서의 어떠한 측면에서 우수한지 분별
두가지 역할로 나눌 수 잇음
1. proposer
다른 LLM으로의 프롬프트를 생성하는데 우수한 LLM
다양한 관점과 더 긴 context를 참조 response로 생성
2. aggregator
다양한 LLM으로 부터의 response를 수집해 높은 퀄리티의 결과값을 내는
LLM
I개의 층마다 n개의 LLM이 들어가는데
이 n개의 모델이 다 동일하다 = single proposer
다르다 = multi proposer
single propser이여도 temperature에 의해 다양한 결과가 생성됨


i 레이어를 통과한 결과값 y는
각 i 레이어의 agent마다의 결과값을 모아서
합성한 '⊕' (aggregate and synthesize)한 것과
input 프롬프트를 concat '+' 한 값
-> 다음 레이어 i+1로 넘어감
마지막 레이어에는 하나의 LLM으로 충분(결과값을 생성하므로)
MoE에서의 영감을 받음
MoE는 활성화함수 수준
MoA는 모델 수준
threads.net/@rien…
\off the shelf ( 요소가 대체가능) LLM의 프롬프트 기능만에 의존
-> 미세조정 계산 오버헤드 x, 유연성, 확장성 등을 제공
한계 여러 LLM을 거쳐 최종 layer의 input이 되야하므로 결과값의 첫번째 토큰 생성까지의 시간이 오래걸림
