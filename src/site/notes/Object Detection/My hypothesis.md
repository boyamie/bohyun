---
{"dg-publish":true,"permalink":"/object-detection/my-hypothesis/"}
---

1) EDA 
   object개수 40쯤에서 outlier처리
2) 파이프라인 구축 - 라이브러리, 개인코드, 공유된코드, 베이스라인코드구축
   [모델 찾기]
   모델 고르고 papers with code SOTA모델 중에서 mmdetection이나 detectron2에 없는거
   모델이름+선정이유
3) Validation set 찾기
4) 성능을 올리기 위한 시도

# Semantic Distribution Bias
데이터셋에 특정 라벨(배경)이 많은 경우 불균형을 해결하기 위한 방법

# Knowledge Distillation
MMDetection은 다양한 객체 탐지 모델을 쉽게 구성하고 실행할 수 있는 프레임워크입니다. MMDetection에서 논문 내용을 적용하기 위해서는 **config 파일 수정**과 **필요한 커스텀 코드 추가**가 핵심입니다. 논문에서 제안한 방법을 적용하려면 다음과 같은 부분을 수정해야 합니다.
### 1. **카테고리 친화도 매트릭스(Category Affinity Matrix)**
   - **Dataset Pipeline 수정**: 카테고리 친화도 매트릭스를 사용하여 다양한 카테고리 간의 관계를 반영하려면 데이터 증강 파이프라인에서 이를 반영해야 합니다. MMDetection의 `data` 섹션에서 데이터 증강을 담당하는 부분에 새로운 증강 방식(친화도 기반 이미지 변환)을 추가할 수 있습니다.
   - `datasets/coco_detection.py` 또는 관련 데이터셋 파일에서 데이터 로드 및 전처리 과정을 수정하여 카테고리 간 친화도 매트릭스를 반영할 수 있습니다.

### 2. **주변 영역 정렬(Surrounding Region Alignment)**
   - **Custom Data Augmentation 추가**: Surrounding Region Alignment는 특정 객체 영역과 그 주변 환경을 다르게 처리하는 방식입니다. 이 작업은 데이터 증강 과정에 통합되어야 하므로, `transform` 또는 `pipeline` 섹션에서 새로운 커스텀 증강 클래스를 추가하여 주변 영역 정렬을 구현할 수 있습니다. 
   - MMDetection의 `mmdet.datasets.pipelines`에 있는 데이터 증강 코드에 맞춰 새로운 augmentation 클래스를 정의하고 이를 config 파일에 적용할 수 있습니다.

### 3. **인스턴스 수준 필터링(Instance-Level Filtering)**
   - **Post-Processing 수정**: 인스턴스 수준 필터링은 생성된 이미지나 예측 결과에서 저품질 객체를 필터링하는 방법입니다. MMDetection에서 후처리 단계에서 이 필터링을 구현할 수 있습니다. 예를 들어, 예측이 완료된 후 특정 기준에 맞는 객체만 남기도록 `mmdet/models/detectors/base.py` 등에서 후처리 로직을 수정할 수 있습니다.

### 4. **Config 파일 수정**
   - 논문에서 제시한 모델 구조, 데이터 증강, 학습 전략을 반영하려면 config 파일에서 `optimizer`, `lr_scheduler`, `runner`, `model` 등을 수정해야 합니다. 
   - `cfg = Config.fromfile('path_to_config')`로 config 파일을 로드하고, 이를 바탕으로 논문에 맞게 수정한 후 모델을 학습시킬 수 있습니다.

### 5. **Stable Diffusion 적용**
   - MMDetection에서 Stable Diffusion 모델을 적용하려면, 확산 모델을 기반으로 데이터 증강을 진행한 후 해당 데이터를 MMDetection에 맞는 포맷으로 저장해 사용할 수 있습니다. 혹은 확산 모델을 직접 호출하는 파이프라인을 구축해 자동으로 증강할 수 있도록 코드를 구성할 수 있습니다.
