---
{"dg-publish":true,"permalink":"/paper-review/mllm/from-objects-to-event-unlocking-complex-visual-understanding-in-object-detectors-via-llm-guided-symbolic-reasoning/"}
---

![](https://i.imgur.com/pDKBxv3.png)
input 이미지를 이해한 후 reasoning을 통해 LLM으로 recaption을 하기 전, 어떻게 기존 영상을 분석하고 이해해야 할까?
그 방법을 찾던 도중 이 논문을 발견하고 읽게 되었다.

영상이 주어졌을 때, 사람은 "see"하지만 LLM은 "read"한다.
따라서 사람처럼 LLM이 영상을 이해하기 위해 이미지와 텍스트를 연결하는 cross-modality learning이 필요하다.

이 논문을 선정하게 된 이유는 객체 선정, training-free, consistency 라는 키워드 세개에 부합했기 때문이다.

Visual Event Detection with Symbolic Reasoning (VED-SR): training free 이상 탐지 프레임워크
![](https://i.imgur.com/nRGVYDA.png)
VED-SR은 
