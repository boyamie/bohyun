---
{"dg-publish":true,"permalink":"/data-centric-cv/cvat/"}
---

computer vision annotation tool로 매뉴얼이 부족해 어려움을 겪었다.
따라서 내가 직접 annotation을 하며 그 과정을 기록하고자 한다.
#### CVAT 지원 기능
![](https://i.imgur.com/C25bmpx.png)
#### docker -> CVAT 시행착오
![](https://i.imgur.com/qSnfl2K.png)
처음엔 CVAT 공식 문서를 보고 docker에서 cvat를 git clone해와서 작업했지만
super계정을 만들기 까진 성공했는데, 어쩐일인지 local host가 열리지 않았다..
![](https://i.imgur.com/SWZo21X.png)
[Computer Vision Annotation Tool](https://app.cvat.ai/)
docker를 쓰지 말고 웹 기반에서 해보라고 하셔서 들어가니까 docker보다 훨씬 쉬웠다..! 감사해요 기훈님
#### CVAT 작업  유형
1. cvat.org 서버 기반 작업
	• 500MB 의 Image Upload 제한
2. Local Install 기반 작업
   로컬에 웹서버를 구성하여 작업
zip 안에는 CVAT에서 허용하는 format(jpeg,png 등)만 들어있어야한다.

#### relabeling 가이드라인
- [ ] 배경에 bbox가 생기는 문제
- [ ] QR 코드에 bbox가 생기는 문제
- [ ] bbox가 text보다 크거나 작게 그려지는 문제
이 세가지 문제를 해결하는데 중점을 두고 relabeling을 진행했다.
## 1. Create a new task
![](https://i.imgur.com/DQPEO7T.png)
task/create a new task를 눌러준다.
![](https://i.imgur.com/4ydieTL.png)
나는 라벨이 하나만 필요해서 하나만 만들었다.
label을 만들고 continue를 눌러준다.
그리고 submit & open을 눌러준다.
![](https://i.imgur.com/6dHZnpL.png)
그러면 task가 생성된다.
![](https://i.imgur.com/bkUsLId.png)

영수증 이미지에서 글자만 검출하는 작업이기 때문에 Rectangle과 Polygon 중 하나를 선택하기로 했는데 영수증이 기울어지거나 구겨진 경우가 있어서 polygon을 선택했다.

- Rectangle은 글자 영역을 사각형으로 쉽게 지정할 수 있으며, 직사각형 형태로 라벨링할 수 있는 경우 간단하게 사용
- **Polygon**: 글자가 직사각형 외의 다양한 형태로 배치된 경우, 좀 더 정교하게 영역을 지정할 수 있다. 글자가 곡선 형태이거나 비정형적인 레이아웃으로 배치된 경우에는 Polygon을 선택
![](https://i.imgur.com/mB0iOsU.png)
몇 초간 만들어진다..
![](https://i.imgur.com/AipQ6Cf.png)
이제 task에 들어가면 내가 만든 task를 확인할 수 있다!
이건 중국어로 된 이미지들이다.
![](https://i.imgur.com/SL6N4Od.png)
### **라벨링 시작**

- 화면 하단의 `Jobs` 섹션에서 작업(Job)을 클릭하여 작업 화면으로 들어간다.
- 작업 화면에서 이미지에 대해 라벨링 작업을 시작할 수 있다.
![](https://i.imgur.com/1mbbbOn.png)
### 1. **Random**
- 프로젝트 전체에서 무작위로 프레임을 선택하여 단일 작업을 생성한다.
- 특정 비율의 프레임을 선택할 때, 한 번의 작업에 대한 무작위 샘플링을 사용한다.
- 이 옵션은 단일 작업 내에서 모든 프레임이 임의로 선택될 때 적합하다.

### 2. **Random per job**
- 여러 개의 작업을 생성할 때, 각 작업마다 개별적으로 무작위로 프레임을 선택한다.
- 예를 들어, 여러 명의 주석자가 작업에 참여할 때 각 작업에 고유한 무작위 프레임이 필요하다면 이 옵션을 선택하면 좋다.
- 여러 작업에서 동일한 프레임이 중복되지 않도록 개별적으로 샘플링이 필요할 때 유용하다.

![](https://i.imgur.com/sw4mLNz.png)
- **Quantity %**: 작업에 포함될 이미지의 비율이다.
- **Frame Count**: 한 작업에 포함될 프레임 수다. 글자 인식만 필요하기 때문에 4로 했다.
시드를 비워두면 매번 다른 무작위 프레임이 선택된다.  작업 생성 시마다 새로운 무작위 샘플이 선택돼도 괜찮아서 비워뒀다.

![](https://i.imgur.com/MIZI0X3.png)


![](https://i.imgur.com/dSMFxPg.png)

### 2. **라벨링 툴 선택**

- 작업 화면의 상단 도구 모음에서 적절한 라벨링 도구(예: `Rectangle`, `Polygon`, 또는 `Polyline`)를 선택합니다.
- 영수증의 글자 영역을 선택하여 라벨링합니다.

### 3. **라벨 지정**

- 각 글자나 단어 영역을 지정한 후, 오른쪽 사이드바에서 해당 라벨(예: `receipt`)을 선택해 할당합니다.

### 4. **라벨링 저장**

- 모든 라벨링이 완료되면, `Save` 버튼을 클릭하여 작업 내용을 저장합니다.

### 5. **결과 내보내기**

- 작업이 완료된 후, `Export` 기능을 사용하여 라벨링 데이터를 원하는 포맷(JSON 등)으로 내보냅니다. 이 데이터를 나중에 `pickle` 포맷으로 변환하여 훈련에 사용할 수 있습니다.
