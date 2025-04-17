---
{"dg-publish":true,"permalink":"/3-d/ne-rf/"}
---

[Representing Scenes as Neural Radiance Fields for View Synthesis](https://arxiv.org/abs/2003.08934)
[Implement](https://www.matthewtancik.com/nerf)
5D scene representation기반 + parameter 조절 + Volume Rendering -> View Synthesis
1. 특정 시점에서 이미지를 얻는다. 
2. object를 다양한 시점에서 바라보았을 때를 예측한다. 
3. 2D 물체를 3D로 보이게 한다.

Deep Convolutional Layer대신 MLP(Multi-layer Perceptron)를 사용했다. 
Radience Field를 구성하는 모델은 Network의 input으로 spatial loaction (x,y,z)와 viewing direction (θ,φ)로 이루어진 5D의 연속된 좌표 값을 구성하고, output은 volume density와 view에 따라 방출되는 radiance로 설정한다. 

5D 좌표는 Camera rays를 따라 5D 생성한다. volume rendering을 통해 output colors(RGB)와 density로 object를 rendering한다.
### Neural Radiance Field Scene Representation
F:(x,d)→(c,σ)
![](https://i.imgur.com/bCGxeFt.png)
input: Camera ray 의 3D location (x,y,z)과 2D viewing direction (θ,φ) -> 5D vector
(3D Cartesian unit vector d로 표현되기 때문에 6개의 input을 지닌 함수라고 표현해도 된다.)
output은 color (r,g,b) and volume density σ
**density**
multiview에서 consistent한 image를 생성하기 위해 density σ는 location input인 x의 영향만 받게 하고, color는 모든 input의 영향을 다 받게했다.
해당 위치를 통과하는 빛의 양에 따라 radiance가 얼마나 얻어지는지를 나타낸다. 
위치와 방향을 나타내는 5D 좌표(x,y,z,θ,ϕ)로부터 single volume density와 view 기반의 RGB color를 추정한다.
#### Radiance Field
![](https://i.imgur.com/rhm0Vtn.png)
point -> 다양한 ray가 지난다 -> 지나는 빛의 방향에 따라 다양한 radiance를 가진다
다양한 radience를 distribution으로 representation : NeRF !
(c)에 point에서 radiance distribution이 보이는데 color(radiance)
누적투영도


### Volume Rendering with Radiance Fields
5D NeRF
1 scene = volume density + directional emitted radiance 로 representation
ray의 radiance는 지나가는 scenec에 대해서 volume rendering을 통해 정해진다.
C(r)은 ray의 예측되는 radiance다.
![](https://i.imgur.com/XNSy5Te.png)
r(t)=o+td
t의 범위인 $t_n$, $t_f$ 은​ near한 곳부터 가장 far한 곳 까지 지나가는 ray의 범위다.

 T(t)
Density에 대한 constraint function
ray가 point에 닿기 전까지 지나온 점들의 Density값을 조사해 해당 점이 최종 radiance에 영향을 끼치는 결과를 반영하기 위해 곱해주는 것 같다.
연속적인 식은 MLP가 이산적인 결과값을 제공하고 이미지와 Voxel 역시도 이산적인 data이기 때문에 Integral 기반의 식을 사용하는데 문제가 생길 수가 있다. 그래서 식을 이산형으로 변환한다. 
![](https://i.imgur.com/o7sVuUS.png)
$t_n$, $t_f$ 를 N으로 나누어서 계산을 했을 때 각 $t_i​$이다.
![](https://i.imgur.com/RvBZ3Ns.png)
C(r)도 적분이 시그마로 바뀌면서 위의 식으로 표현된다.

### Optimizing a Neural Radiace Field
+Positional Encoding + Hierarchical Sampling -> NeRF Optimize -> SOTA quality
#### Positional Encoding
NeRF의 input은 저차원이다. high resolution image도출을 위해 input을 higher dimensional space에 mapping한다. 
위에서 언급했던 $F=F′∘γ$로 표현해서 performance를 확장한다. γ는 R을 $R^2L$로 mapping 해주는 parameter다.
![](https://i.imgur.com/X3eLrtU.png)
γ(p)는 x에 속해있는 3 좌표에 대해 분해되어 사용된다. 
이 과정은 d와도 동일하게 나타나며 L의 값은 사용자가 설정하는 값이다. 
(논문에서는 γ(x)에 대해서는 L=10으로,γ(d)에 대해서는 L=4로 설정되었다고 한다
방향 자체가 가지고 있는 정보보다 

Positional encoding에 대한 mapping은 Transformer에서 사용한 positional encoding과 비슷하지만 목적에서 차이가 있다. 
Transformer의 경우는 model에게 token들의 순서에 대한 정보를 알려주지만 NeRF는 MLP가 고밀도 함수를 처리할수 있도록 input coordinate를 고차원의 공간으로 mapping하기 위해 사용된다. 
![](https://i.imgur.com/pjRzF9b.png)
view dependent 기법과 positional encoding 기법을 적용했을 경우와 적용하지 않았을 경우에 대한 비교 사진이다.
목적에서 차이가 있는데 세세한 표현을 살리기 위해 매핑을 해야지만 디테일한 정보를 살릴 수 있다. 0.1같은 작은 값들은 고차원으로 매핑해주면서 간격으로 벌려주는 점에서 차이가 있다.
### Hierarchical volume sampling
N개의 ray로 분할 후 값을 얻어내는 rendering 기법은 비효율적이다. free space와 occluded된 부분은 결과값에 기여를 안하기 때문이다. 
scene을 표현하기 위해 하나의 network를 사용하기보다는 coarse network와 fine network로 구분해 두 가지 network를 학습시킨다. 
network는 coarse network에 가까우며, 주어진 coarse network의 결과에 따라 fine network의 구성이 바뀌게 된다.
![](https://i.imgur.com/5hIUpj0.png)coarse network
![](https://i.imgur.com/NnYKjoO.png)
Loss function
 w 값들 Normalizing해주면 w 값들에 대한 하나의 PDF 생긴다. 
 PDF를 기반으로 w값이 높은 곳에 대해서 $C_f​(r)$, Fine하게 radiance의 분포를 얻게 된다. 
 $N_c​+N_f$​개의 색 분포를 얻은 최종 Loss function식이다.
![](https://i.imgur.com/zcDfCgY.png)

# NeRF on-the-go
[NeRF On-the-go: Exploiting Uncertainty for Distractor-free NeRFs in the Wild](https://arxiv.org/abs/2405.18715)
![](https://i.imgur.com/bsvgOXA.png)
목표: 정적 장면에 대한 NeRF를 학습하고 장면의 모든 동적 요소(주의 분산기)를 효과적으로 제거
불완전한 결과를 생성하는 NeRF-W및 RobustNeRF와 같은 기존 방법과 달리예측된 uncertainty 맵을 활용하여 이러한 주의 분산기를 효과적으로 제거한다. 
결과: 동적 장면에 대해 적응력이 강한 새로운 view 합성

Novel View Synthesis (NVS):이전에 관찰되지 않은 관점에서 장면을 렌더링하는 문제를 다룬다.
vanilla NeRF는 움직이는 물체, 그림자 또는 기타 동적 요소와 같은 주의를 산만하게 하는 요소 없이 capture porcess동안 장면이 완전히 정적인 상태로 유지되어야 한다는 가정 하에 작동하지만
NeRF On-the-go

 NeRF On-the-go: 효과적인 주의 분산자 제거를 위해 설계된 다용도 플러그 앤 플레이 모듈이다. 
캡처한 이미지에서 신속한 NeRF 학습을 가능하게 한다.

![](https://i.imgur.com/8MYiWOF.png)
1. 사전 학습된 DINOv2 네트워크는 포즈를 취한 이미지에서 피쳐 맵을 추출
2. ray를 선택하는 확장 패치 샘플러를 사용. 
3. 불확실성 MLP G는 이러한 ray의 DINOv2 피쳐를 입력으로 사용하여 불확실성 β(r)를 생성 
4. 세 가지 loss(오른쪽)가 G 및 NeRF 모델을 최적화하는 데 사용 
5. 학습 프로세스는 색상이 있는 점선으로 표시된 대로 경사 흐름을 분리함으로써 용이하다.

# Nerf: Representing scenes as neural radiance field for view synthesis
#### Volume Rendering
대응되는 픽셀의 고유 값을 ..?
흡수되지 않고 살아남았을 확률
