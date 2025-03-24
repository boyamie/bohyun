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
기존 방법들은 일관된 T2I 생성에서 도전 challenges에 직면해 있다. SDXL (Podell et al., 2023) 및 Juggernaut-X-v10 (RunDiffusion, 2024)와 같은 T2I 모델은 생성된 이미지에서 뚜렷한 정체성 불일치를 자주 나타낸다. 최근 IP-Adapter 및 ConsiStory와 같은 방법들이 정체성 일관성을 개선했지만, 생성된 이미지와 해당 입력 프롬프트 간의 정렬이 무너졌다. 1Prompt1Story (아래)의 추가 결과는 텍스트와 이미지 간의 정렬을 저해하지 않으면서 우수한 일관성을 입증한다.

긴 프롬프트의 고유한 속성이 정체성 정보를 맥락 이해를 통해 암묵적으로 유지한다는 점을 이용한다. 이를 언어 모델의 맥락 일관성이라고 부른다. 예를 들어, "개가 영화를 보고 있다. 그 후, 개가 정원에 누워 있다."에서 개 객체는 같은 단락에 나타나며 문맥에 의해 연결되기 때문에 혼란 없이 같은 존재로 쉽게 이해될 수 있다. 우리는 이 고유한 특징을 활용하여 추가적인 미세 조정이나 복잡한 모듈 설계의 필요성을 없앤다.

언어 모델의 고유한 맥락 일관성을 관찰하고, 우리는 단일 프롬프트를 사용하여 일관된 캐릭터의 이미지를 생성하는 새로운 접근 방식을 제안한다. 이를 One-Prompt-One-Story(1Prompt1Story)라고 한다. 구체적으로, 1Prompt1Story는 원하는 모든 프롬프트를 단일 긴 문장으로 통합하며, 이는 해당 정체성 속성을 설명하는 정체성 프롬프트로 시작하고 각 프레임에서 원하는 시나리오를 설명하는 후속 프레임 프롬프트로 이어진다. 우리는 이 첫 번째 단계를 프롬프트 통합이라고 지칭합니다. 통합된 프롬프트 임베딩의 가중치를 재조정함으로써, Naive Prompt Reweighting이라는 기본 방법을 쉽게 구현하여 T2I 생성 성능을 조정할 수 있으며, 이 방법은 본질적으로 우수한 정체성 일관성을 달성한다. 

Naive Prompt Reweighting은 정체성 일관성을 유지하는 동시에 다양한 시나리오 프롬프트를 사용하여 각각 다른 프레임 설명을 포함하는 이미지 두 가지 예를 보여준다. 그러나 이 기본 접근 방식은 통합된 프롬프트 임베딩 내에서 각 프레임 프롬프트의 의미가 일반적으로 얽혀 있기 때문에 강력한 텍스트-이미지 정렬을 보장하지 않는다 (Radford et al., 2021). T2I 생성 모델의 텍스트-이미지 정렬과 정체성 일관성을 더욱 향상시키기 위해, 두 가지 추가 기술인 Singular-Value Reweighting (SVR) 및 Identity-Preserving Cross-Attention (IPCA)을 도입한다. Singular-Value Reweighting은 현재 프레임의 프롬프트 표현을 정제하고 다른 프레임의 정보를 감소시키는 것을 목표로 한다. 한편, Identity-Preserving Cross-Attention 전략은 교차 주의 레이어에서 주체의 일관성을 강화한다. 우리의 제안한 기술을 적용함으로써, 1Prompt1Story는 기존 접근 방식에 비해 더 일관된 T2I 생성 결과를 달성한다.

실험에서는 기존의 일관된 T2I 생성 벤치마크를 ConsiStory+로 확장하고, 여러 최첨단 방법과 비교한다. 여기에는 ConsiStory (Tewel et al., 2024), Story-Diffusion (Zhou et al., 2024), IP-Adapter (Ye et al., 2023) 등이 포함됩니다. 정성적 및 양적 성능 모두 우리 방법 1Prompt1Story의 효과성을 입증한다.

• 우리의 지식으로는, 언어 모델이 내부적인 ==context consistency==를 유지하는 과정을 분석한 것은 처음이다. 여기서 단일 프롬프트 내 여러 프레임 설명은 본질적으로 동일한 주체의 정체성을 지칭한다.  
• 문맥 일관성 특성을 기반으로, 우리는 일관된 T2I 생성을 위한 새로운 훈련 필요 없는 방법인 <span style="background:#affad1">One-Prompt-One-Story</span>를 제안한다. 더 구체적으로, 우리는 텍스트-이미지 정렬과 주체 일관성을 개선하기 위해 <span style="background:#affad1">Singular-Value Reweighting</span> 및 <span style="background:#affad1">Identity-Preserving Cross-Attention</span> 기법을 추가적으로 제안한다. 이를 통해 각 프레임 프롬프트를 단일 프롬프트 내에서 개별적으로 표현하면서도 정체성 프롬프트와 함께 일관된 정체성을 유지할 수 있다.  
• 기존의 일관된 T2I 생성 접근 방식과의 광범위한 비교를 통해, 우리는 연장된 <span style="background:#affad1">ConsiStory+</span> 벤치마크에서 긴 서사 전반에 걸쳐 정체성을 일관되게 유지하는 이미지를 생성하는 데 있어 <span style="background:#affad1">1Prompt1Story</span>의 효과성을 확인했다.   

### Related Work
**T2I personalized generation.** T2I personalization은 T2I model adaption이라고도 한다. 이는 몇 개의 이미지를 제공하고 새로운 개념을 고유한 토큰에 binding하여 주어진 모델을 새로운 개념에 맞게 조정하는 것을 목표로 한다. 결과적으로, adaption model은 새로운 개념의 다양한 변형(rendition)을 생성할 수 있다. 가장 대표적인 방법 중 하나는 DreamBooth(2023)로, 여기서 사전 훈련된 T2I 모델은 몇 개의 이미지를 제공받아 수정된 고유 identifier를 specific subject에 결합하는 방법을 학습하며, T2I 모델 매개변수도 업데이트한다. 최근의 접근 방식(2023; 2023b; 2023)은 이 파이프라인을 따라가며 생성 품질을 향상시킨다. 또 다른 대표적인 방법인 Textual Inversion(2023a)은 T2I 생성 모델을 미세 조정하는 대신 새로운 개념 토큰을 학습하는 데 중점을 둔다. Textual Inversion은 text embedding space에서 personalization를 수행하여 새로운 pseudo-words를 찾는다. 최근 연구(2022; 2023; 2023a; 2024)는 유사한 기술을 따른다.

**Consistent T2I generation.** 최근의 발전에도 불구하고, T2I personalization 방법은 종종 modifier 토큰을 효과적으로 학습하기 위해 광범위한 훈련을 요구한다. 이 훈련 과정은 시간 소모가 클 수 있어 실용적인 영향을 제한합니다. 최근에는 일관된 T2I 생성 접근 방식을 개발하는 방향으로 변화가 있었습니다(2024b; a), 이는 T2I 개인화의 특수한 형태로 간주될 수 있습니다. 이러한 방법들은 대부분 입력 이미지와 의미적으로 유사한 특성을 가진 인간 얼굴을 생성하는 데 중점을 둡니다. 특히, 이들은 추가적인 미세 조정 없이 정체성을 유지하는 T2I 생성을 달성하는 것을 목표로 합니다. 이들은 주로 PEFT 기술(2023; 2024)을 활용하거나 대규모 데이터셋으로 사전 훈련(2024; 2023)을 통해 의미 공간에서 사용자 맞춤형 이미지 인코더를 학습합니다. 예를 들어, PhotoMaker(2023b)는 이미지 인코더의 일부 변환기 계층을 미세 조정하고 클래스와 이미지 임베딩을 병합하여 정체성 임베딩을 추출하는 능력을 향상시킵니다. The Chosen One(2023)은 동일한 프롬프트로 생성된 이미지 세트에서 반복적으로 유사한 외관을 가진 이미지를 식별하기 위해 정체성 클러스터링 방법을 활용합니다.  
하지만 대부분의 일관된 T2I 생성 방법(2024; 2024a)은 여전히 T2I 모델의 매개변수를 훈련해야 하며, 이로 인해 기존의 사전 훈련된 커뮤니티 모델과의 호환성을 희생하거나 높은 얼굴 충실도를 보장하지 못합니다. 추가로, 이러한 시스템의 대부분(2023b; 2023b; 2024)은 인간 얼굴에 특화되어 설계되었기 때문에 비인간 주체에 적용할 때 제한에 직면합니다. 최신 접근 방식인 StoryDiffusion(2024), The Chosen One(2023) 및 ConsiStory(2024)조차도 정체성 일관성을 달성하기 위해 시간 소모적인 반복 클러스터링이나 높은 메모리 요구 사항이 필요합니다.  
스토리텔링. 스토리 생성(2019; 2021), 즉 스토리텔링은 캐릭터 일관성과 관련이 깊은 활성화 연구 방향 중 하나입니다. 최근 연구(2024; 2023)는 저명한 사전 훈련된 T2I 확산 모델(2022; 2022)과 통합하였으며, 이 접근 방식의 대부분은 스토리 생성 데이터셋에 대해 강렬한 훈련을 요구합니다. 예를 들어, Make-a-Story(2023)는 스토리텔링 과정 전반에 걸쳐 맥락 정보를 캡처하고 활용하기 위해 설계된 시각적 메모리 모듈을 도입합니다. StoryDALL-E(2022)는 스토리 생성 패러다임을 스토리 연속성으로 확장하며, DALL-E의 기능을 활용하여 이전의 GAN 기반 방법론에 비해 상당한 향상을 이룹니다. 