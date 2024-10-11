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

3. **Inpainting 기법 통합**
	추가적으로, **inpainting 기법**을 통해 이미지를 보완하고, 객체 탐지 성능을 높인다.

### 관련 작업
- 기존의 **생성 기반 데이터 증강** 방법은 주로 **이미지 분류**에만 적용되었으며, 객체 탐지에서의 데이터 증강은 아직 도전 과제가 많았습니다. 특히, **layout-to-image**나 **copy-paste** 방식은 객체 탐지에 적합하지 않았으나, 이 논문은 이를 극복하여 확산 모델을 통한 **효율적인 데이터 증강**을 가능하게 했습니다.

이 논문의 기여는 **고품질 바운딩 박스 주석이 포함된 합성 이미지**를 생성하고, 이를 통해 **객체 탐지 성능을 향상**시킨 점에서 의미가 있습니다.