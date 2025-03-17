---
{"dg-publish":true,"permalink":"/paper-review/one-prompt-one-story/"}
---

ONE-PROMPT-ONE-STORY: FREE-LUNCH CONSISTENT TEXT-TO-IMAGE GENERATION USING A SINGLE PROMPT
 ICLR 2025

### abstract
text-to-image generation model은 input prompt로부터 고품질 이미지를 생성할 수 있다. 
그러나 이들은 storytelling에 대한 일관된 identity preserving 요구사항을 지원하는 데 어려움을 겪는다. 이러한 문제에 대한 기존 접근법은 일반적으로 대규모 데이터셋에서 광범위한 훈련이나 원래 모델 아키텍처에 대한 추가 수정이 필요하다. 
이는 다양한 domain과 다양한 diffusion model configuration 간의 적용 가능성을 제한한다. 
이 논문에서는 먼저 single prompt로 identity을 이해하는 능력인 맥락 일관성(<span style="background:#affad1">context consistency</span>)을 언어 모델이 가지고 있음을 관찰한다. 이러한 inherent context consistency에서 영감을 받아 "One-Prompt-One-Story"(1Prompt1Story)라는 일관된 text-to-image(T2I) 생성에 대한 새로운 training-free 방법을 제안한다. 1Prompt1Story는 T2I 확산 모델을 위한 single input으로 모든 prompt를 연결하여 처음에 캐릭터의 신원을 보존한다. 이후 우리는 두 가지 새로운 기술인 <span style="background:#affad1">Singular-Value Reweighting</span>와 <span style="background:#affad1">Identity-Preserving Cross-Attention</span>을 사용하여 생성 과정을 다듬고, 각 프레임에 대한 input 설명과의 더 나은 alignment를 보장한다. 다양한 기존의 일관된 T2I 생성 접근 방법과 비교하여 수치적 지표와 질적 평가를 통해 효과성을 입증한다.
### Introduction
Text-based image generation(T2I)은 다양한 장면에서 여러 대상을 묘사하는 textual prompt로부터 고품질 이미지를 생성하는 것을 목표로 한다. T2I diffusion model이 다양한 장면에서 주제의 일관성을 유지할 수 있는 능력은 애니메이션(Hu, 2024; Guo et al., 2024), 이야기 전개(Yang et al., 2024; Gong et al., 2023; Cheng et al., 2024), 비디오 생성 모델(Khachatryan et al., 2023; Blattmann et al., 2023) 및 기타 narrative-driven visual applications에 중요합니다. 그러나 일관된 T2I 생성을 달성하는 것은 기존 모델들에게 여전히 도전 과제입니다. 최근 연구는 다양한 접근 방식을 통해 주제의 일관성을 유지하는 문제를 다룹니다. 대부분의 방법은 신원 클러스터링을 위한 대규모 데이터셋에서의 시간 소모적인 훈련(Avrahami et al., 2023), 대형 매핑 인코더 학습(Gal et al., 2023b; Ruiz et al., 2024), 또는 미세 조정(Ryu, 2023; Kopiczko et al., 2024)을 수행하는 데 요구되어 언어 변 drift를 유발할 위험( Heng & Soh, 2024; Wu et al., 2024a; Huang et al., 2024) 등을 포함합니다. 최근의 몇몇 훈련 없는 접근법(Tewel et al., 2024; Zhou et al., 2024)은 사전 훈련된 모델의 공유 내부 활성화를 활용하여 일관된 주제를 가진 이미지를 생성하는 데 있어서 눈에 띄는 결과를 보여준다. 이러한 방법들은 T2I 확산 모델이 만족스러운 일관된 이미지를 생성하기 위해 메모리 자원이나 복잡한 모듈 디자인을 요구한다. 
![](https://i.imgur.com/5qHBgIe.png)
기존 방법들은 일관된 T2I 생성에서 도전 challenges에 직면해 있다. SDXL (Podell et al., 2023) 및 Juggernaut-X-v10 (RunDiffusion, 2024)와 같은 T2I 모델은 생성된 이미지에서 뚜렷한 정체성 불일치를 자주 나타낸다. 최근 IP-Adapter 및 ConsiStory와 같은 방법들이 정체성 일관성을 개선하였지만, 생성된 이미지와 해당 입력 프롬프트 간의 정렬이 무너졌습니다. 우리의 1Prompt1Story (아래)의 추가 결과는 텍스트와 이미지 간의 정렬을 저해하지 않으면서 우수한 일관성을 입증합니다.