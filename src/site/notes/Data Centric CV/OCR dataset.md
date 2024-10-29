---
{"dg-publish":true,"permalink":"/data-centric-cv/ocr-dataset/"}
---


![](https://i.imgur.com/tewbgju.png)
대량의 합성데이터로 학습을 먼저한 뒤, 실제 데이터로 빈틈을 메꿔주는 hybrid방식이 있다.
![](https://i.imgur.com/mnZt6vY.png)
크롤링 할 때 license 관련된 문제가 있다.

## 데이터 수집방법 별 특징
![](https://i.imgur.com/mITMgEv.png)

## Dataset 수집방법
![](https://i.imgur.com/e237SWa.png)

데이터셋을 볼 때 확인해야할 부분
![](https://i.imgur.com/LPvWILl.png)
어떤 기준에 맞춰 정리할지 정하기
![](https://i.imgur.com/sk9EHVa.png)

## ICDAR (공개 dataset) OCR관련 학회
새로운 데이터셋을 구축하기도 하고, 재가공하기도 한다.
![](https://i.imgur.com/1Xa9vnY.png)
![](https://i.imgur.com/W84rzdg.png)
2019년에는 세로쓰기, 휘어진글자까지 포함한다.
icdar 2019년에는 한국어데이터도 포함되있는걸 확인할 수 있는데 개발한 OCR의 성능을 확인할 수 있다.
![](https://i.imgur.com/FHkbQAf.png)
![](https://i.imgur.com/66Bq0jh.png)

## AI HUB
무료로 공개된 공공행정문서 등이 있음
![](https://i.imgur.com/jnyqN2Q.png)
다른 데이터셋은 4개 좌표값을 알려줬었는데 여기선 json파일 형태가 다르다.

![](https://i.imgur.com/pCfdsSV.png)
오른쪽 아래엔 meta data도 함께 제공한다.

![](https://i.imgur.com/6BvigMK.png)
문서 OCR 데이터셋 아래 네가지는 영어 data다.

#### CORD
![](https://i.imgur.com/vweS6PL.png)
계층 구조로 parsing함.

## UFO (Upstage Format for OCR)
![](https://i.imgur.com/MX8duL7.png)
통일된 포멧의 하나의 예시.

![](https://i.imgur.com/NoN7Sjc.png)
가장 큰 특징은 위계를 따라가는 것이 아니라 바로 image에서 원하는 정보를 얻을 수 있다는 점이다.

#### UFO format
![](https://i.imgur.com/e806KF4.png)
orientation: 가로쓰기, 세로쓰기, 불규칙
tag: 음각 양각, 거울에 비침, 가려짐, 초점이 맞지 않음
![](https://i.imgur.com/UkllyS4.png)

## EDA
![](https://i.imgur.com/WQvRjas.png)
종횡비가 생각보다 영향을 많이 미친다.
고득점을 위해선 종횡비가 매우 큰 경우를 어떻게 처리해야할지 고민해보아야한다.
![](https://i.imgur.com/OYyfMJR.png)

## OCR 성능평가
![](https://i.imgur.com/oGIN8ZP.png)
![](https://i.imgur.com/ljFXAp3.png)

## Text Detection
step1
![](https://i.imgur.com/OjXyPu1.png)
step2
![](https://i.imgur.com/oNLlxtD.png)
scoring: 매칭행렬로부터 예측과 정답 사이의 유사도를 하나의 수치로 뽑아내는 것

## 글자 검출 모델 평가
![](https://i.imgur.com/wxdRTSx.png)
매칭 행렬 계산 + 유사도 계산


# Glossary
1. IoU: 두 영역간의 유사도를 계산
2. Recall(정답 기준), Precesion(예측 기준)

#### 기본 용어
![](https://i.imgur.com/327qp3X.png)
One-to-One
One-to-Many(split case)
Many-to-One

## DetEval
![](https://i.imgur.com/homgp9l.png)
One-to-One과 Many-to-One는 1의 값을 사용한다.
One-to-Many(split case)는 바람직하지 않은 경우라서 약간의 페널티가 들어간 0.8로 바꾼다.
![](https://i.imgur.com/9lBXYQ7.png)
![](https://i.imgur.com/lyLbz1j.png)
한계는 merge는 허용하고 split은 방지하는게 맞는가? 하는 의문점이다.
![](https://i.imgur.com/kzeMFWg.png)

## IoU
![](https://i.imgur.com/o5wCj54.png)
One-to-Many: 0점

## TIoU
![](https://i.imgur.com/58f0c7l.png)
얼마나 정답에 가깝게 예측되었나를 중요하게 여기는 IoU
![](https://i.imgur.com/8azXKAh.png)
이걸 하나의 값(scoring)으로 합쳐야하기 때문에 최종 수치는
![](https://i.imgur.com/o2GtLfC.png)
recall과 precision의 조화평균을 구하게 된다.

#### 예제를 통한 TIoU의 한계점
![](https://i.imgur.com/WYbZj0b.png)

## CLEval
글자 수 기반
![](https://i.imgur.com/QS7X71p.png)
글자-level 평가다.
글자 영역 위치 정보와 sequence로 부터 추정하는게 중요하다.
![](https://i.imgur.com/uxxT3TO.png)
![](https://i.imgur.com/BCLKega.png)
글자 중심 위치를 구한 것이 PCC이다.
![](https://i.imgur.com/qRL3nf4.png)
정답 영역 기준이므로 Recall이라고 부를 수 있다.

2개의 예측 영역이 있을 때,
![](https://i.imgur.com/JfZaIWS.png)
![](https://i.imgur.com/hmi3jli.png)
![](https://i.imgur.com/qgOoRa5.png)
이건 예측 영역 기준이므로 Precision이다.
![](https://i.imgur.com/qx1cbWt.png)

## Summary of metrics
![](https://i.imgur.com/XXiMxRC.png)
하나의 정량평가를 이용하지 않고 여러개의 정량평가를 이용하는 게 중요하다.
정성평가도 함께 하면서 정량평가 방법이 이상한건 아닌지 의심해보아야한다.