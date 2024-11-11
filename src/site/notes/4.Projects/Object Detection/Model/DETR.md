---
{"dg-publish":true,"permalink":"/4-projects/object-detection/model/detr/"}
---

- [x] Debug
수현님이 작성하신 cascade_rcnn_config.py를 참고해서 detr_config 파일을 만들었다.
두 모델의 **데이터 처리 파이프라인**이 다르게 구성되어 있기 때문에 error가 발생했다. 
두 모델 간의 차이점이 있어서 error가 발생한걸까?

### 1. **DETR과 Cascade R-CNN의 차이점**
- **Cascade R-CNN**은 **2-Stage Detector**이다. 
	첫 번째 단계에서 **Region Proposal Network(RPN)** 을 통해 관심 영역(Region of Interest, RoI)을 찾아내고, 두 번째 단계에서 RoI를 이용해 객체를 분류하고 박스를 조정한다.
	이 과정에서 데이터 처리 파이프라인에서의 이미지 변환이 잘 정의되어 있고, **`RandomFlip`** 같은 변환이 문제가 되지 않았다.

- **DETR**(Detection Transformer)은 **End-to-End** 방식의 **1-Stage Detector**로, **Transformer** 구조를 기반으로 한다.(RPN)이 필요없다.
	이 모델에서는 이미지의 크기 조정(`img_scale`)이 직접적으로 이미지 특징 추출과 학습에 중요하다.
	DETR의 경우 `Resize`는 이미지의 크기를 조정하는 단계에서만 사용되어야 하며, **`RandomFlip`** 같은 이미지 변환에서는 `img_scale`이 필요없다.
### 2. **왜 DETR에서는 Error가 발생하는가?**
**Cascade R-CNN**의 파이프라인에서는 **`RandomFlip`** 과 **`Resize`** 가 별도의 역할을 하며, 이러한 변환이 이미지 크기나 비율과 관련된 오류를 발생시키지 않았다. 
**`RandomFlip`** 은 단순히 이미지를 좌우로 뒤집는 변환일 뿐, 이미지 크기를 다루지 않았다.

**DETR**에서는 `img_scale`과 같은 인자가 잘못된 위치에서 사용되면, 모델이 데이터를 처리하는 과정에서 오류가 발생한다. **`RandomFlip`** 변환에서 `img_scale`은 불필요한 매개변수인데, DETR에서는 이 매개변수가 전달되었기 때문에 오류가 발생한 것이다. (오류 메시지에 불필요한 매개변수가 입력되었다고 되어있었다.)

DETR은 **Transformer** 구조를 사용하기 때문에, 입력되는 이미지 크기 및 비율이 매우 중요한 역할을 한다. 
따라서 **이미지 크기 변환(Resize)** 은 별도로 명확하게 처리되어야 하고, **RandomFlip** 와 같은 변환에서는 크기 변환과 관련된 인자를 포함하지 않아야 한다.