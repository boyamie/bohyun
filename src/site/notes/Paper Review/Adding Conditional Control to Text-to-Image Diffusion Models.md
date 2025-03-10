---
{"dg-publish":true,"permalink":"/paper-review/adding-conditional-control-to-text-to-image-diffusion-models/"}
---

[paper pdf](https://openaccess.thecvf.com/content/ICCV2023/papers/Zhang_Adding_Conditional_Control_to_Text-to-Image_Diffusion_Models_ICCV_2023_paper.pdf)

ControlNet: NN architecture to add spatial conditioning controls to large, pretrained text-to-image diffusion models.

neural architecture --connected-- "zero convolutions"
parameter: zero-initialized convolution layers(zero -> progressively grow)
harmful noise affect X (to finetuning)
초록내용이다. ControlNet은 거대하고 사전학습된 text-to-image diffusion model에 공간적 conditioning control을 nn에 더한 architecture다.
신경망 구조는 "zero convolution"과 연결된다. (==zero-initialized convolution layers==, 이게 뭔지 아직 모르겠음. 나중에 설명 나오면 자세히 읽어보자!) 여기서 설명하기론 1. 파라미터가 점진적으로 증가하고 2. noise가 finetuning에 악영향을 끼칠 수 없게한다고 한다.

여기서 테스트한 conditioning control에는 edge, depth, segmentation, human pose가 있다.
prompt없이 싱글 또는 멀티 condition을 사용해서 Stable diffusion으로 테스트했다.

## Introduction
control 한계: 공간적 composition 정확하게 표현을 text prompt 혼자 하기 어려움(spatial composition: layout,pose, shape,form)
large text-to-image diffusion model을 위한 conditional control을 엔드 투 엔드 방식으로 학습하는건 어렵다.
적은 데이터셋 해결법: 훈련가능한 파라미터의 수나 rank를 제한해서 forgetting을 완화한다.

ControlNet은 end-to-end nn architecture인데 사전 훈련된 text-to-image diffusion model(여기선 Stable Diffusion 사용)의 conditional control을 학습한다.(이 논문에서 제안함)
라지모델의 품질과 기능을 유지하기 위해 매개변수를 고정함으로써 인코딩 레이어의 학습가능한 복사본을 생성한다.
복사본(학습가능)과 원본(잠금)은 zero convolution 레이어로 연결된다. 가중치는 훈련동안 점진적으로 증가하도록 zero로 초기화된다.
훈련 시작 때 라지 diffusion model의 deep feature에 해로운 노이즈가 더해지지 않도록 보장한다. 훈련가능한 복사본의 대규모 사전 훈련된 백본이 이런 노이즈로부터 손상되지 않도록 보호한다.

ControlNet: 다양한 conditioning input을 통해 Stable Diffusion을 control할 수 있다. conditioning input에는 Canny edge, Hough line, user scribble, human key points, segmentation maps, shape nnormals, depths등이 포함된다.
single conditioning image를 사용해서 접근 방식에 테스트한다. textprompt 있을 때 없을 때 모두 포함. 
각 모델 구성 요소의 기여도를 조사하기 위해 ablative study(제거 실험)을 수행한다. user연구를 통해 강력한 conditional image generation baseline과 비교한다.

(1) ControlNet: add spatially localized input conditions to a pretrained text-to-image diffusion model via efficient finetuning.
(2) control Stable diffusion(by pretrained ControlNets)
(3) ablative experiments 소거 실험

## Related Work
### Finetuning Neural Networks
continue training with 추가 training data. 추가 데이터 문제는 overfitting, mode collapse, catastrophic forgetting.
#### HyperNetwork
하이퍼 네트워크는 NLP에서 시작됨. 작은 recurrent nn(순환신경망)이 더 큰 신경망의 가중치에 영향을 미치도록 훈련하는 것이 목표다.
#### Adapter
NLP에선 사전훈련된 트랜스포머 모델을 다른 task에 커스터마이징 하기 위해서 새로운 모듈 레이어를 embedding하는데 사용한다. 
CV에선 adapter는 incremental learning(점진적 학습)과 domain adaption에 사용된다. CLIP과 함께 사용된다. 다양한 task로 사전훈련된 backbone 모델을 transfer하기 위해서이다. 최근엔 adapter가 vision transformer와 ViT-adapter에서 성공적 결과를 도출했다. T2I adapter는 Stable Diffusion을 외부 조건에 맞게 adapt한다.
#### Additive Learning
원래 모델 가중치는 freeze, 학습된 가중치 마스크, pruning, hard attention을 사용해서 적은 수의 parameter를 추가함으로써 circumvent forgetting.
Side-tuning은 frozen 모델의 출력을 추가한 네트워크와 선형적으로 blending해서 extra 기능을 학습하기 위해 side branch model을 사용한다. predefined blending weight schedule에 따라 이루어진다.
#### Low-Rank Adaptation(LoRA)
 catastrophic. forgetting을 방지하기위해 low rank(차원) 행렬을 사용해서 매개변수의 offset(편차)를 학습한다. 과도하게 파라미터화된 모델은 낮은 intrinsic dimension subspace에 존재한다는 관찰을 기반으로 한다.

#### Zero-Initialized Layer
ControlNet에서 네트워크 블록을 연결하는데 사용된다.
nn에 대한 연구는 네트워크 가중치 초기화와 조정에 대해 광범위하게 논의되었다. 
 