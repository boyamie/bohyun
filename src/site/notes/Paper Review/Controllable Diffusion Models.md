---
{"dg-publish":true,"permalink":"/paper-review/controllable-diffusion-models/"}
---

[Paper](https://openaccess.thecvf.com/content/WACV2024/papers/Fang_Data_Augmentation_for_Object_Detection_via_Controllable_Diffusion_Models_WACV_2024_paper.pdf)
[Code](https://github.com/FANGAreNotGnu/ControlAug)
## abstract

Objbect Detection 작업에서 Data Augmentation을 수행하기 위해 **Controllable Diffusion Models**을 사용한다.
특히, 객체 탐지는 **expensive bounding box annotations**이 필요한데, 이 주석 작업은 시간과 비용이 많이 든다. 
이를 해결하기 위해 이 논문은 **diffusion model**과 **CLIP**을 활용하여 **synthetic data**를 생성하고, 그 데이터를 통해 성능을 향상시키는 방법을 제안한다.

1. **확산 모델을 이용한 합성 이미지 생성**
   - 확산 모델을 사용해 이미지 데이터를 증강하며, CLIP을 통해 **visual priors**를 제어하여 **카테고리에 맞는 데이터**를 생성한다.
   - 이를 통해 기존의 데이터 증강 방법보다 더 정교하게 **특정 카테고리**에 맞는 이미지를 생성할 수 있다.

2. **포스트 필터링(Post-filtering)
   - 생성된 합성 데이터 중 **카테고리-보정된 CLIP 점수**를 사용하여 품질이 낮은 이미지를 필터링한다. 
   - 이를 통해 **높은 품질의 데이터**만 학습에 사용되도록 보장한다.

3. **Few-shot 설정에서 성능 향상**
   - 논문에서는 **MSCOCO**, **PASCAL VOC** 데이터셋을 활용한 실험에서 성능 향상을 확인했다. 특히, 적은 양의 데이터만으로도 효과적인 성능 향상을 보여주었으며, few-shot 설정에서 성능 개선을 입증했다.
   - COCO의 5/10/30-shot 설정에서는 각각 **+18.0%**, **+15.6%**, **+15.9%** 의 mAP 성능 향상을 보였다.
   - PASCAL VOC 전체 데이터셋에서는 **+2.9%**, 기타 다운스트림 데이터셋에서는 평균 **+12.4%** 의 성능 향상을 보였다.

**객체 탐지(Object Detection)** 모델이 최첨단 성능을 발휘하기 위해서는 **대규모의 다양하고 주석된 데이터셋**이 필요하다. 객체 탐지는 단순히 이미지 분류를 넘어서 **정확한 바운딩 박스(bounding box)** 주석이 필요하다.

## Intro

![Figure 1.](https://i.imgur.com/HWnOvce.png) Figure 1.

![](https://i.imgur.com/uKGGoWf.png)

객체 탐지를 위한 데이터 증강 기법을 제안한다. 객체 탐지(Object Detection)는 이미지 분류보다 더 복잡한 주석 작업이 필요하며, 정확한 바운딩 박스(bounding box) 정보를 추가로 포함해야 한다. 새로운 데이터를 주석하는 대신, 데이터 증강(data augmentation)을 활용해 기존 데이터를 변형하여 더 많은 학습 데이터를 생성하는 방법이 주로 사용된다. 전통적인 데이터 증강은 회전, 크기 조정, 이미지 뒤집기 등의 방법을 포함하지만, 최근에는 생성 모델을 활용한 데이터 증강이 주목받고 있다.

### ideas

1. **diffusion models**
	생성 기반 데이터 증강은 비생성 방식으로는 얻을 수 없는 다양성과 현실성, 새로운 시각적 특징을 학습 데이터에 추가할 수 있다는 장점이 있다. 하지만 생성된 이미지에서 객체 탐지를 위해 정확한 바운딩 박스 주석을 얻는 것은 어려운 과제이다. 이를 해결하기 위해, 저자들은 확산 모델(diffusion models)을 사용하여 바운딩 박스를 지정한 후, 그 내부에 객체를 생성하는 방법을 제안한다. 또한 텍스트 기반 이미지 생성과 시각적 사전 정보를 결합해 스타일, 조명, 객체를 다르게 생성하면서도 고품질의 바운딩 박스를 유지하는 방법도 탐구하였다.

2. **CLIP 기반 필터링**
	보정된 CLIP 점수를 사용하여 생성된 이미지의 품질을 자동으로 제어하며, 페인트 기법을 결합해 성능을 더 개선하는 방법을 제안한다. 이 방법은 MSCOCO, PASCAL VOC 등의 데이터셋에서 실험을 통해 평가되었으며, 소량의 데이터로도 성능을 크게 개선할 수 있음을 보였다. YOLOX 탐지기의 성능은 소량의 COCO 데이터셋에서 최대 18.0% 개선되었고, 전체 PASCAL VOC 데이터셋에서는 2.9% 향상되었다.

## Method

![](https://i.imgur.com/Diy6Nql.png)

1. Visual Prior Generator
2. Prompt Constructor
3. Controllable Diffusion Model
4. Post Filter with Category-Calibrated CLIP Rank
### 2.1. Prior Extractor (비주얼 프라이어 추출기)
- **Visual Prior Generator**는 훈련에 사용될 이미지-주석 쌍에서 M개의 이미지를 샘플링하고, 일반적인 데이터 변환을 적용하여 "비주얼 프라이어-주석 쌍"을 생성한다. 기본적으로 HED 에지 검출기(HED edge detector)를 사용하며, 다른 비주얼 프라이어 추출기도 논의된다(예: Canny 에지 검출기, 세그멘테이션 마스크).

### 2.2. Prompt Construction (프롬프트 생성)
- 주석 정보에 따라 프롬프트를 생성하며, 기본 전략은 주석에 있는 모든 카테고리 라벨을 하나의 문장으로 결합하는 것이다. 여러 가지 다른 전략도 실험하였으나, 기본 전략이 가장 효과적임이 밝혀졌다.

### 2.3. Controllable Diffusion Model (제어 가능한 확산 모델)
- 확산 모델을 사용하여 주석된 이미지-프롬프트 쌍으로부터 합성 이미지를 생성한다. 미리 훈련된 모델의 일부 파라미터는 고정되고, 다른 부분은 업데이트된다. 이 과정에서 생성된 이미지는 주석과 일치하는 구조를 가지며, 결과적으로 고품질의 바운딩 박스가 포함된 합성 이미지-주석 쌍이 생성된다.

### 2.4. Post Filter with Category-Calibrated CLIP Rank 
	카테고리 보정된 CLIP 점수를 사용한 후처리 필터
- 생성된 이미지 내에서 바운딩 박스 안의 객체가 프롬프트와 일치하는지 확인하기 위해 CLIP 점수를 계산한다. 그런 다음 각 주석에 대해 유사도 점수를 수집하고, 이를 기반으로 데이터의 품질을 평가하여 상위 품질의 합성 데이터를 필터링한다.

