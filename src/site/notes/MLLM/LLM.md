---
{"dg-publish":true,"permalink":"/mllm/llm/"}
---

![](https://i.imgur.com/hKAtdSJ.png)
foundation 모델을 self supervise modeling을 한다

![](https://i.imgur.com/6m2qbdg.png)
맨 처음의 MLP쪽 pretraining을 보면 word 하나로 embedding을 해둔다.
bert는 단어 하나만 임베딩하는 word to vector와 달리, 문장 전체를 embedding한다고 생각하면 된다.
문장의 context를 반영함(masked)
language model은 autoregressive하게 학습을 한다.
결국 transformer의 decoder만 쓴게 성능이 좋아서 그쪽으로 발전을 하게 된다.
![](https://i.imgur.com/chj42hP.png)
MLM(BERT)
LM(GPT)

![](https://i.imgur.com/8XMagEF.png)

![](https://i.imgur.com/1R3PdJo.png)
기존 모델과의 차별점은 데이터를 많이 쓰고 트랜스포머 decoder 레이어를 깊게 쌓아서 파라미터를 늘렸다는 점이다.
책 내용을 전부 auto regressive하게 학습했다.

![](https://i.imgur.com/DYnuNtd.png)
데이터를 GPT1의 10배 정도로 늘렸다
10억파라미터가 넘는다.
똑같이 autoregressive하게 학습한다.
데이터와 모델 사이즈가 크게 바뀌었다.

![](https://i.imgur.com/Xg8I9ur.png)
120배의 모델사이즈 14배의 데이터로 늘렸다.

![](https://i.imgur.com/URdJerX.png)
주로 원하는 데이터에 fine tuning해서 많이 썼었는데
GPT3에서는 in-context learning이라는 특성을 발견한다.
학습 없이 몇 가지 예제로 유저가 comment가 있고 긍정, 부정이 있으면 다음 comment가 뭘까? 물어보면 대답을 한다.
language 모델을 키우다 보니, 몇가지 예제로 학습이 되네? (few- shot learners)

![](https://i.imgur.com/d61Hc0k.png)
chat gpt에 기반이 된 논문
GPT3는 질문에 대해 following하는 방법은 없었다. 질문에 뒤에 쭉 나열하는 형식
사용자 질의에 following하는 능력을 주기 위해 수많은 answer pair dataset을 수집해서 학습에 사용한다.
그 결과 instruction-following

![](https://i.imgur.com/sRrhchD.png)
이 때부터 chat gpt가 사용되면서
사람들이 LLM에 관심을 가지게 되었다.
large scale language models..

![](https://i.imgur.com/cy31Cvw.png)

![](https://i.imgur.com/rT64016.png)

![](https://i.imgur.com/lSAexqg.png)

Hallucination과 context understanding문제가 있다.
![](https://i.imgur.com/81KZamZ.png)
![](https://i.imgur.com/yPMm8xz.png)
![](https://i.imgur.com/jDk5dFe.png)
![](https://i.imgur.com/l733Gcy.png)
![](https://i.imgur.com/xK1yTiy.png)
dataset을 먼저 모은다.
다양하게 답변을 LLM을 가지고 뽑는다.
Human anntator한테 보여주고 점수를 매기게 한다.
Lank도 자연스럽게 매겨진다.
수집한 데이터로 reward model도 학습을 한다.
![](https://i.imgur.com/eqNDeDP.png)
![](https://i.imgur.com/QDDbiI7.png)
![](https://i.imgur.com/OIwn9f2.png)
Policy Model이 LLM이라고 생각하면 되고 Reward Model을 태워서 점수가 쭉 나오면 점수를 쪼갠 다음 거기서 다시 preference 데이터가 나오는 과정을 반복한다.
앞의 DPO과정을 온라인으로 옮겨서 loop를 돌린다고 생각하면 된다
![](https://i.imgur.com/mybsryN.png)

