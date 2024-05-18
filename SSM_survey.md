현재의 connectionism-based 딥러닝에서의 SSM에 관해

1. introduction

transformer 아키텍쳐는 놀라운 성능을 입증해왔다. 하지만 요구하는 컴퓨팅이 많은 연구자들을 단념시킨다.
더 효율적인 방법을 찾기위해 최근에 관심을 받고 있는것이 SSM인데 이 논문에서는 이에 대한 comprehensive한 리뷰를 제공한다

SSM은 딥러닝에서 보통 선형 불변(linear invariant), 즉 정적인 시스템으로,
기본적으로 원래의 연속적이고 동적인 시스템 모델링하기 위한 제어 이론과 컴퓨터 뇌과학에서의 SSM을 
RNN(반복)과 CNN(합성곱)으로 discretize해 컴퓨터가 처리할 수 있도록 하게했다.

2. FORMULATION OF SSM
SSM은 기본적으로 kalman filter에서 유래됬는데 input signal U(t)를 받아 잠재상태 X(t)에 이를 매핑하고
output signal y(t)로 이를 project(linear)한다.
A,B,C,D는 차례로
state matrix, input matrix, output matrix, feed-forward matrix 를 나타냄
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/2b32d1c9-c059-4310-9ebc-d069eaae8786)
