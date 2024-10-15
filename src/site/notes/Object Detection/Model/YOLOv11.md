---
{"dg-publish":true,"permalink":"/object-detection/model/yol-ov11/"}
---


### YOLOv11의 주요 기능
1. **향상된 특징 추출**
   YOLOv11은 개선된 백본과 넥 아키텍처를 사용하여 특징 추출 능력을 크게 향상시킵니다. 이를 통해 더 복잡한 시각 작업도 정확하게 처리할 수 있습니다.

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