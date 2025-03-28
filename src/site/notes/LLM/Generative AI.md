---
{"dg-publish":true,"permalink":"/llm/generative-ai/"}
---

Generative AI, 특히 최근 급격한 발전을 보이고 있는 Large Language Models(LLMs), 이미지 생성 모델, 그리고 Stable Diffusion과 같은 기술들
LLM의 작동 원리, Reinforcement Learning from Human Feedback (RLHF)의 개념, Safety and Helpfulness를 보장하는 방법, Parameter Efficient Fine Tuning (PEFT) 기법
이미지 생성 모델들의 기초부터 고급 활용 방안까지 탐구하며, 실제 실무에서 이러한 기술들이 어떻게 창의적이고 혁신적인 문제 해결에 기여

- LLM
    - 인간과 유사한 언어 이해 및 생성 능력을 가진 인공지능 모델로, 자연어 처리에서 다양한 과제를 수행할 수 있습니다.
- Diffusion Models
    - 데이터의 분포를 학습하여 고품질의 새로운 샘플을 생성할 수 있는 딥러닝 기반 이미지 생성 모델입니다.
- PEFT
    - 기존 모델의 구조를 크게 변경하지 않으면서도 특정 작업에 대해 모델을 효율적으로 미세 조정할 수 있는 기술입니다
- Stable Diffusion
    - 사용자가 제공한 텍스트 프롬프트를 기반으로 고해상도 이미지를 생성할 수 있는 최신 이미지 생성 모델로, 창의적인 콘텐츠 생성에 혁신을 가져왔습니다.

1. Generation AI Overview
    1. Vision, NLP 분야에서의 생성형 AI 모델들의 발전 방향을 간단히 이해한다.
    2. 생성형 이미지 모델들의 유형을 이해한다.
    3. 생성 모델 활용 사례(ChatGPT, DALL-E 3)를 간단히 이해한다.
2. LLM Pretrained Models
    1. LLM의 구조 및 사전학습 방법을 이해한다.
    2. Safety와 Helpfulness 측면에서의 LLM의 생성방향과 Instruction Tuning에 대해 이해한다.
    3. Supervised Fine Tuning 과 RLHF 에 대해 이해한다.
3. Parameter Efficient Tuning
    1. LLM의 발전 과정 및 학습 방법론의 발전 과정을 이해한다.
    2. Adapter, Prefix Tuning 기법에 대해 이해한다.
    3. Prompt  Tuning, Low-Rank Adaptation 기법에 대해 이해한다.
4. sLLM Models
    1. Open Source LLM 과, LLM이 갖는 License problem에 대해 이해한다.
    2. Alpaca류 모델들과, Self-Instruct 방법에 대해 이해한다.
    3. LLM의 평가 방법에 대해 이해한다.
5. Image Generation Models Overview
    1. GAN 계열의 이미지 생성 모델들의 원리에 대해 이해한다.
    2. Autoencoder 계열의 이미지 생성 모델들의 원리에 대해 이해한다.
    3. Diffusion 계열의 이미지 생성 모델들의 원리에 대해 이해한다.
6. Image Generation 2 : Stable Diffusion & Evaluation
    1. Stable Diffusion 모델의 구조와 학습 및 생성 과정에 대해 이해한다.
    2. Stable Diffusion 2, SDXL, SDXL Turbo의 작동 원리에 대해 이해한다.
    3. 생성된 이미지의 평가 방법에 대해 이해한다.

- [ ] 4강, 6강 강의를 수강한 이후 실습-1,2 와 실습-3,4를 수강

## Text Generation
- 최근 Large Language Model은 우수한 능력과 다양한 활용으로 많은 관심을 받고 있습니다.
- Large Language Model은 그 이름처럼 학습 데이터와 모델 크기를 막대하게 증대하여 학습한 모델입니다.

- Large Language Model 학습 코퍼스의 품질은 어떻게 측정할 수 있을까요?
    - 좋은 코퍼스란 무엇인지 정의가 필요합니다. 
    - 전처리 과정에서 삭제/수정해야 하는 데이터가 무엇일까요?
    - 욕설 및 편향 데이터를 학습 코퍼스에서 완전히 제외하는 것은 모델에게 욕설/편향의 개념을 학습하지 못하도록 합니다. 
    - 코드, HTML, 테이블 데이터 등 다양한 데이터를 포함해야 합니다. 
    - 코퍼스의 품질을 정량화할 수 있는 지표가 있는지 찾아봅시다.

#### Instruction Tuning
SFT: Supervised Fine-Tuning
DPO: Direct Preference Optimization

파라미터 수가 많은 Large Language Models(LLM)을 효율적으로 학습할 수 있는 Parameter Efficient Fine-Tuning(PEFT) 방법론을 탐색
LLM의 발전으로 모델 크기가 증가함에 따라, 기존 학습 방법론의 가진 한계점을 살펴봄
PEFT의 다양한 접근 방식인 Adapter, Prefix Tuning, Prompt Tuning, 그리고 LoRA를 통해 모델의 성능을 최적화하는 전략과 기법을 심층적으로 다룸

- 최근에 제안된 새로운 방법론들은 기존 PEFT 방법들을 어떻게 활용하고 있을까요?

2024.12.03
- [x] 멘토링

아이클리어는 모든 리뷰가 오픈되어있음
리부탈: 이니셜한 리뷰가 온다. 리뷰어들이 니 점수는 이 점수고.. 이유는 ~이다.
![](https://i.imgur.com/r14rbvW.png)
결국 아이클레어에서 만점을 받음 ㄷㄷ

#### runway
Frames
풍을 만들어준다.. 미장셴 풍
메이크업 풍
스타일리스틱한게 들어갈 수 있다
컨텐츠는 이미 정해져있는 상황에서 ...~상황이였는데
이건 

