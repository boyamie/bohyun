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