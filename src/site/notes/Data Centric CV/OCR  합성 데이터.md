---
{"dg-publish":true,"permalink":"/data-centric-cv/ocr/"}
---

합성 데이터 제작은 원본 데이터 없이 새로운 데이터를 인공적으로 만들어내는 기법으로, 적은 비용으로도 비교적 좋은 품질의 훈련데이터를 확보할 수 있다.
# TextRecognitionDataGenerator
 TextRecognitionDataGenerator는 text recognition data의 합성을 염두에 두고 제작된 패키지이다.detection/localization task엔 어떻게 활용할 수 있을까?
 - ikipedia 검색, 랜덤 텍스트 생성 등 다양한 방법으로 텍스트 이미지를 생성할 수 있다.

#### 🌳🔬Treescope
Treescope란 Jupyter Notebook에서 변수를 더 보기 좋게 출력하기 위해 Google에서 개발한 패키지이다.
잘 정돈된 데이터라 하더라도 가공 없이 데이터를 읽고 파악하는 것은 어려운 일이다. 🚀 Treescope를 사용하면 그 어려움을 조금이나마 덜 수 있다.

- 문서 생성 시 얻은 단어의 크기/위치 정보를 활용해 bounding box(bbox)도 문서 위에 그리기
  ```python
# 화면 왼쪽에 생성된 문서 이미지를 출력

plt.subplot(1, 2, 1)

plt.imshow(document['image'])

plt.axis('off')

plt.title("generated document")

# 화면 오른쪽에는 문서 이미지에 bounding box를 그린 이미지를 출력

plt.subplot(1, 2, 2)

draw_bbox(document)

plt.title("document w. bboxes")
```

- 다양하지 않은 글꼴, 명암, 대비, 색조 등 여러가지 원인을 고려하여 카메라의 시점(=위치&방향)을 바꾸어 조금 더 다양한 데이터를 만들기
  [OpenCV: Geometric Image Transformations](https://docs.opencv.org/4.x/da/d54/group__imgproc__transform.html)
  `cv2.getPerspectiveTransform`
- 시점변환 행렬 M을 이용해 bbox 좌표를 변환하는 함수 구현
  [Lesson 4: Perspective projection · ssloy/tinyrenderer Wiki · GitHub](https://github.com/ssloy/tinyrenderer/wiki/Lesson-4:-Perspective-projection#homogeneous-coordinates)
  