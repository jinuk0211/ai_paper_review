현재의 connectionism-based 딥러닝에서의 SSM에 관해
또한 linearzed 트랜스포머에 관해

논문1
State Space Model survey
논문2 
linearized LLM

SSM survey
1. introduction
transformer 아키텍쳐는 놀라운 성능을 입증해옴 하지만 요구하는 컴퓨팅이 많은 연구자들을 단념시켯다
더 효율적인 방법을 찾기위해 최근에 관심을 받고 있는것이 SSM인데 이 논문에서는 이에 대한 comprehensive한 리뷰를 제공한다

SSM은 딥러닝에서 보통 선형 불변(linear invariant), 즉 정적인 시스템으로,
기본적으로 원래의 연속적이고 동적인 시스템 모델링하기 위한 제어 이론과 컴퓨터 뇌과학에서의 SSM을 
RNN(반복)과 CNN(합성곱)으로 discretize해 컴퓨터가 처리할 수 있도록 하게함

2. FORMULATION OF SSM
SSM은 기본적으로 kalman filter에서 유래됬는데 input signal U(t)를 받아 잠재상태 X(t)에 이를 매핑하고
output signal y(t)로 이를 project(linear)
A,B,C,D는 차례로
state matrix, input matrix, output matrix, feed-forward matrix 를 나타냄.
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/2b32d1c9-c059-4310-9ebc-d069eaae8786)
이를 ZOH로 discretize하는데 RNN과 유사한 형태가 됨.
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/818eb00d-71e2-4505-8c27-497884e4ad38)
RNN의 문제인 병렬화 불가능 문제가 발생함. 이런 딜레마를 해결하기 위해 이 식을 확장해 각 y값을 구하기 위한 multiplier을 커널로써 활용함. 이를 통해 CNN 형식의 병렬화를 가능하게 됨

여기까지가 기존의 SSM이고 트랜스포머의 어텐션 레이어 같은 similarity(context 정보가 저장됨)를 계산하는 module이 없기에 문맥을 학습하는데서 저조한 성능을 보임(contetual learning)

->
1. selective scan operation : 모델이 관련된 정보를 필터링 할 수 있게함 -  ∆, B, and C 가 input에 대한 함수
2. 병렬 scanning, kernel fusion, recomputing

3 STATE SPACE MODEL
RNN과 유사하게 긴 시퀸스를 처리할때 vanishing/ exploding gradient의 문제를 겪는데 이를 
  Recurrent Memory (반복 기억, RNN)와 Optimal Polynomial Projections(데이터를 다항식으로 변환하여)의 개념을 합쳐 Hippo <-- recursive memory 성능을 매우 올림(long-range dependency에 효과적)
  ![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/f5e123a4-67b1-42ef-8b2c-4f109195957f)

S4 - Legendre 다항식 , 푸리에 변환 , HiPPO framework
RWKV, Vision-RWKV, RetNet, Mega, H3 도 SSM의 한 형태로 볼수 있음

MAMBA의 등장이후의 연구들

3.1 자연어
GSS(Gated State Space)라는 새로운 방법 
이는 장문 시퀀스 모델링에 사용될 수 있으며 참여자 수를 효과적으로 줄임
실험 결과는 DSS(diagnol)보다 2-3배 빠른 성과를 달성한다는 것을 보여줌

Grazzi 등은 Mamba를 간단한 함수 추정 및 컨텍스트 학습 과업에서의 자연어 처리에 활용하고, 전반적인 성능이 S4 버전보다 우수하며 다른 Transformer 네트워크와 비교 가능하다는 것을 입증합니다. S4++ [46]는 S4 아키텍처의 두 가지 문제점인 비정상적인 상태(NSS)와 종속성 편향을 찾아내고 현재 상태에 다중 상태 정보를 통합하기 위해 State Memory Reply (SMR) 메커니즘을 제안합니다. 또한, 상호작용하는 교차-주의 메커니즘을 통해 복잡한 종속성 편향을 통합하고, 다양한 시퀀스 모델링 과업에서 S4보다 S4++가 우수한 성능을 보여주는 중요한 성과 향상을 시연합니다

3.3 컴퓨터비젼, 3.4 그래프 3.5 멀티모달 생략

3.7 시계열 데이터
SSM은 sequence 모델이기 때문에 multivariate 시계열 데이터를 처리하는 데 SSM을 적용하는 것이. 특히, 장기 시간대 시계열 예측(LTSF) 과업에서의 주요 도전 과제는 장기 의존성 관계를 포착하는 어려움과 선형 확장성의 부족에 있습니다. TimeMachine [180]은 이러한 문제를 해결하기 위해 Mamba를 활용하여 다변량 시계열 데이터에서 장기 의존성을 캡처하는 방법을 소개합니다. 다중 Mamba 모듈이 통합된 아키텍처를 활용함으로써 TimeMachine은 채널 혼합 및 채널 독립성과 관련된 도전 과제를 효과적으로 해결합니다. 이 접근 방식은 서로 다른 스케일에서 전역 및 로컬 맥락 정보를 선택적으로 예측할 수 있게 합니다. 실험적 검증은 TimeMachine이 우수한 확장성을 유지하면서 정확도를 크게 향상시킨다는 것을 보여줍니다.
5. 직면한 문제와 또다른 기회
GPU 사용 측면에서 SSM의 장점: 우리의 실험에 따르면, 몇몇 후속 작업에서 메모리 소비가 Transformer 네트워크와 비교하여 낮거나 비슷했습니다. 이 측면에서 상당한 개선이 관찰되었지만, 일부 작업에서는 그렇지 않았습니다. 더 낮은 GPU 메모리 소비를 탐색하는 연구가 더 필요합니다.
고해상도 또는 장기 비전 데이터에서의 장점 탐구: SSM 아키텍처는 이론적으로 모델의 복잡성을 크게 줄이기 때문에, 고해상도 데이터(원격 감지 데이터, X-ray 의료 이미지)나 장기 시퀀스 데이터(장기 비디오 프레임)에서의 모델링 능력은 매우 가치가 있습니다. 그러나 이러한 측면은 Transformer 네트워크와 같은 다른 강력한 모델을 사용해도 잘 해결되지 않습니다.
SSM 아키텍처를 사용한 사전 학습된 대형 모델: 딥 뉴럴 네트워크의 확장은 일반 인공지능을 위한 중요한 단계입니다. 현재의 대형 모델은 주로 CNN 또는 Transformer 네트워크를 기반으로 구축되며, SSM 아키텍처를 채택한 모델은 드뭅니다. 최근 AI21Labs에서 출시한 Jamba [136]는 Transformer, Mamba, MoE(Mixture-of-Experts)를 융합한 새로운 대형 언어 모델로, 최대 256K 토큰의 컨텍스트 길이를 지원하며 Mixtral-8x7B [266] 및 Llama-2 70B [225]와 비교 가능한 성능을 달성했습니다. 순수한 Mamba 또는 하이브리드 아키텍처를 구축하는 연구는 매우 유망한 방향이 될 것입니다.
SSM 아키텍처를 사용한 멀티모달 학습: 초기 멀티모달 관련 연구는 모달리티별 및 모달리티 공유 표현을 학습하는 방법에 초점을 맞췄습니다. Transformer 네트워크의 영향을 받아, 현재의 멀티모달 알고리즘은 여러 단서를 통합하여 통합된 Transformer 네트워크에서 직접 인코딩하고 융합하는 경우가 많습니다 [267], [268]. 따라서 추론 단계에서의 비용이 단일 모달리티만 사용하는 경우에 비해 두 배가 될 수 있습니다. 비용 민감한 멀티모달 학습을 위한 새로운 SSM 기반 백본을 설계하는 것은 중요한 연구 주제입니다.
SSM을 위한 새로운 스캔 연산자 개발: 스캔은 SSM 아키텍처의 핵심 연산자이며, 1D 및 2D 데이터는 일반적으로 다른 스캔 메커니즘으로 처리됩니다. 예를 들어, VMamba [60]는 CSM(스캔 확장)을 사용하여 이미지를 스캔하고, 네 개의 출력 특징을 최종 2D 특징 맵으로 병합합니다. 더 특수한 원격 감지 데이터를 처리하기 위해 일부 연구자들은 기울어진 특징 표현을 포착하여 더 포괄적인 특징을 얻기 위한 추가 스캔 메커니즘을 제안했습니다 [139]. 다양한 스캔 방식의 비교는 그림 8에서 확인할 수 있습니다. 따라서 SSM의 특징 학습을 향상시키기 위해 새로운 스캔 방식을 설계하는 것이 자연스럽습니다. 예를 들어, 포인트 클라우드와 이벤트 스트림을 더 잘 인코딩하기 위한 새로운 트랙 변경 스캔 방법을 개발하는 것이 가능합니다.
SSM의 일반화 성능: CNN과 Transformer의 제한된 수용 범위와 더 큰 복잡성과 비교할 때, SSM은 선형 복잡성과 글로벌 수용 범위를 가지고 있어 도메인 일반화 분야에서 더 큰 장점과 잠재력을 가질 수 있습니다. 그러나 현재의 SSM 기반 네트워크는 제한된 도메인 일반화 능력을 보여줍니다. Long 등 [120]은 숨겨진 상태와 부적절한 스캔 메커니즘의 관점에서 이 문제를 해결하기 위해 Hidden State Suppressing (HSS) 및 Semantic-aware Patch Refining (SPR) 전략을 제안했습니다. 우리는 도메인 일반화의 전반적인 성능을 더욱 향상시키기 위해 더 많은 통찰력과 개선이 필요하다고 믿습니다.
기존 딥 뉴럴 네트워크 모델에 최신 SSM 모델을 적용: 딥 러닝의 세 번째 물결 초기 단계에서는 지식 증류, 피라미드 구조, 네트워크 인 네트워크 [269], 확산 모델, GAN 등 많은 창의적인 뉴럴 네트워크 모듈이나 디자인이 제안되었습니다. 이러한 성공적인 모듈을 기반으로 SSM을 강화하거나 이러한 모듈에 SSM을 도입하면 더 나은 성능을 얻을 수 있습니다.
