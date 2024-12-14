---
{"dg-publish":true,"permalink":"/paper-review/scaling-in-the-wild-training-for-diffusionbased-illumination-harmonization-and-editingby-imposing-consistent-light-transport/"}
---

##### ABSTRACT
###### novelty
The current bottleneck in scaling up the training of diffusion-based illumination editing models is mainly in the difficulty of preserving the underlying image details and maintaining intrinsic properties, such as albedos,
unchanged.
논문의 저자가 생각한 diffusion 모델의 한계는 diffusion-based illumination editing model을 훈련할 때의 병목현상이다. 
albedo(색상)같은 intrinsic property를 유지하는 데 어려움을 겪고 있다는 점을 지적한다.
propose Imposing Consistent Light (ICLight) transport during training
-> rooted in the physical principle that the linear blending of an object’s appearances under different illumination conditions is consistent with its appearance under mixed illumination.

