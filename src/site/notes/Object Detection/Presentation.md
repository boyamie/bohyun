---
{"dg-publish":true,"permalink":"/object-detection/presentation/"}
---

![](https://i.imgur.com/4UZzVAP.png)
안녕하세요, 21조의 김보현입니다. 
Contrallable Diffusion Models를 활용한 데이터 증강을 중점으로 프로젝트 협업 방식, 솔루션 및 아이디어, 실험 내용을 공유하고자 합니다.
![](https://i.imgur.com/H1QF0XI.png)
발표는 크게 네 가지 주제로 구성되어 있습니다. 
첫째, 프로젝트 협업 방식과 과정, 둘째, 저희가 채택한 솔루션 및 아이디어, 셋째, 실험 내용 및 과정, 마지막으로 결과입니다.
![](https://i.imgur.com/deJjeb6.png)
프로젝트의 첫 단계는 협업 도구 설정과 규칙 정립이었습니다. 
첨부한 깃 그래프는 project기간동안 저희조 팀원들이 feature branch를 파고 merge한 내용을 담고있는 main branch입니다.
![](https://i.imgur.com/QHuzPlC.png)
저희는 GitHub, Slack, GitHub Bot을 적극 활용하여 이슈와 PR을 관리하고, 서버 사용 현황을 실시간으로 공유했습니다. 
github issue나 pr을 확인해달라는 말을 하지 않아도 슬랙과 연동해서 팀원들에게 자동으로 알림이 와서 효율적인 협업을 할 수 있었습니다.
![](https://i.imgur.com/h0WOFr5.png)
특히, 메인 브랜치에 직접 커밋하지 않는 rule을 세웠습니다. commit 컨벤션 규칙을 세워서 VSCode Extension의 gitmoji를 설치해서 commit메시지를 보낼 때 gitmoji를 적극 활용했습니다.
![](https://i.imgur.com/XywyDcB.png)
이슈별로 branch를 나누어 작업한 뒤 코드 리뷰를 거쳐 merge하는 방식으로 진행했습니다.
issue 목적에 따른 isuue label도 커스텀해서 목적에 따라 한눈에 알아볼 수 있도록 했습니다.
이를 통해 프로젝트의 진행 상황을 체계적으로 관리하고 팀원 간의 협업을 강화할 수 있었습니다.
![](https://i.imgur.com/JmkwzQM.png)
EDA 단계에서는 Ultralytics를 사용해 데이터의 시각화를 수행했습니다. 
또한 각 라벨의 분포와 데이터 bias를 분석했습니다.
![](https://i.imgur.com/3PD2WzU.png)
ultralytics를 활용해서 validation에선 예측값과 실제 라벨을 비교해서 confidence score도 시각화했습니다.
이를 보완하기 위한 증강 전략을 수립했습니다.
![](https://i.imgur.com/AUl7hdq.png)
가설을 세울 땐 hypothesis라벨을 등록해서 issue에 적었습니다.
가설을 feature-branch에서 구현하고, 실험 후 PR을 통해 reviewer에게 코드 리뷰를 받고 나서 메인 브랜치에 merge하는 일련의 과정을 반복했습니다.
저희 repository에서 모든 issue를 보실 수 있습니다.
그 중 몇가지를 소개하겠습니다.
![](https://i.imgur.com/Nx8HrWi.png)
10시 데일럼스크럼에서 세운 가설을 바탕으로 3가지 실험을해서 issue를 만들고 pr을 날려서 시각화했습니다.
![](https://i.imgur.com/aXcp5Hj.png)
또한 아이디어가 떠오르면 issue를 만들고 실험의 결과를 체계적으로 비교하고 개선 방향을 제시할 수 있었습니다.
![](https://i.imgur.com/pGylein.png)
**Controllable Diffusion Model**을 사용한 생성 기반 Data augmentation을 했습니다.
1번 **Visual Prior Generator** 에서는 HED edge 검출기를 사용해 시각적 프라이어를 추출합니다. 
1번에서 생성된 이미지 주석 쌍을 활용해서 2번 **Prompt Constructor** 는 주석 정보를 기반으로 프롬프트를 생성하고, 모든 카테고리 라벨을 문장으로 결합합니다.
3번 **Controllable Diffusion Model** 은 주석된 이미지-프롬프트 쌍으로부터 합성 이미지를 생성합니다. 
![](https://i.imgur.com/zk40f2i.png)
![](https://i.imgur.com/mNT8xgz.png)
생성 이미지 크기를 조정했더니 고품질의 이미지가 생성되는 것을 확인했습니다.
![](https://i.imgur.com/Ujmct8j.png)
그러나 contrallable diffusion model은 small, medium사이즈의 object 생성에는 어려움을 겪었습니다.
그 결과 diffusion image를 추가해서 train을 하면 성능이 하락되는 것을 확인했습니다.
![](https://i.imgur.com/K0lRSHo.png)
TTA(Test Time Augmentation)와 Diffusion Augmentation을 통해 성능 향상을 확인했습니다. Swin Model과 Faster-RCNN 모델에서 각각 mAP50 성능이 +0.0558, +0.0742만큼 향상되었습니다. 그러나 일부 경우에서는 작은 객체에 대한 성능 저하가 나타나기도 했습니다.
![](https://i.imgur.com/uOvApjk.png)
Ultralytics를 활용해 다양한 실험을 수행했습니다. 
특히 Ultralytics Docs에 여러 기능이 상세하게 나와있었고 독스를 읽는 것 만으로도 파이프라인 최적화에 큰 도움이 되었습니다.
![](https://i.imgur.com/Hubd5uL.png)
첫째 주에는 가설 설정과 데이터 시각화를 시작으로, 둘째 주에는 파이프라인 최적화와 모델 추가를 진행했습니다. 마지막 주에는 Diffusion Augmentation, 앙상블 전략, Pseudo Labeling 등의 다양한 접근 방식을 실험하며 최종 결과를 도출했습니다.
![](https://i.imgur.com/TNMBFjs.png)
이상으로 발표를 마치겠습니다. 이번 발표를 통해 Diffusion Models의 가능성과 한계에 대해 다뤘으며, 프로젝트 경험을 바탕으로 얻은 인사이트와 독특한 아이디어를 공유했습니다. 질문이 있으시면 언제든지 말씀해 주세요.