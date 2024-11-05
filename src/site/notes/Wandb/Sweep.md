---
{"dg-publish":true,"permalink":"/wandb/sweep/"}
---

난 원래 독스를 읽지않고 그냥 해보는 사람이였다.
그러나 지난 프로젝트를 겪으며 ultralytics docs의 중요성을 뼈저리게 깨닫고
이번 sweep을 돌리면서 sweep 공식문서를 읽어보려고 한다..
![](https://i.imgur.com/oZFUwdJ.png)
지금까지 나와는 다르다.
[Sweeps | Weights & Biases Documentation](https://docs.wandb.ai/guides/sweeps/?_gl=1*njn2w1*_gcl_au*MTk1MjU0MjI4OS4xNzI2OTI2OTk4)
문서를 읽어보겠습니다
![](https://i.imgur.com/no4WOgx.png)
이건 제가 어제 만들어둔 agent로 돌린 sweep입니다.
```yaml
# sweep_config.yaml

program: train.py

method: bayes # 또는 'grid' 또는 'random'

metric:

name: val_f1_score # W&B에서 사용할 목표 메트릭

goal: maximize

  

parameters:

optimizer:

values: ["Adam", "AdamW"]

learning_rate:

min: 0.0001

max: 0.01

batch_size:

values: [4, 8, 16]

max_epoch:

values: [50, 100, 150]

save_interval:

value: 5 # 모델 저장 주기 (예: 매 5 에포크마다)

seed:

value: 4096 # 고정된 시드값 설정

data_dir:

value: /data/ephemeral/home/data # 기본 데이터 경로 설정

model_name:

values: "vgg16_bn-6c64b313" # 사용할 모델 선택 가능

dropout_rate:

min: 0.1

max: 0.5 # 드롭아웃 비율 범위 설정

weight_decay:

min: 0.0001

max: 0.01 # 가중치 감쇠 범위 설정

  

# 추가 기본 설정

early_stopping:

value: true # 학습 조기 종료 설정

early_stopping_patience:

value: 10 # 조기 종료 대기 에포크 수

  

augmentation:

values: ["basic", "strong", "none"] # 데이터 증강 방법 선택

  

scheduler:

values: ["StepLR", "CosineAnnealingLR"] # 학습률 스케줄러 선택 가능

  

log_interval:

value: 10 # 로그 출력 주기 설정

val_interval:

value: 1 # 검증 주기 (예: 매 에포크마다 검증 실행)

  

save_best_only:

value: true # 최상의 모델만 저장

  

use_gpu:

value: true # GPU 사용 여부 설정

  

project_name:

value: "cv-21" # 프로젝트 이름 설정

entity:

value: "codehyun17"
```
이건 저의 sweep yaml 파일입니다.

#### sweep 초기화와 agent시작
```bash
wandb sweep --project <propject-name> <path-to-config file>
```
초기화하는 코드입니다
```bash
wandb agent <sweep-ID>
```
sweep agent를 시작합니다.

#### 코드 뜯어보기
wandb sweep --cancel cv-21/uncategorized/jhx56d1w
