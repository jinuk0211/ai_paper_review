1. Video Is Worth Thousands of Words
2024년 6월 10일

2. Image is Worth 32 Tokens 
2024년 6월 11일 - bytedance

3. autoregressive Model beats Diffusion
2024년 6월 10일

1.
이미지 토크나이져의 중요성 (이미지 -> latent space)
TiTok (Transformer-based 1-Dimensional Tokenizer)라는 걸 사용

classification , object detection, segmentation, multi-modal large language
models <-- Unet 비스무리한(enco-deco) tokenizer-detokenizer 이 사용된다하면
detokenizer은 사실상 쓸모가 없지 않나??

이유 
이미지(input)에서 이미지(output)로가 아닌 이미지(input)에서 특정한 포맷, 구조로 결과값이 나옴 

https://arxiv.org/pdf/2406.06040
https://arxiv.org/pdf/2406.07550
DiT-XL/2의 SOTA 성능 능가, 74배 빨리
근본적인 한계 
원래 이미지의 패치가 latent 토큰의 이미지 정보 위치와 같음
맨 위 왼쪽 패치 = 맨 위 왼쪽 latent token 
https://arxiv.org/abs/2406.06525
