GPT 4o - part 1

3개 이상의 modality Mirasol3B - 2024년 4월 3일자 (구글 딥마인드)
https://arxiv.org/pdf/2311.05698

카멜레온 - 2024년 5월 16일자 (Meta)
https://arxiv.org/pdf/2309.02591 https://arxiv.org/pdf/2405.09818

vlm을 만들때 중요한것 - 2024년 5월 3일자 (huggingface)
https://arxiv.org/pdf/2405.02246

gemini 1.5 https://arxiv.org/abs/2403.05530
chameleon
기존의 vlm

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/68bdf539-8118-4957-a3fe-48174c158afb)

카멜레온
humaneval에서의 GPT-4V , gemini pro 능가
https://arxiv.org/pdf/2309.02591
https://arxiv.org/pdf/2405.09818
early fusion model , 기존의 VLM처럼 vision encoder를 써서 이미지의 상태 표현 공간을 만드는게 아닌 tokenizer로 이미지와 텍스트를 처리해 바로 autoregressive LLM에 feed하는 방식

input말고도 output 또한 interleaved 되있는 것을 알수 잇다.

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/27c49fc7-d39a-4b4d-bd98-92fd0c4d4995)

![image](https://github.com/jinuk0211/ai_paper_review/assets/150532431/469001cd-f8d2-4353-a701-baff26cfff43)

miralsol3b
비디오와 오디오는 텍스트보다 더 많이 얻어짐(obtained at higher rate), 시간에 따라 정렬됨

대부분 global context로써 제공되는 텍스트와 일치하지 않는다
예를 들어 비디오의 제목이나 어떤 묘사는 비디오의 전체 프레임들과 일치하지 않는다

또한 비디오 및 오디오 input은 메모리가 훨씬 크며, 비디오 길이가 증가함에 따라 증가하므로 이러한 모달리티에 더 많은 컴퓨팅 리소스가 필요하고 long-range dependancies 모델링이 더 어려워짐

->> 시간을 고려할 수 잇는 요소를 가진 멀티모달 모델을 제안
