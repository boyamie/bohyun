---
{"dg-publish":true,"permalink":"/paper-review/implement/controllable-diffusion-model/"}
---

**Knowledge Distillation(KD)** 은 **대형 모델(Teacher)** 이 **소형 모델(Student)** 에 지식을 전달하여, 소형 모델의 복잡도를 증가시키지 않고 성능을 향상시키는 기법이다.
**MMDetection**(인기 있는 객체 탐지 프레임워크)에서 **Knowledge Distillation(KD)** 를 구현하기 위해서는, **Teacher-Student 학습 패러다임**을 사용하여 학습을 진행한다.

**"Controllable Diffusion Model"** 논문을 기반으로 한 객체 탐지 모델에 **Knowledge Distillation**을 통합하려면, `trainer.py` 파일과 **config 파일**을 수정하여 KD 프레임워크를 지원해야한다.

### 1. **Trainer.py Modification**

```python
import os
import datetime
import uuid
import wandb
from mmcv import Config
from mmdet.datasets import build_dataset
from mmdet.models import build_detector
from mmdet.apis import train_detector, set_random_seed

# 추가: KD를 위한 Helper 함수들
from kd_utils import knowledge_distillation_loss

wandb.login()

def main():
    """메인 실행 함수"""
    
    # 실험 이름 생성
    timestamp = datetime.datetime.now().strftime("%Y-%m-%d_%H-%M-%S")
    random_code = str(uuid.uuid4())[:5]
    
    # 설정 생성
    cfg, model_name, output_dir = create_config('atss')
    experiment_dir = os.path.join(output_dir, f"{timestamp}_{random_code}")
    os.makedirs(experiment_dir, exist_ok=True)
    cfg.work_dir = experiment_dir

    wandb.init(
        project="Object Detection", 
        dir=experiment_dir,
        name=f'{model_name}_{random_code}',
        config=cfg._cfg_dict.to_dict()
    )
    
    # Optimizer 설정
    cfg.optimizer = dict(
        type='SGD', 
        lr=wandb.config.lr, 
        momentum=0.9,
        weight_decay=wandb.config.weight_decay
    )
    
    # 데이터셋 및 모델 빌드
    datasets = [build_dataset(cfg.data.train)]
    student_model = build_detector(cfg.model)
    teacher_model = build_detector(cfg.model)  # 같은 구조로 teacher 모델 생성
    teacher_model.init_weights()
    teacher_model.eval()  # Teacher 모델 고정

    student_model.init_weights()

    # Knowledge Distillation Loss 적용
    for epoch in range(cfg.runner.max_epochs):
        for batch in datasets:
            # Forward pass: 학생 모델 예측
            student_pred = student_model(batch['img'])
            
            # Forward pass: 교사 모델 예측 (고정된 모델)
            with torch.no_grad():
                teacher_pred = teacher_model(batch['img'])
            
            # KD 손실 계산
            kd_loss = knowledge_distillation_loss(student_pred, teacher_pred, cfg.kd_loss_weight)

            # 학습 및 손실 업데이트
            loss = kd_loss + student_model.compute_loss(batch, student_pred)
            loss.backward()
            optimizer.step()

if __name__ == "__main__":
    sweep_configuration = {
        "method": "bayes",
        "metric": {"goal": "maximize", "name": "val/bbox_mAP_50"},
        "parameters": {
            "lr": {"max": 0.003, "min": 0.0001},
            "weight_decay": {"max": 0.01, "min": 0.0001}
        },
        "early_terminate":{
            "type": "hyperband",
            "s": 3,
            "eta": 2,
            "min_iter": 8,
        }
    }

    sweep_id = wandb.sweep(sweep=sweep_configuration, project='ATSS')
    wandb.agent(sweep_id, function=main, count=2)
```
- `student_model` is the smaller model that you are trying to improve using **Knowledge Distillation**.
- `teacher_model` is typically a larger, more accurate model that provides "soft targets" for the student.
- **Knowledge Distillation Loss** (`kd_loss`) combines the student’s predictions with the teacher’s predictions.

### 2. **Config File Adjustments**

```python
# Config 파일의 추가 내용
kd_loss_weight = 0.5  # 교사-학생 학습에서의 손실 가중치

model = dict(
    type='ATSS',
    backbone=dict(
        type='ResNet',
        depth=50,
        init_cfg=dict(type='Pretrained', checkpoint='open-mmlab://detectron2/resnet50_caffe'),
    ),
    neck=dict(type='FPN', in_channels=[256, 512, 1024, 2048], out_channels=256, num_outs=5),
    bbox_head=dict(
        type='ATSSHead',
        num_classes=80,  # 클래스 수 맞추기
    ),
    teacher=dict(  # Teacher 모델 설정 추가
        type='ATSS',
        pretrained='path/to/teacher_model.pth',  # Teacher 모델 경로 설정
        backbone=dict(type='ResNet', depth=101),  # 교사는 더 큰 모델일 가능성이 큼
    )
)

# 손실 함수 설정
kd_loss = dict(
    type='DistillationLoss',
    temperature=4,  # Knowledge Distillation 온도 하이퍼파라미터
)
```

### 3. **Knowledge Distillation Loss Function (`kd_utils.py`)**

```python
import torch.nn.functional as F

def knowledge_distillation_loss(student_pred, teacher_pred, kd_weight=0.5, temperature=4):
    """
    Knowledge Distillation 손실 함수
    student_pred: 학생 모델의 예측 결과
    teacher_pred: 교사 모델의 예측 결과
    kd_weight: Knowledge Distillation 손실 가중치
    temperature: Knowledge Distillation의 온도
    """
    # softmax로 예측 결과를 변환
    teacher_probs = F.softmax(teacher_pred / temperature, dim=-1)
    student_probs = F.log_softmax(student_pred / temperature, dim=-1)
    
    # KL Divergence Loss 계산 (soft target 기반)
    kd_loss = F.kl_div(student_probs, teacher_probs, reduction='batchmean') * (temperature ** 2)
    
    return kd_weight * kd_loss
```