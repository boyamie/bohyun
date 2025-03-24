---
{"dg-publish":true,"permalink":"/paper-review/layout-diffusion-controllable-diffusion-model-for-layout-to-image-generation/"}
---

#### abstract
![](https://i.imgur.com/FkZtlFi.png)
layout-guided vs. text-guided image generation
복잡한 장면에서 텍스트 프롬프트 만으로는 원하는 객체의 위치, 형태, 종류를 정확하게 제어하기 어려움
객체의 bbox와 카테고리 정보가 포함된 레이아웃을 사용하면 이미지 생성 시 더 강력하게 제어할 수 있다.
그림에서 caption(text 설명)만으로는 원하는 장면이 정확하게 생성되지 않는다. 
레이아웃을 사용하면 user가 지정한 object와 위치가 반영된 이미지를 생성할 수 있다.
![](https://i.imgur.com/rstRani.png)
unified space에서 이미지 레이아웃 fusion
structural image patch는 이미지의 각 패치를 위치 및 크기 정보를 포함하는 특별한 객체로 취급한다. 
이미지 패치의 위치 정보와 레이아웃 객체가 동일한 공간에서 정의된 bbox 안에 포함되도록 한다.
object layout은 이미지 내 객체들의 위치와 크기 정보를 담고있다.
fusion in unified form은 이미지 패치와 객체 레이아웃을 통합된 형태로 융합한다.

image와 layout의 복잡한 multimodal fusion을 극복하기 위해서 region information을 포함한 structural image patch를 구성하고 패치된 이미지를 일반 레이아웃과 통합된 형태로 fusion하기위한 특별한 레이아웃으로 변환한다.

게다가 layout fusion module(LFM)과 object-aware cross attention(OaCA)이 여러 객체 간의 관계를 모델링하기 위해 제안되었다. object-aware과 position-sensitive에 중점을 두도록 설계되어 spatial related information를 정밀하게 제어할 수 있도록 한다.
Extensive experiment결과, LayoutDiffusion은 COCO-stuff에서 SOTA를 능가함.
#### Introduction
GLIDEN, Imagen, Stable Diffusion같은 text-to-image분야에서 두드러진 성과를 보였다.
그러나 Fig. 1 (a)처럼 multiple objects가 포함된 복잡한 이미지를 생성하려고 할 때 properly, comprehensively 프롬프트를 생성하기 어렵다.
well-designed prompts를 입력해도 SOTA text-guided diffusion model이라도 missing object, incorrectly generating objects’ positions, shapes, categories 잘못 생성하는 문제들이 여전히 있다.
text의 ambiguity와 image space의 위치를 정확하게 표현하는 데 있어 weakness가 있기 때문이다.
guidance로 coarse(경량) layout을 사용할 땐 문제가 없다.
coarse layout as guidance은 bbox와 object category로 주석이 달린 object의 set이다.
spatial, high-level semantic information이 모두 포함된 diffusion model은 높은 품질을 유지하면서 더 강력한 controllability를 확보할 수 있다. 
layout-to-image 초기 연구는 GANs에 제한되어 있고 종종 unstable convergence(수렴)와 mode collapse문제를 겪는다. 