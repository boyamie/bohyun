---
{"dg-publish":true,"permalink":"/object-detection/model/bug-report/"}
---

2024.10.22(Tue) rt-DETR 결과 분석
#### 문제상황

![](https://i.imgur.com/dMM23Aq.png)
클래스 라벨이 하나씩 밀린 현상이 발생했다.
가설1: 학습 및 변환 과정에서 클래스 ID와 실제 클래스 간의 매핑이 잘못되어 발생했을 것이다.
가설2: category 문제가 아니고 .txt로 변환하는 과정에서 json을 잘못 읽어온 거라면?

가설 1 solution
- `convert_yolo` 함수 수정
	`category_id`를 조정하는 부분에서 클래스 ID가 정확히 일치하도록 수정한다. 
	if COCO 형식의 `category_id`가 이미 0부터 시작한다면, `category_id -= 1`을 하면 클래스 매핑이 밀린다.

가설 2 solution
- ID 오프셋 확인
	`category_id`에서 1을 빼서 YOLO 형식의 클래스 ID로 변환할 때, 1을 빼지 않아야 할 경우 혹은 데이터셋에 따라 조정이 필요한 경우가 있는데 내가 조정한 곳에서 문제가 발생했을 수 있다.

#### 가설 검증
`convert_yolo` 함수에서 `category_id` 처리 과정에서 클래스 ID를 1씩 빼주는 것이 잘못된 것 같다. 
`convert_yolo` 함수에서 `category_id -= 1` 부분을 제거하고 `category_id`를 그대로 사용했다.

#### 해결
가설 1 검증 결과, 코드 한줄을 수정했더니 문제를 해결했다.

![](https://i.imgur.com/m3mN9Bh.png)

#### 문제발생
![](https://i.imgur.com/dMMb0fQ.png)
wandb에서 10개 클래스 라벨이 포함됨
#### 변경
ground-truth에 10개 클래스 외의 라벨이 포함되지 않도록 수정하고 예측 결과를 올릴 때 WandB의 Vision Table에서 클래스 이름을 명시적으로 설정하고, 예측한 클래스가 10개 클래스에 해당하는지 필터링

![](https://i.imgur.com/uV4VQCs.png)

#### 문제상황
Pascal VOC형태로 바꿀 때, xmin,ymin,xmax,ymax형태로 저장해야되는데 xyxy는 우