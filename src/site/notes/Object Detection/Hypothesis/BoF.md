---
{"dg-publish":true,"permalink":"/object-detection/hypothesis/bo-f/"}
---

Object Detection model을 향상시키기 위해 BOF, BOS 방법들을 실험을 통해서 증명하고 조합을 찾는다.

• BOF (Bag of Freebies) : inference 비용을 늘리지 않고 정확도 향상시키는 방법

• BOS (Bag of Specials) : inference 비용을 조금 높이지만 정확도가 크게 향상하는 방법

![](https://i.imgur.com/kGXxe4u.png)
2024.10.14(Mon)

![](https://i.imgur.com/Cxm4jrd.png)
Semantic Distribution Bias

• 데이터셋에 특정 라벨(배경)이 많은 경우 불균형을 해결하기 위한 방법

Label smoothing

• 라벨에 0 또는 1로 설정하는 것이 아니라 smooth하게 부여

• Ex) 원래 0이었던 라벨을 0.1로 부여, 1이었던 라벨을 0.9로 부여

• 모델의 overfitting 막아주고 regularization의 효과

Bounding Box Regression

• Bounding box 좌표값들을 예측하는 방법(MSE)은 거리가 일정하더라도 IoU가 다를 수 있음

• IoU 기반 loss 제안 (IoU는 1에 가까울수록 잘 예측한 것이므로 loss처럼 사용 가능)