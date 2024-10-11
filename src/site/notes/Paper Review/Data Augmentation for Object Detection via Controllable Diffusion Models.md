---
{"dg-publish":true,"permalink":"/paper-review/data-augmentation-for-object-detection-via-controllable-diffusion-models/"}
---

- [x] dp-home

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

이 논문은 **확산 모델(Controllable Diffusion Models)**을 이용하여 객체 탐지(Object Detection)를 위한 **데이터 증강** 방법을 제안하고 있다. 특히, 데이터 주석에 사람의 개입 없이도 **바운딩 박스**와 함께 **합성 이미지를 생성**하는 방법을 다룬다.

### ideas

1. **시각적 우선순위(Visual Priors)와 텍스트 프롬프트**를 사용하여 이미지와 바운딩 박스를 생성:
   - 확산 기반의 **inpainting** 기법을 사용해 바운딩 박스 내의 객체를 생성하고, 이와 함께 **텍스트 기반의 프롬프트**를 사용해 다양한 합성 데이터를 생성합니다.
   - 생성된 객체가 **바운딩 박스**를 완전히 채우지 않을 수 있지만, 이 방법은 기존의 **이미지 분류 기반** 확산 모델을 **객체 탐지**로 확장할 수 있는 간단한 방식입니다.

2. **CLIP 기반 필터링**:
   - 생성된 이미지에서 객체가 바운딩 박스와 **프롬프트에 부합하는지**를 판단하기 위해 **CLIP 점수**를 사용하여 자동으로 데이터를 필터링합니다. 이 과정에서 **카테고리별로 보정된(CLIP-calibrated) 점수**를 통해 데이터의 품질을 보장합니다.

3. **Inpainting 기법 통합**:
   - 추가적으로, **inpainting 기법**을 통해 이미지를 보완하고, 객체 탐지 성능을 높입니다.

### 성능 평가
- 이 방법은 **MSCOCO**, **PASCAL VOC**, 그리고 다양한 다운스트림 데이터셋에서 평가되었습니다. 특히, **few-shot** 설정과 **전체 데이터** 설정에서 모두 성능 향상을 확인했습니다.
- **COCO** 5/10/30-shot 설정에서 각각 **+18.0%**, **+15.6%**, **+15.9%**의 mAP 성능 향상을 보였으며, **PASCAL VOC** 전체 데이터셋에서는 **+2.9%** 향상, 그리고 다른 다운스트림 데이터셋에서는 평균 **+12.4%**의 성능 향상을 달성했습니다.

### 관련 작업
- 기존의 **생성 기반 데이터 증강** 방법은 주로 **이미지 분류**에만 적용되었으며, 객체 탐지에서의 데이터 증강은 아직 도전 과제가 많았습니다. 특히, **layout-to-image**나 **copy-paste** 방식은 객체 탐지에 적합하지 않았으나, 이 논문은 이를 극복하여 확산 모델을 통한 **효율적인 데이터 증강**을 가능하게 했습니다.

이 논문의 기여는 **고품질 바운딩 박스 주석이 포함된 합성 이미지**를 생성하고, 이를 통해 **객체 탐지 성능을 향상**시킨 점에서 의미가 있습니다.