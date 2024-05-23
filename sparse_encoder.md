SAE decomposing polysemanticity
---------------------------
superposition 중첩, polysemanticity 다중의미
residual stream - 이전 layer의 결과값들을 합쳐놓은 것을 의미
![Uploading image.png…]()

검은 상자를 열어보더라도 반드시 도움이 되는 것은 아닙니다.
모델의 내부 상태, 즉 모델이 응답을 작성하기 전에 "생각하고" 있는 것은 명확한 의미 없이 긴 숫자 목록("뉴런 활성화")으로 구성되어 있습니다. 
클로드와 같은 모델과 상호작용해보면 모델이 다양한 개념을 이해하고 활용할 수 있음을 알 수 있지만, 뉴런을 직접 살펴보면 그 개념을 파악하기 어렵습니다. 
사실 각 개념은 많은 뉴런에 걸쳐 표현되고, 각 뉴런은 많은 개념을 표현하는 데 관여합니다.
이전에는 뉴런 활성화의 패턴인 특징(feature)을 사람이 해석 가능한 개념과 매칭하는 데 어느 정도 진전이 있었습니다. 
우리는 "사전 학습(dictionary learning)"이라는 기법을 활용했는데, 
이는 전통적인 머신러닝에서 차용한 것으로 다양한 컨텍스트에서 반복되는 뉴런 활성화 패턴을 분리합니다.
그러면 모델의 내부 상태를 많은 활성 뉴런 대신 몇 가지 활성 특징으로 표현할 수 있습니다. 
영어 단어가 문자를 조합해서 만들어지고, 문장이 단어를 조합해서 만들어지듯이, 
AI 모델의 모든 특징은 뉴런을 조합해서 만들어지고, 모든 내부 상태는 특징을 조합해서 만들어집니다.

page 
----------------
독일어 특화 뉴런은 프랑스어 텍스트를 볼때 활성화되지 않음 -> superposition free model? -> MoE가 그런 경향이 있음 -> 뇌의 섹터 분할과 비슷함
Inception v1, a single neuron responds to faces of cats and fronts of cars 
[1]
. In a small language model we discuss in this paper, a single neuron responds to a mixture of academic citations, English dialogue, HTTP requests, and Korean text. Polysemanticity makes it difficult to reason about the behavior of the network in terms of the activity of individual neurons.

page 4 superposition에서 feature 찾기
------------------
 (1) 활성화 희소성을 장려함으로써 중첩 없이 모델을 만드는 것, (2) 중첩을 나타내는 모델에서 과잉 완전한(feature overcomplete) 특징 기초를 찾기 위해 사전 학습(dictionary learning)을 사용하는 것, (3) 두 가지를 결합한 혼합 접근법을 사용하는 것. 이 연구가 출판된 이후, 우리는 이 세 가지 접근법을 모두 탐구했습니다. 결국, 희소한 구조적 접근법(접근법 1)이 다의성(polysemanticity)을 방지하기에 충분하지 않다는 것을 설득하는 반례들을 개발했고, 표준 사전 학습 방법(접근법 2)은 과적합 문제로 인해 상당한 문제를 겪고 있음을 알게 되었습니다.

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/afb3e608-defd-4a94-adde-ec182e06e374)
