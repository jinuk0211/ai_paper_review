world model - Google deepmind
2024 / 02 / 23
11억 파라미터
200,000시간 게임 플레이 영상 학습
텍스트,이미지 프롬프트로 interactive 게임 환경을 만듦
기본 architecture
LAM + video tokenizer -> MaskGIT(decoder only)
결론 : 게임영상을 학습시켜 모든 이미지, 텍스트를 간단한 게임으로 만드는 model
[arxiv.org/pdf/2…](https://arxiv.org/pdf/2402.15391)

SORA의 경우 text + video
genie는 action + video
ViT 아키텍쳐 components 사용, 메모리 효율성을 위해 spatiotemporal (ST) transformers 사용
novel video tokenizer - 비디오 토큰
VQ-VAE with ST
youtu.be/wcqLF…
causal action model - 잠재 액션
이전 프레임의 액션을 이용해 다음 프레임을 예측하기 위해 action annotation이 필요 근데? 돈이 많이듬. 그래서 unsupervised manner로
latent action model사용
비디오 토큰, 잠재액션들을 MaskGIT 를 통과시켜 다음 프레임을 예측 - 스테이블 디퓨전과 유사
조작은 os copilot 같은 ai agent 형식? 로봇공학에도 로봇의 action결정에 사용 가능
