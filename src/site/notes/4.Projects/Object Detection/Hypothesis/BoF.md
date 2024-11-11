---
{"dg-publish":true,"permalink":"/4-projects/object-detection/hypothesis/bo-f/"}
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

현재 `inference.py`와 `train.py` 파일에서 **MMDetection** 라이브러리를 사용하여 실험을 진행하고, **WandB**를 통해 하이퍼파라미터 튜닝 및 실험 관리가 가능하도록 설정한 상태다. 

### 1. BOF 기법 적용
**BOF (Bag of Freebies)** 는 추론 비용을 증가시키지 않고 정확도를 향상시키는 기법
- **Data Augmentation**: 이미지 크기 조정, 잘라내기, 회전, 색상 변형 등으로 다양한 데이터를 생성하여 학습의 다양성을 높입니다. MMDetection에서 `pipeline`을 통해 설정할 수 있습니다.
- **Label Smoothing**: 라벨 스무딩은 모델이 확실한 결정을 내리지 않도록 하여 overfitting을 방지하는 데 도움을 줍니다.
- **IoU Loss**: 일반적으로 사용하는 `bbox regression` 대신 `IoU Loss`를 사용하여 예측된 박스의 IoU가 높을수록 loss가 줄어드는 형태로 설정할 수 있습니다.

#### 적용 방법:
1. **Label Smoothing 적용**
   - Config 파일에서 `CrossEntropyLoss` 대신 `LabelSmoothLoss`를 사용하는 방법입니다. 이를 위해 MMDetection의 `loss` 부분을 수정합니다:
     ```python
     model = dict(
         roi_head=dict(
             bbox_head=dict(
                 loss_cls=dict(
                     type='LabelSmoothLoss',  # CrossEntropy 대신 Label Smooth Loss 적용
                     reduction='mean',
                     smoothing=0.1)  # smoothing 정도 설정
             )
         )
     )
     ```
   
2. **IoU Loss 적용**
   - `Bounding box regression` 대신 IoU 기반의 손실 함수로 변경하여 정확도를 높입니다:
     ```python
     bbox_head=dict(
         loss_bbox=dict(
             type='IoULoss',  # IoU 기반 loss 적용
             loss_weight=1.0
         )
     )
     ```

3. **Data Augmentation**:
   - Config 파일에서 데이터 전처리 파이프라인을 설정할 수 있습니다. 
     ```python
     train_pipeline = [
         dict(type='Resize', img_scale=(1333, 800), keep_ratio=True),
         dict(type='RandomFlip', flip_ratio=0.5),
         dict(type='RandomRotate', prob=0.5, degree=10),  # 회전 추가
         dict(type='ColorJitter', brightness=0.2, contrast=0.2, saturation=0.2),  # 색상 변형 추가
         # 다른 augmentation 기법 추가 가능
     ]
     ```
### 3. Semantic Distribution Bias 해결
배경 데이터가 과도하게 많은 경우, 라벨 불균형 문제를 해결하기 위해 **focal loss**나 **class re-weighting**을 사용할 수 있습니다.

```python
model = dict(
    roi_head=dict(
        bbox_head=dict(
            loss_cls=dict(
                type='FocalLoss',  # 불균형을 완화하기 위한 Focal Loss 적용
                gamma=2.0,
                alpha=0.25
            )
        )
    )
)
```