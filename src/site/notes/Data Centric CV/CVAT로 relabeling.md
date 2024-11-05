---
{"dg-publish":true,"permalink":"/data-centric-cv/cvat-relabeling/"}
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
그러면 task가 생성된다. 몇분 기다려주면 아래와 같은 화면이 뜬다.
![](https://i.imgur.com/bkUsLId.png)
여기서 아래쪽의 `Job #1441474` 를 누르면 작업을할 수 있는 환경으로 이동한다.

annotation작업을 할 때 가장 중요한건 bbox를 겹치지 않게 해야 한다는 점이다.
![](https://i.imgur.com/cPzeUNx.png)
Draw new polygon을 눌러서 원하는 points 개수를 설정해준다.
여기서 주의할 점은 track이 아닌 shape를 눌러줘서 이 프레임에서만 작업해줘야 한다는 점이다.
(실제로 나는 처음에 track으로 해봤다가 다시 delete하고 재작업을 하는 뼈아픈 경험을 했다.)
기울어진 글자의 경우 10points를 정도를 선택하면 글자를 정확하게 annotation할 수 있다!
![](https://i.imgur.com/m8ARaHF.png)
자 이렇게 한 프레임이 끝났으면 다음 프레임으로 넘어간다.
![](https://i.imgur.com/uC481Mf.png)
CVAT의 가장 큰 장점은 이렇게 기울어진 글자에도 정확히 bbox를 만들 수 있다는 점이다.