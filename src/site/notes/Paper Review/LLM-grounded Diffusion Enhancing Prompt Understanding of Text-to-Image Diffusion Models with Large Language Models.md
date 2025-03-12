---
{"dg-publish":true,"permalink":"/paper-review/llm-grounded-diffusion-enhancing-prompt-understanding-of-text-to-image-diffusion-models-with-large-language-models/"}
---

GRIGEN: 레이아웃을 줌

LLM layout generator(생성) 어케하는지

#### abstract
recent text-to-image diffusion model: struggle with ==complex prompt==(involve ==numeracy and spatial reasoning==)

propose to enhance prompt understanding capability in diffusion model
grounded generation two-stage process <- pretrained LLM

stage 1: LLM generate scene layout -> captioned bounding boxes(given prompt describing the desired image)
stage 2: novel controller -> off-the-shelf diffusion model -> layout-grounded image generation

doubling the generation accuracy across four tasks on average

#### Introduction
SD, SDXL 겪는 어려움: 특정 수 object 생성, prompt 내 neg 이해, spatial reasoning과 속성을 object와 연관

포괄적 multi-modal데이터셋(복잡한 캡션 포함됨)데이터셋 수집해서 text-to-image diffusion model 훈련시켜 prompt 이해력 향상
![](https://i.imgur.com/BfbwR9K.png)
SDXL(왼)과 LMD(오)가 prompt type에 따라 어떻게 처리하는지 보여준다.
negation에는 SDXL 바나나 없애라고 했는데 안없앴고 LMD은 잘했다.
attribute blending에는 SDXL은 남자빨간옷 여자파란옷 못하고 LMD는 잘했다.
generative numeracy에는 SDXL는 네마리 그리고 LMD는 잘했다.
spatial relationship에는 SDXL는 L자 모양으로 못했고 LMD는 잘했다.
![](https://i.imgur.com/U2BtCKj.png)
Stage 1: LLM Layout Generator
	prompt -> image layout
	layout: caption & 경계 보여주는 bounding box 형태(레이아웃 정보)
	LLM을 text-grounded layout generator로 adapt함.
	원하는 이미지 설명하는 prompt가 주어지면 배경과 negative prompt(생성 피해야하는) 캡션이 있는 bounding box 형태로 scene layout을 생성한다.
Stage 2: Layout-grounded SD
	layout 정보 바탕으로 SD가 이미지 생성
	

==training-free== 향상된 prompt understanding을 위해 grounding을 제공하는 LLM으로 diffusion model을 장비하는 방법
LMD는 두 단계 generation으로 구성

==off-the-shelf==(특정한 추가 훈련 없이 즉시 사용할 수 있는 모델): LLM이나 diffusion model parameter optimization 없이 독립적으로 훈련된 LLM, diffusion model에 적용 가능

추가 학습 없이도 base diffusion model이 지원하지 않는 언어로부터 프롬프트를 사용한 이미지 생성이 가능



