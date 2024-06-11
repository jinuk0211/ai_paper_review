bitsfusion 

Stable Diffusion v1.5 의 Unet의 메모리를
FP16 (1.72 GB) ->1.99 bits (219 MB)

2024년 6월 6일 - snap

https://snap-research.github.io/BitsFusion/

시사하는바 
1. 대부분이 4bit 에 연구, 그럼에도 여기의 1.99bit이 더 좋은 퍼포먼스
2. 크기가 작은 디퓨전모델에서의 시도가 대부분

각 레이어의 서로 다른 양자화(2bit,3bit,4bit)로 인한 에러
-> quantization-aware training (QAT) 진행

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/4ca53458-f56c-4d9b-82b5-549e628b540c)
