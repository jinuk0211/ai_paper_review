### gpt 4o 

카멜레온 - Meta 2024년 5월 16일자
https://arxiv.org/pdf/2309.02591
https://arxiv.org/pdf/2405.09818

Mirasol3B - google Deepmind 2024년 4월 3일자
https://arxiv.org/pdf/2311.05698

vlm을 만들때 중요한것 - huggingface 2024년 5월 3일자
https://arxiv.org/pdf/2405.02246

gemini 1.5 
https://arxiv.org/abs/2403.05530


#### miralsol3b
-------------
비디오와 오디오는 텍스트보다 더 많이 얻어짐(obtained at higher rate), 시간에 따라 정렬됨

대부분 global context로써 제공되는 텍스트와 일치하지 않는다
예를 들어 비디오의 제목이나 어떤 묘사는 비디오의 전체 프레임들과 일치하지 않는다

또한 비디오 및 오디오 input은 메모리가 훨씬 크며, 비디오 길이가 증가함에 따라 증가하므로 이러한 모달리티에 더 많은 컴퓨팅 리소스가 필요하고 long-range dependancies 모델링이 더 어려워짐

->> 시간을 고려할 수 잇는 요소를 가진 멀티모달 모델을 제안
