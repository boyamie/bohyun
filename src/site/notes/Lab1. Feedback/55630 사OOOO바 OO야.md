---
{"dg-publish":true,"permalink":"/lab1-feedback/55630-oooo-oo/"}
---

### 1. VGGNet 코드 구현 및 평가
#### 1.1 VGGNet 네트워크 구조 (3점)
- **Type-D 구성**이 정확하게 구현되었다.
- `Conv2d -> BatchNorm2d -> ReLU` 순서로 레이어가 구성되었고, 3개의 클래스(plane, car, bird)를 분류하기 위한 **3-way classifier**가 구현되었다.
- **Batch Normalization**이 적절하게 적용되었으며, dropout 없이 구조가 명확하게 구현되었다.

#### 1.2 데이터셋 처리 및 학습 (2점)
- CIFAR-10 데이터셋에서 세 가지 클래스(plane, car, bird)를 적절히 사용하고 있으며, **플리핑**과 **랜덤 크롭핑**을 사용한 데이터 증강이 적용되었다.
- **Cross-Entropy Loss**와 **SGD 옵티마이저**가 설정되어 있으며, 하이퍼파라미터가 주어진 대로 설정되었다.
- 학습 진행 시 **테스트 정확도**와 **손실**이 매 epoch마다 출력되며, 학습 성능이 적절하게 향상되고 있다.

### 2. ResNet 코드 구현 및 평가
#### 2.1 ResNet50 네트워크 구조 (4점)
- **ResNet50** 네트워크 구조가 CIFAR-10에 맞게 구현되었으며, **bottleneck 블록**과 **strided convolution**을 사용한 다운샘플링이 적절히 적용되었다.
- Conv1 레이어의 변경 사항이 반영되었으며, max-pooling 레이어가 제거되었다.
- **Batch Normalization**이 정확하게 적용되었고, 네트워크 구조가 명확하다.

#### 2.2 학습 및 평가 (1점)
- ResNet50 모델이 학습되면서 매 epoch마다 **테스트 정확도**와 **손실**이 기록되었으며, 학습 성능이 점진적으로 향상되었다.
- **Cross-Entropy Loss**와 **SGD 옵티마이저** 설정이 적절하며, 학습 곡선이 안정적으로 진행되고 있음을 확인할 수 있다.