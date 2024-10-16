---
{"dg-publish":true,"permalink":"/object-detection/model/yol-ov11/"}
---


### YOLOv11의 주요 기능
1. **향상된 특징 추출**
   YOLOv11은 개선된 백본과 넥 아키텍처를 사용하여 특징 추출 능력을 크게 향상시킴. 
   복잡한 시각 작업도 정확하게 처리할 수 있습니다.

2. **최적화된 효율성과 속도**
   YOLOv11은 아키텍처 설계를 개선하고 학습 파이프라인을 최적화하여 더 빠른 처리 속도를 제공합니다. 이를 통해 실시간 및 대규모 애플리케이션 모두에서 높은 정확도를 유지하면서 빠른 속도로 동작합니다.

3. **더 적은 파라미터로 높은 정확도**
   YOLOv11의 중간 크기 모델인 YOLOv11m은 COCO 데이터셋에서 더 적은 파라미터(22% 감소)를 사용하면서도 더 높은 mAP(평균 정확도)를 기록합니다. 이는 연산 효율성을 높이면서도 성능 저하 없이 높은 정확도를 유지합니다.

4. **다양한 환경에서의 적응성**
   YOLOv11은 엣지 디바이스, 클라우드 플랫폼, 또는 NVIDIA GPU 기반 시스템 등 다양한 배포 시나리오에서 유연하게 사용할 수 있습니다.

5. **광범위한 작업 지원**
   YOLOv11은 전통적인 객체 탐지 외에도 인스턴스 분할, 이미지 분류, 자세 추정, 방향 객체 탐지(OBB) 등을 지원합니다. 이로 인해 다양한 컴퓨터 비전 과제를 해결할 수 있는 강력한 도구로 자리매김합니다.

---

### YOLOv11을 이용한 이미지 객체 탐지 방법

#### 1. 필요한 라이브러리 설치
```bash
pip install opencv-python ultralytics
```

#### 2. 라이브러리 불러오기
```python
import cv2
from ultralytics import YOLO
```

#### 3. 모델 선택
```python
model = YOLO("yolo11x.pt")
```
YOLOv11의 다양한 모델을 비교할 수 있으며, 여기서는 `yolo11x.pt` 모델을 선택했습니다.

#### 4. 이미지에서 객체를 예측하고 탐지하는 함수 작성
```python
def predict(chosen_model, img, classes=[], conf=0.5):
    if classes:
        results = chosen_model.predict(img, classes=classes, conf=conf)
    else:
        results = chosen_model.predict(img, conf=conf)

    return results

def predict_and_detect(chosen_model, img, classes=[], conf=0.5, rectangle_thickness=2, text_thickness=1):
    results = predict(chosen_model, img, classes, conf=conf)
    for result in results:
        for box in result.boxes:
            cv2.rectangle(img, (int(box.xyxy[0][0]), int(box.xyxy[0][1])),
                          (int(box.xyxy[0][2]), int(box.xyxy[0][3])), (255, 0, 0), rectangle_thickness)
            cv2.putText(img, f"{result.names[int(box.cls[0])]}",
                        (int(box.xyxy[0][0]), int(box.xyxy[0][1]) - 10),
                        cv2.FONT_HERSHEY_PLAIN, 1, (255, 0, 0), text_thickness)
    return img, results
```

#### 5. YOLOv11을 사용한 이미지 객체 탐지
```python
# 이미지 불러오기
image = cv2.imread("YourImagePath.png")
result_img, _ = predict_and_detect(model, image, classes=[], conf=0.5)
```
특정 클래스만 탐지하려면 클래스 ID를 `classes` 리스트에 넣으면 됩니다.

#### 6. 결과 이미지 저장 및 출력
```python
cv2.imshow("Image", result_img)
cv2.imwrite("YourSavePath.png", result_img)
cv2.waitKey(0)
```

#### 전체 코드
```python
from ultralytics import YOLO
import cv2

def predict(chosen_model, img, classes=[], conf=0.5):
    if classes:
        results = chosen_model.predict(img, classes=classes, conf=conf)
    else:
        results = chosen_model.predict(img, conf=conf)
    return results

def predict_and_detect(chosen_model, img, classes=[], conf=0.5, rectangle_thickness=2, text_thickness=1):
    results = predict(chosen_model, img, classes, conf=conf)
    for result in results:
        for box in result.boxes:
            cv2.rectangle(img, (int(box.xyxy[0][0]), int(box.xyxy[0][1])),
                          (int(box.xyxy[0][2]), int(box.xyxy[0][3])), (255, 0, 0), rectangle_thickness)
            cv2.putText(img, f"{result.names[int(box.cls[0])]}",
                        (int(box.xyxy[0][0]), int(box.xyxy[0][1]) - 10),
                        cv2.FONT_HERSHEY_PLAIN, 1, (255, 0, 0), text_thickness)
    return img, results

model = YOLO("yolo11x.pt")
image = cv2.imread("YourImagePath.png")
result_img, _ = predict_and_detect(model, image, classes=[], conf=0.5)
cv2.imshow("Image", result_img)
cv2.imwrite("YourSavePath.png", result_img)
cv2.waitKey(0)
```

#### **COCO 형식 주석을 YOLO 형식으로 변환**
COCO 형식의 `train.json` 및 `test.json` 파일을 YOLO 형식의 `.txt` 파일로 변환합니다.
(COCO 형식에서 클래스 ID는 1부터 시작하기 때문에 YOLO에서 사용하는 0부터 시작하는 방식으로 맞추기 위해 -1을 해줍니다.)

명령어
```bash
python /data/ephemeral/home/cv-21/yolov11/utils/convert_json_to_yolo.py
```

![](https://i.imgur.com/pjlwy87.png)

![](https://i.imgur.com/bsMaaPI.png)


YOLO 형식, 각 이미지마다 `.txt` 파일이 생성되었습니다.

데이터 출력 확인은 변환된 `.txt` 파일에 실제로 클래스 ID와 바운딩 박스 정보가 제대로 저장되었는지 확인하면 됩니다.

변환이 제대로 완료되면 `.txt` 파일 예시입니다. `0 0.5 0.5 0.2 0.3 1 0.4 0.6 0.1 0.1`
각 줄은 개별 객체의 정보를 나타내며, 형식은 `[class_id] [x_center] [y_center] [width] [height]` 입니다.
- `x_center`, `y_center`: 바운딩 박스의 중심 좌표 (이미지 크기에 정규화된 값, 즉 0~1 사이의 값)
- `width`, `height`: 바운딩 박스의 너비와 높이 (역시 이미지 크기에 정규화된 값)

위와 같은 포맷으로 클래스 ID와 바운딩 박스 정보가 저장되어야 모델이 정상적으로 학습할 수 있습니다.

`<class_id> <x_center> <y_center> <width> <height>`

명령어
```bash
python train.py --data config/dataset.yaml --weights models/yolo11x.pt --epochs 50 --img-size 512 --batch-size 16
```
`YOLO`와 `MMDetection`의 구조가 다르기 때문에, `inference.py`를 YOLO에 맞게 수정할 필요가 있습니다. YOLOv11을 기반으로 inference 및 CSV 파일 생성을 위한 코드를 작성하려면, 모델을 불러오고, 테스트 데이터를 처리한 후, 결과를 CSV 파일로 출력하는 부분을 구성해야 합니다.

MMDetection에서는 `single_gpu_test`와 같은 유틸리티 함수가 제공되지만, YOLO에서는 직접 모델을 사용하여 예측값을 추론하고 결과를 파일로 저장하는 방식을 사용해야 합니다. YOLO의 경우 보통 `detect.py`나 비슷한 스크립트를 사용하여 추론을 수행합니다.

아래는 YOLOv11의 추론 결과를 CSV 파일로 저장하는 Python 코드 예시입니다.

### 1. `detect.py` (추론 결과 CSV로 저장)
YOLOv11을 사용해 추론을 수행하고 그 결과를 CSV 파일로 저장
### 2. 코드 설명

- `detect_and_save_csv`: 이 함수는 모델을 불러오고 이미지를 로드한 후 추론을 수행하고 결과를 CSV 파일로 저장
- 결과 형식 결과는 이미지 파일 이름, 클래스, 신뢰도, 중심 좌표(x, y) 및 객체의 너비, 높이를 포함하는 CSV 파일로 저장
- 파라미터
    - `weights`: 추론에 사용할 YOLO 모델의 가중치 파일 경로.
    - `source`: 추론할 이미지나 비디오 파일의 경로.
    - `img_size`: 이미지 크기.
    - `conf_thres`: 객체 검출의 confidence threshold.
    - `iou_thres`: NMS(Non-Maximum Suppression)를 적용할 때의 IoU threshold.
    - `save_csv_path`: 결과를 저장할 CSV 파일 경로.

명령어
```bash
python detect.py --weights weights/best.pt --source data/test --save-csv-path output.csv
```