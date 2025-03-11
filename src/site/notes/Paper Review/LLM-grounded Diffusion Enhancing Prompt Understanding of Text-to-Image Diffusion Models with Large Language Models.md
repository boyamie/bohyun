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

