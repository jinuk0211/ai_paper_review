Layer-Condensed KV Cache for Efficient Inference of Large Language Models
----------------------------
https://arxiv.org/pdf/2405.10637

You Only Cache Once: Decoder-Decoder Architectures for Language Model
--------------------------
https://arxiv.org/pdf/2405.05254
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/515a33c0-5fb0-48a2-b3f9-bcca3558d3dc)

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/9086a806-1949-40d7-a2ad-f7e2d2e5a158)


Reducing Transformer Key-Value Cache Size with Cross-Layer Attention
--------------------------
https://arxiv.org/pdf/2405.12981

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/888a9b89-7825-43ef-a05a-0f6d659510f7)

트랜스포머 autoregressive LLM 에서 kv cache는 decoding과정을 빠르게 하는데 중요한 역할
하지만 큰 배치 사이즈와 긴 context에서 메모리 문제가 심각 
이를 위해 GQA, MHA로 query가 key, value 공유(share)
-> 이를 넘어서 인접한 레이어 간에도 key,value를 공유 

accuracy 감소 없이 kv cache 사이즈를 2배 줄임
![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/c801f97b-eb68-4e03-b77f-82b59589b94c)

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/276811b9-7a05-4207-8f25-c2dc7da31fec)
