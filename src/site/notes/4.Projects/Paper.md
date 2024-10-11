---
{"dg-publish":true,"permalink":"/4-projects/paper/"}
---

# Data Augmentation for Object Detection via Controllable Diffusion Models

Ojbect Detection 작업에서 Data Augmentation을 수행하기 위해 **Controllable Diffusion Models**을 사용한다.
특히, 객체 탐지는 **expensive bounding box annotations**이 필요한데, 이 주석 작업은 시간과 비용이 많이 든다. 
이를 해결하기 위해 이 논문은 **diffusion model**과 **CLIP**을 활용하여 **synthetic data**를 생성하고, 그 데이터를 통해 성능을 향상시키는 방법을 제안한다.

1. **확산 모델을 이용한 합성 이미지 생성**
   - 확산 모델을 사용해 이미지 데이터를 증강하며, CLIP을 통해 **visual priors**를 제어하여 **카테고리에 맞는 데이터**를 생성합니다.
   - 이를 통해 기존의 데이터 증강 방법보다 더 정교하게 **특정 카테고리**에 맞는 이미지를 생성할 수 있습니다.

2. **포스트 필터링(Post-filtering)
   - 생성된 합성 데이터 중 **카테고리-보정된 CLIP 점수**를 사용하여 품질이 낮은 이미지를 필터링합니다. 이를 통해 **높은 품질의 데이터**만 학습에 사용되도록 보장합니다.

3. **Few-shot 설정에서 성능 향상**
   - 논문에서는 **MSCOCO**, **PASCAL VOC** 데이터셋을 활용한 실험에서 성능 향상을 확인했습니다. 특히, 적은 양의 데이터만으로도 효과적인 성능 향상을 보여주었으며, few-shot 설정에서 성능 개선을 입증했습니다.
   - COCO의 5/10/30-shot 설정에서는 각각 **+18.0%**, **+15.6%**, **+15.9%**의 mAP 성능 향상을 보였으며, PASCAL VOC 전체 데이터셋에서는 **+2.9%**, 기타 다운스트림 데이터셋에서는 평균 **+12.4%**의 성능 향상을 보였습니다.