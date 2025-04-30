---
{"dg-publish":true,"permalink":"/paper-review/mllm/from-objects-to-event-unlocking-complex-visual-understanding-in-object-detectors-via-llm-guided-symbolic-reasoning/"}
---

![](https://i.imgur.com/pDKBxv3.png)
input 이미지를 이해한 후 reasoning을 통해 LLM으로 recaption을 하기 전, 어떻게 기존 영상을 분석하고 이해해야 할까?
그 방법을 찾던 도중 이 논문을 발견하고 읽게 되었다.

영상이 주어졌을 때, 사람은 "see"하지만 LLM은 "read"한다.
따라서 사람처럼 LLM이 영상을 이해하기 위해 이미지와 텍스트를 연결하는 cross-modality learning이 필요하다.

이 논문을 선정하게 된 이유는 객체 선정, training-free, consistency 라는 키워드 세개에 부합했기 때문이다.
![](https://i.imgur.com/MTep6gl.png)
Vision LLM은 크게 세가지로 나뉜다.
![](https://i.imgur.com/W93bY7K.png)
이 중에서 내가 하고자 하는 부분은 Vision과 Language를 연결해서 visual, textual representation을 결합한 후 시각적 요소를 텍스트 설명과 연결시키는 것이다.
![](https://i.imgur.com/rz7vlak.png)
이 논문의 초록에는 object detector를 단순히 object detection에만 쓰지 않는다. LLM기반 symbolic reasoning을 통해서 복잡한 event understanding을 가능하게 한다.
이 논문의 novelty는 od와 event understanding을 따로 수행하지 않아서 semantic gap을 해소한다는 점과 task specific한 훈련 없이 event understanding을 할 수 있다는 점이다.
![](https://i.imgur.com/nRGVYDA.png)

