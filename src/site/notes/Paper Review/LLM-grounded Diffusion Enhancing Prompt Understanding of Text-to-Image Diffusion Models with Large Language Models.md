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

LMD는 한번에 복잡한 프롬프트에서 정확한 고품질 이미지 생성을 가능하게하는 프롬프트 이해 통합 솔루션을 제공한다.

![](https://i.imgur.com/Yv7CqnH.png)
LMD는 instruction-based multi-round scene specification을 가능하게 한다.
instruction-based multi-round scene specification은 user의 followup instruction과 clarification에 따라 생성의 subsequent round 적용가능하게 함.

#### Related Work
**Text-to-image diffusion models**
object에 attribute 결합과 spatial reasoning같은 복잡한 프롬프트 처리할 때 unsatisfactory performance

**LLMs for visual grounding**
다양한 multi-modal model은 grounding vision model의 LLM을 통합하는 것의 이점이 있다.
BLIP-2는 frozen image encoder와 LLM에서 vision-language pre-training을 bootstrap(시작)
training set에 포함되지 않은 object의 layout을 생성할 수 없다.
conditional image generation에 LLM을 포함하지만 CLIP text embedding에 의존해서 정보를 diffusion model에 전달한다는 한계가 있다.
our method와 비교하여 control이 부족하다. our method는 LLM에게 서로 다른 object의 spatial composition에 대해 추론하도록 명확하게 요구하고 직접적인 spatial control을 제공한다.
our work와 동시에 LayoutGPT는 CSS 구조에서 layout 생성을 위해 LLM prompting을 제안한다.
LayoutGPT는 LLM을 위해 관련된 in-context예제를 검색하기 위해 box와 caption이 달린 데이터셋에 의존한다. our method는 고품질 layout 생성을 위한 능력을 보여준다. pretrained LLM 가중치에는 고품질 layout 생성 기능이 이미 포함되어 있고 외부 annotation없이 고정된 in-context 예시를 사용해서 프롬프트 할 수 있는 능력이다.

**Spatially-conditioned image generation methods**
![](https://i.imgur.com/ytRZvkA.png)
pose, segmentation maps, strokes, layout 같은 주어진 prior에 따라 이미지를 생성.
전에는 주어진 layout에서 photorealistic 이미지를 합성한다.
전에는 scene graph를 활용한 이미지 생성.
ControlNet,LayoutDiffuse, GLIGEN, ReCo는 (layout box를 위한 open-vocabulary label을 지원하는)spatially-conditioned 이미지 생성을 위한 diffusion model의 훈련 기반 적응을 제안한다. 이런 방법들은 rely on annotated external dataset(COCO같은)..box와 caption을 제공하기 위해서..
훈련기반 adaption의 문제점은 추가 기능과의 모델 호환성을 저하시킨다. box annotation없이 training set에서 새로운 LoRA모델을 훈련하는 것이 어려워진다.
ours는 training-free generation controller를 제공한다. 이 generation controller는 layout-grounded image generation에 외부 dataset이 필요없다. 게다가 traininng-based 방법과도 통합돼서 추가적인 개선과 통합할 수 있다.

매우 최근, 이미지 생성에서 training-free region control이 가능해졌다. ours도 layout-to-image stage와 유사한 task formulation을 공유한다. 그러나 이 work들은 이미지 생성을 region semantics에 기반을 두고 있고 region semantics 내의 object instance 수에 대한 제어 기능이 거의 없다. 
ours는 instance에 기반한 생성에 집중하고 있다.

우리 instruction-based scene specification과 비슷하게 최근 instruction-based image editing을 제안했다.
LLM 기반 대화에서 external image editing model을 사용할 수 있다.
ours는 image pixel보다 ==scene layout==을 편집하는 것을 목표로 한다.

#### LLM-grounded Diffusion
LMD는 text prompt y가 주어졌을 때 image x0을 생성하는 것을 포함한 text-to-image generation setting에 집중한다.
two stage: text-grounded layout generation -> layout-grounded image generation

#### LLM-based Layout Generation
generate the layout of an image: embed the input text prompt y into a template and queries and LLM 

![](https://i.imgur.com/cOApwqk.png)
