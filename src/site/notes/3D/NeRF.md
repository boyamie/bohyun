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
