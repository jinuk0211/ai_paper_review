TimesFM

모든 시계열 데이터를 위한 foundation model
https://arxiv.org/pdf/2310.10688 - google

각 데이터셋 마다의 SOTA모델들의 accuracy와 거의 비슷함

웹 검색 query와 위키백과 페이지 방문수(visits)로부터 주로 얻은 실제 시계열 데이터와 합성 데이터를 모두 활용해 구축한 대규모 시계열 corpus로 학습

input patching과 디코더 스타일 어텐션 아키텍처를 사용하여 이 시계열 corpus에서 효율적으로 사전 학습가능하게 함 - PatchTST와의 차이점

다음의 패치의 예측은 모든 과거 패치들의 function(LLM의 context window처럼 실행됨)

input patching(A time series is worth 64 words)

또한 auto regressive하게 추론해 다단계로 추론보다 seq_pred_len, horizon을 정해서 결과를 얻음(실제로 예측관련 성능이 뛰어남)
