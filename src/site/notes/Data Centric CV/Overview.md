---
{"dg-publish":true,"permalink":"/data-centric-cv/overview/"}
---

OCR (Optical Character Recognition) 기술은 사람이 직접 쓰거나 이미지 속에 있는 문자를 얻은 다음 이를 컴퓨터가 인식할 수 있도록 하는 기술로, 컴퓨터 비전 분야에서 현재 널리 쓰이는 대표적인 기술 중 하나입니다.
![](https://i.imgur.com/4rjEUks.png)
OCR은 글자 검출 (text detection), 글자 인식 (text recognition), 정렬기 (Serializer) 등의 모듈로 이루어져 있습니다.
- 본 대회에서는 다국어 (중국어, 일본어, 태국어, 베트남어)로 작성된 영수증 이미지에 대한 OCR task를 수행합니다.
    
- 본 대회에서는 글자 검출만을 수행합니다. 즉, 이미지에서 어떤 위치에 글자가 있는지를 예측하는 모델을 제작합니다.
    
- 본 대회는 제출된 예측 (prediction) 파일로 평가합니다.
    
- 대회 기간과 task 난이도를 고려하여 코드 작성에 제약사항이 있습니다. 상세 내용은 Data > Baseline Code (베이스라인 코드)에 기술되어 있습니다.
    
- 모델의 입출력 형식은 다음과 같습니다.
    
    - 입력 : 글자가 포함된 JPG 이미지 (학습 총 400장, 테스트 총 120장)
        
    - 출력 : bbox 좌표가 포함된 UFO Format (상세 제출 형식은 Overview > Metric 탭 및 강의 6강 참조)
        

자세한 내용은 AI Stages에서 확인해주세요.

대회 플랫폼 AI Stages 링크: [https://stages.ai/en/competitions/315/](https://stages.ai/en/competitions/315/)