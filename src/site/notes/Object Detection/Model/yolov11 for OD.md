---
{"dg-publish":true,"permalink":"/object-detection/model/yolov11-for-od/"}
---

2024.10.17(Thu)
### 목표
1. YOLOv11x 를 활용해 COCO 형식의 학습 데이터를 사용해 object detection model 학습
2. Pascal VOC 형식의 제출 파일을 생성

### To do
- [x] ➕ 2024-10-17 Refactoring ✅ 2024-10-17
- [x] ➕ 2024-10-17 W&B를 활용한 mAP50 시각화 도입 ✅ 2024-10-17
- [x] ➕ 2024-10-17 Streamlit을 통한 EDA 도입 ✅ 2024-10-17
#### Data
data는 COCO 형식으로 제공. 
train data(`train.json`)는 COCO 형식의 annotation을 포함하고 있다.
test data(`test.json`)는 이미지 정보만 포함하고 있다. 
YOLOv11x 모델을 사용하려면 COCO 형식의 데이터를 YOLO 형식으로 변환해야 한다.

#### COCO format
- images: id, height, width, filename
- annotations: bbox, area, category_id, image_id, is_crowd
- categories

#### Pascal VOC format (submission)
- PredictionString: `(label, score, xmin, ymin, xmax, ymax)`로 구성된 `.txt` file
- 제출은 `.csv`로 해야 한다. (이미지와 해당 이미지에서 탐지된 객체들에 대한 정보를 포함)

#### 실행 순서
- 1. convert.py: COCO -> YOLO 형식 변환
- 2. split.py: 데이터 분할 (train/val)
- 3. train.py: YOLO 모델 학습
- 4. inference.py: 추론 및 CSV 변환
- 5. streamlit.py: 추론 결과 시각화

## Refactoring
#### 1. convert.py
* 코드는 feature-72 브랜치의 convert.py를 참고해주세요.
COCO 형식의 `train.json` 파일을 YOLO 형식으로 변환하기 위한 코드이다.
각 이미지에 대해 텍스트 파일을 생성하고, 이 파일에 객체의 클래스, 바운딩 박스 좌표를 저장한다.
변환된 label file들은 이미지와 동일한 이름을 가지며, `.txt` 확장자로 저장된다.

#### 2. train.py
* 코드는 feature-72 브랜치의 train.py를 참고해주세요.
