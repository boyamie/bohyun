---
{"dg-publish":true,"permalink":"/data-centric-cv/data-augmentation/"}
---

# RandomRotation
[RandomRotation — Torchvision main documentation](https://pytorch.org/vision/main/generated/torchvision.transforms.RandomRotation.html?highlight=randomrotation#torchvision.transforms.RandomRotation)
torchvision.transforms 모듈의 클래스를 통해 임의로 이미지를 회전시키기
#### 매개변수
- **degree** ( _시퀀스_ _또는_ _숫자_ ) – 선택할 degrees 범위. degree가 (min, max)와 같은 시퀀스가 ​​아닌 숫자인 경우 도 범위는 (-degrees, +degrees)가 됩니다.
    
- **보간** ( _InterpolationMode_ ) – interpolation에 의해 정의된 원하는 보간 열거형 `torchvision.transforms.InterpolationMode`입니다. 기본값은 . 입력이 Tensor인 경우 , `InterpolationMode.NEAREST`만 지원됩니다. 해당 Pillow 정수 상수(예: PIL.Image.BILINEAR) 도 허용됩니다.
    
- **expand** ( [_bool_](https://docs.python.org/3/library/functions.html#bool "(Python v3.13에서)") , _optional_ ) – 선택 사항인 확장 플래그입니다. true이면 출력을 확장하여 회전된 전체 이미지를 담을 수 있을 만큼 크게 만듭니다. false이거나 생략하면 출력 이미지를 입력 이미지와 같은 크기로 만듭니다. expand 플래그는 중심을 중심으로 회전하고 변환은 없다고 가정합니다.
    
- **center** ( _sequence_ _,_ _optional_ ) – 회전의 선택 사항 중심, (x, y). 원점은 왼쪽 위 모서리입니다. 기본값은 이미지의 중심입니다.
    
- **fill** ( _시퀀스_ _또는_ _숫자_ ) – 회전된 이미지 외부 영역에 대한 픽셀 채우기 값입니다. 기본값은 0입니다. 숫자가 주어지면 값은 각각 모든 밴드에 사용됩니다.

# ColorJitter
#### 매개변수
- **brightness** ( [_float_](https://docs.python.org/3/library/functions.html#float "(Python v3.13에서)") _또는_ _python:float_ _(_ _min_ _,_ _max_ _)_ 의 튜플 ) – 밝기를 얼마나 흔들어야 하는지. 밝기_인자는 [max(0, 1 - 밝기), 1 + 밝기] 또는 주어진 [min, max]에서 균일하게 선택됩니다. 음수가 아니어야 합니다.
    
- **contrast** ( [_float_](https://docs.python.org/3/library/functions.html#float "(Python v3.13에서)") _또는_ _python:float_ _(_ _min_ _,_ _max_ _)_ 의 튜플 ) – 대비를 얼마나 지터할 것인가. contrast_factor는 [max(0, 1 - contrast), 1 + contrast] 또는 주어진 [min, max]에서 균일하게 선택됩니다. 음수가 될 수 없습니다.
    
- **saturation** ( [_float_](https://docs.python.org/3/library/functions.html#float "(Python v3.13에서)") _또는_ _python:float_ _(_ _min_ _,_ _max_ _)_ 의 튜플 ) – saturation을 얼마나 흔들어야 하는지. saturation_factor는 [max(0, 1 - saturation), 1 + saturation] 또는 주어진 [min, max]에서 균일하게 선택됩니다. 음수가 아니어야 합니다.
    
- **hue** ( [_float_](https://docs.python.org/3/library/functions.html#float "(Python v3.13에서)") _또는_ _python:float_ _(_ _min_ _,_ _max_ _)_ 의 튜플 ) – hue를 얼마나 흔들어야 하는지. hue_factor는 [-hue, hue] 또는 주어진 [min, max]에서 균일하게 선택됩니다. 0<= hue <= 0.5 또는 -0.5 <= min <= max <= 0.5여야 합니다. hue를 흔들려면 HSV 공간으로 변환하기 위해 입력 이미지의 픽셀 값이 음수가 아니어야 합니다. 따라서 이 함수를 사용하기 전에 이미지를 음수 값으로 정규화하거나 음수 값을 생성하는 보간을 사용하면 작동하지 않습니다.

# Bounding box
vertices: 텍스트 영역을 감싸는 점들. <numpy.ndarray, (8,)>
theta : 회전 각도 [radian]
-> output: rotated vertices <numpy.ndarray, (8,)>

1. 주어진 점들인 `vertices`를 (2, 4) 형태로 변환해 회전 중심을 anchor로 지정하여 회전시키는 방법
   v생성: 형태 변환
      ```python
   # vertices를 (2, 4) 형태로 변환하여 v에 할당 
   v = vertices.reshape(4, 2).T
```
2. 회전 변환 적용
   - anchor 원점으로 이동
    `v_centered = v - anchor`
   - 회전행렬을 사용해 회전
    `v_rotated = rotate_mat @ v_centered`
   - 회전 후 원래 위치로 이동
    `res = v_rotated + anchor`
   
