---
{"dg-publish":true,"permalink":"/3-d/ne-rf/"}
---

Representing Scenes as Neural Radiance Fields for View Synthesis
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

