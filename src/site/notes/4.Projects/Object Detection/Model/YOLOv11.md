---
{"dg-publish":true,"permalink":"/4-projects/object-detection/model/yol-ov11/"}
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

**YOLOv11x**와 **YOLOv11n**은 같은 YOLOv11 모델의 두 가지 변형으로, 각각 다른 목적에 최적화되어 있습니다. 이 중 어느 모델을 사용할지는 **성능 요구사항**과 **리소스 제약**에 따라 결정됩니다.

### YOLOv11n (Nano) vs. YOLOv11x (Extra-large)

1. **YOLOv11n (Nano)**
   - **특징**: YOLOv11n은 가장 가벼운 모델로, 매우 빠른 속도를 제공하면서도 비교적 적당한 정확도를 목표로 설계되었습니다.
   - **적용 사례**: **실시간 처리**나 **경량화된 장치**에서 주로 사용됩니다. 예를 들어, **모바일 장치**, **엣지 컴퓨팅 환경**, **임베디드 시스템** 등에서 실시간으로 객체를 탐지해야 하는 경우에 적합합니다.
   - **장점**: 빠른 처리 속도, 낮은 계산 자원 요구(메모리 및 GPU/CPU 사용량이 적음).
   - **단점**: 상대적으로 **낮은 정확도**.

2. **YOLOv11x (Extra-large)**
   - **특징**: YOLOv11x는 가장 큰 변형으로, 높은 **정확도**를 목표로 설계되었습니다. 더 깊고 복잡한 네트워크 구조를 가지고 있어 더 많은 파라미터와 계산을 수행합니다.
   - **적용 사례**: 고성능 하드웨어(GPU)를 활용할 수 있으며, **최고의 정확도**를 요구하는 **고해상도 이미지** 또는 **복잡한 객체 탐지** 작업에 적합합니다. 자원이 충분한 상황에서 **정밀한 분석**을 수행할 때 적합합니다.
   - **장점**: 더 높은 **정확도**와 **성능**.
   - **단점**: **계산 비용**과 **처리 시간**이 많이 소요됨. 리소스가 부족한 환경에서는 사용이 어려울 수 있음.

### YOLOv11x를 사용해야 하는 이유
1. **정확도**가 매우 중요한 경우: YOLOv11x는 YOLOv11 모델 중 가장 높은 정확도를 제공합니다. 특히, 다양한 클래스 간 구분이 필요한 복잡한 객체 탐지 작업에 적합합니다.
2. **고성능 GPU**가 있는 경우: YOLOv11x는 처리 시간이 길고 더 많은 자원을 소모하지만, 고성능 하드웨어를 활용할 수 있다면 매우 강력한 성능을 발휘합니다.
3. **비실시간 작업**: 처리 속도보다는 **정확한 탐지**가 중요한 경우, 예를 들어 **이미지 후처리**, **비디오 분석** 등의 작업에 적합합니다.

### YOLOv11n을 고려해야 하는 경우
1. **리소스가 제한적인 환경**: 만약 **모바일**이나 **엣지 컴퓨팅** 환경에서 실시간으로 빠른 추론이 필요하다면, YOLOv11n은 훌륭한 선택입니다. 속도가 빠르고 경량화되어 있어 저사양 하드웨어에서도 원활하게 작동합니다.
2. **실시간 애플리케이션**: 매우 빠른 속도와 **낮은 지연 시간**이 중요한 경우, YOLOv11n은 더 나은 선택이 될 수 있습니다. 예를 들어, **실시간 객체 추적**이나 **영상 스트리밍 중 객체 탐지**와 같은 상황에서 유리합니다.

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

YOLOv11은 기존의 YOLO 기반 모델과 다를 수 있지만, YOLO 스타일 모델에서는 기본적으로 `inference.py` 스크립트를 사용하여 훈련된 모델을 불러오고 이미지나 비디오에서 객체를 감지한 후 결과를 저장할 수 있습니다. 

다음은 YOLOv11을 위한 `inference.py` 스크립트 예시로, 모델을 사용하여 테스트 이미지를 감지하고 그 결과를 CSV 파일로 저장하는 코드입니다. 이 코드는 기본적으로 YOLOv5의 흐름을 기반으로 하지만, YOLOv11의 구조에 맞게 수정하였습니다.

### `inference.py` (YOLOv11 기반)
### 주요 포인트:
1. **모델 로드**: `torch.load`를 사용하여 YOLOv11 모델을 로드합니다. YOLOv11에 맞는 모델 경로 및 구조를 사용합니다.
2. **데이터셋 로드**: `LoadImages`는 이미지 데이터를 로드하는 역할을 하며, 이미지나 비디오를 불러옵니다.
3. **추론 및 NMS 적용**: 모델 추론 결과에 대해 `non_max_suppression`을 적용하여 중복된 감지 결과를 제거합니다.
4. **결과 저장**: 추론 결과를 `pandas.DataFrame`으로 만들어 CSV 파일로 저장합니다.

### 사용 방법:
1. 터미널에서 아래 명령어를 실행하여 모델을 사용해 객체를 감지하고 결과를 CSV 파일로 저장합니다.
   ```bash
   python detect.py --weights weights/best.pt --source data/test --save-csv-path output.csv
   ```

이 코드에서 `models.yolo`, `utils.datasets`, `utils.general`, `utils.torch_utils`가 YOLOv11에서 적합한지 확인한 후 경로와 모듈명을 수정하거나 해당 모듈을 사용해야 합니다.