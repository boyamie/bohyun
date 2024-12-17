---
{"dg-publish":true,"permalink":"/paper-review/scaling-in-the-wild-training-for-diffusionbased-illumination-harmonization-and-editingby-imposing-consistent-light-transport/"}
---

##### ABSTRACT
###### novelty
<font color="#a5a5a5">The current bottleneck in scaling up the training of diffusion-based illumination editing models is mainly in the difficulty of preserving the underlying image details and maintaining intrinsic properties, such as albedos,</font>
<font color="#a5a5a5">unchanged.</font>
논문의 저자가 생각한 diffusion 모델의 한계는 diffusion-based illumination editing model을 훈련할 때의 병목현상이다. 
albedo(색상)같은 intrinsic property를 유지하는 데 어려움을 겪고 있다는 점을 지적한다.
###### methods
<font color="#a5a5a5">propose Imposing Consistent Light (ICLight) transport during training</font>
<font color="#a5a5a5">-> rooted in the physical principle that the linear blending of an object’s appearances under different illumination conditions is consistent with its appearance under mixed illumination.</font>
그래서 제안한 방법은 IC_Light (Imposing Consistent Light)으로 training동안 transport를 한다.
그 principle은 서로 다른 조명 조건 하에서 객체의 모습들이 선형적으로 혼합될 때, 혼합된 조명 하에서의 모습과 일치한다는 것이다. 여러 조명을 경험한 객체는 자연스럽게 혼합된 조명을 받았을 때도 일관된 형태로 나타난다.

###### results
<font color="#a5a5a5">Based on this method, we can scale up the training of diffusion-based illumination</font>
<font color="#a5a5a5">editing models to large data quantities (>10 million), across all available data</font>
<font color="#a5a5a5">types (real light stages, rendered samples, in-the-wild synthetic augmentations,</font>
<font color="#a5a5a5">etc.), and using strong backbones (SDXL, Flux, etc.). </font>
모델은 1천만 개 이상의 다양한 샘플을 통해 훈련될 수 있으며, 현실 사진, 렌더링된 이미지 및 합성 조명 보강을 포함한 다양한 타입의 데이터를 사용할 수 있다. 조명 편집의 정확도를 높이고 artifact(예: 잘못 맞춰진 재료, 알베도 변형 등)를 줄이는 데 기여한다.



