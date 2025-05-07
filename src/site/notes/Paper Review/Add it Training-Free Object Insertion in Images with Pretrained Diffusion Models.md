---
{"dg-publish":true,"permalink":"/paper-review/add-it-training-free-object-insertion-in-images-with-pretrained-diffusion-models/"}
---


## Introduction

Object insertion into images based on textual instructions is a fundamental task in image editing that remains challenging despite recent advances in generative AI. The ability to naturally place objects within existing scenes requires understanding both the object properties and the scene context, including physical constraints, spatial relationships, and semantic plausibility - a concept known as "affordance."

The ADD-IT (Attention-Driven Diffusion for Image Translation) method addresses this challenge through a training-free approach that leverages pretrained diffusion models. Unlike existing methods that either require extensive task-specific fine-tuning or struggle with proper object placement, ADD-IT introduces techniques to balance the preservation of the original scene while seamlessly integrating new objects in semantically plausible locations.

Developed by researchers from NVIDIA, Tel-Aviv University, and Bar-Ilan University, ADD-IT introduces three key components: weighted extended self-attention, structure transfer, and subject-guided latent blending. These components work together to maintain the structural coherence of the source image while allowing for detailed and contextually appropriate object insertion.

textual instruction을 기반으로 image editing으로 object insertion task는 generative AI의 발전에도 불구하고 여전히 challenging하다. 기존 장면에 객체를 자연스럽게 배치하려면, 객체 자체의 특성뿐 아니라 장면의 맥락—즉 물리적 제약, 공간적 관계, 의미론적 타당성(이를 **“어포던스”**라고 부름)을 이해해야 합니다.

**ADD-IT (Attention-Driven Diffusion for Image Translation)**는 이러한 문제를 해결하기 위해 고안된 **훈련이 필요 없는 방법**입니다. 이 방식은 사전 학습된 디퓨전 모델을 활용하며, 기존의 방식들과 달리 **과도한 태스크별 파인튜닝 없이도** 객체를 장면에 자연스럽게 삽입할 수 있습니다.

ADD-IT는 **원본 장면의 구조를 유지하면서도 의미적으로 적절한 위치에 새로운 객체를 통합하는 기술**을 도입합니다. 이 기술은 NVIDIA, 텔아비브 대학교, 바르일란 대학교의 연구진에 의해 개발되었습니다.

ADD-IT는 다음의 세 가지 핵심 구성 요소로 이루어져 있습니다:

1. **가중 확장 셀프 어텐션 (weighted extended self-attention)**
    
2. **구조 전이 (structure transfer)**
    
3. **주제 유도 잠재 혼합 (subject-guided latent blending)**
    

이 세 가지 기술은 서로 결합되어, **이미지의 구조적 일관성을 유지하면서도 맥락에 적절한 정밀한 객체 삽입**을 가능하게 합니다.

---

다른 단락도 번역해드릴까요?
## Background and Related Work

Current approaches to object insertion in images typically fall into two categories:

1. **Methods using pretrained text-to-image diffusion models**: These leverage the general knowledge of diffusion models but often struggle with aligning new objects properly within existing scenes. Examples include Prompt-to-Prompt and SDEdit, which can produce objects but frequently place them in implausible locations or fail to maintain the original scene's integrity.
    
2. **Task-specific fine-tuned models**: Approaches like InstructPix2Pix and MagicBrush train on large datasets of image editing examples. While effective within their training domain, these methods often lack generalization capability for open-world object insertion and can produce artifacts or incorrect object placements.
    

Both approach types struggle with the concept of "affordance" - understanding where and how objects should naturally fit within a scene. This limitation stems from difficulties in balancing adherence to the source image's structure while allowing sufficient freedom to incorporate new elements.

## The ADD-IT Approach

ADD-IT introduces a training-free pipeline that leverages pretrained diffusion models, particularly Stable Diffusion, to perform object insertion without additional training. The method consists of three main components that work together to achieve high-quality object insertion while maintaining scene coherence:

1. Weighted Extended Self-Attention (WESA)
2. Structure Transfer
3. Subject-Guided Latent Blending

For real images, ADD-IT includes an inversion step to reconstruct the image in the latent space of the diffusion model. The method is applicable to both real and AI-generated images and supports iterative, step-by-step image modifications.

## Weighted Extended Self Attention

The core innovation in ADD-IT is the Weighted Extended Self-Attention (WESA) mechanism, which modifies how attention operates within the diffusion model. In standard diffusion models, cross-attention connects text prompts to image features, while self-attention allows different regions of an image to influence each other.

WESA extends this by integrating information from both the source image and the text prompt through weighted attention. The mechanism is formulated as:

```
Attention(Q, K, V) = softmax(QK^T / √d) · V
```

Where Q, K, and V are query, key, and value matrices.

In WESA, the attention is extended to include both the source image features and target image features:

```
WESA(Q, K_s, K_t, V_s, V_t) = softmax(w_s · QK_s^T / √d + w_t · QK_t^T / √d) · [w_s · V_s + w_t · V_t]
```

Where:

- K_s, V_s are key and value matrices from the source image
- K_t, V_t are key and value matrices from the target image
- w_s, w_t are weighting factors that balance influence

This weighted approach allows the model to preserve the structure of the source image while incorporating new content based on the text prompt. The weights can be adjusted throughout the diffusion process to control how much influence the source image has at different denoising steps.

## Structure Transfer

The Structure Transfer component addresses the challenge of maintaining the global structure of the source image while allowing local changes for object insertion. The approach involves:

1. Noising the source latent to a high noise level (typically t=0.8)
2. Starting the denoising process from this noisy latent
3. Gradually reducing the noise while using WESA to guide the denoising process

This technique preserves the overall composition and layout of the source image while providing enough flexibility for local modifications. By starting from a highly noised state of the original image rather than pure noise, the model retains structural information that helps maintain scene coherence.

The process can be described by:

```
z_t = α_t · x + σ_t · ε
```

Where:

- z_t is the noised latent at timestep t
- x is the source image latent
- ε is random noise
- α_t and σ_t are noise schedule parameters

## Subject Guided Latent Blending

The Subject-Guided Latent Blending component aims to preserve fine background details from the source image that should not be affected by the added object. This component:

1. Generates a rough mask of the area where the new object will be placed
2. Refines this mask using an external segmentation model (SAM-2)
3. Blends the source and target noisy latents based on this mask

The blending operation can be formulated as:

```
z_blend = M ⊙ z_target + (1-M) ⊙ z_source
```

Where:

- M is the refined mask
- ⊙ represents element-wise multiplication
- z_target and z_source are the target and source latents

This approach ensures that only the regions directly affected by the new object are modified, while the rest of the image remains unchanged from the source. This selective blending helps maintain the integrity of the original scene while allowing for precise object insertion.

## Evaluation and Results

ADD-IT was evaluated through both objective metrics and human evaluations across multiple benchmarks:

1. **Additing Benchmark**: A new benchmark introduced by the authors for evaluating object insertion in AI-generated images.
2. **Emu-Edit**: An existing benchmark for image editing tasks.
3. **Additing Affordance Benchmark**: A specialized benchmark focusing on the plausibility of object placement.

The evaluation relied on several metrics:

- CLIP similarity scores to measure alignment with text prompts
- Object inclusion metrics to verify the presence of requested objects
- Human evaluations to assess overall quality and plausibility

ADD-IT consistently outperformed both training-free approaches (like Prompt-to-Prompt) and supervised methods (like InstructPix2Pix and MagicBrush) across these benchmarks. Human evaluators preferred ADD-IT's results in over 80% of cases compared to competing methods.

The most significant improvements were observed in the affordance aspect - ADD-IT demonstrated a superior ability to place objects in semantically plausible locations within scenes, respecting physical constraints and spatial relationships.

## Applications and Limitations

ADD-IT enables several practical applications:

1. **Single object insertion**: Adding a specific object to an existing scene based on a text prompt.
2. **Iterative scene building**: Step-by-step addition of multiple objects to gradually construct complex scenes.
3. **Real image editing**: Modifying real photographs by adding new objects while maintaining the original image's characteristics.

Despite its strengths, ADD-IT has several limitations:

1. It sometimes struggles with complex spatial relationships or very specific object placement requests.
2. The method is computationally more intensive than single-pass approaches due to the multiple diffusion steps.
3. Like all diffusion-based methods, it can occasionally generate artifacts or fail to perfectly preserve fine details from the source image.
4. The method inherits biases present in the underlying pretrained diffusion model.

## Conclusion

ADD-IT represents a significant advancement in training-free object insertion by introducing mechanisms that balance preservation of the original scene with the integration of new objects. Through weighted extended self-attention, structure transfer, and subject-guided latent blending, the method achieves state-of-the-art performance without requiring task-specific fine-tuning.

The approach demonstrates that careful manipulation of attention mechanisms in diffusion models can achieve superior results compared to supervised methods trained specifically for object insertion tasks. ADD-IT's ability to understand and respect scene affordance - placing objects in semantically plausible locations - represents a particularly important advance in image editing technology.

By providing a training-free method that works with existing pretrained diffusion models, ADD-IT makes high-quality object insertion more accessible while advancing our understanding of how to effectively control generative models for precise image editing tasks. This approach opens up new possibilities for intuitive and versatile image manipulation tools that better understand the semantic relationships between objects and scenes.

## Relevant Citations

Tim Brooks, Aleksander Holynski, and Alexei A. Efros. [Instructpix2pix: Learning to follow image editing instructions.](https://alphaxiv.org/abs/2211.09800) InCVPR, 2023.

- InstructPix2Pix is a supervised method used as a baseline. It's relevant because ADD-IT aims to outperform such supervised methods for object insertion in images.

Alper Canberk, Maksym Bondarenko, Ege Ozguroglu, Ruoshi Liu, and Carl Vondrick. Erasedraw: Learning to draw step-by-step via erasing objects from images. 2024.

- Erasedraw is another supervised baseline for image editing and object insertion tasks. It's directly compared to ADD-IT in the experiments.

Shelly Sheynin, Adam Polyak, Uriel Singer, Yuval Kirstain, Amit Zohar, Oron Ashual, Devi Parikh, and Yaniv Taigman. [Emu edit: Precise image editing via recognition and generation tasks.](https://alphaxiv.org/abs/2311.10089) 2023.

- The Emu Edit benchmark is used in the evaluation of ADD-IT, providing a dataset of real images and instructions. ADD-IT's performance is compared against other methods on this benchmark.

Kai Zhang, Lingbo Mo, Wenhu Chen, Huan Sun, and Yu Su. MagicBrush: A manually annotated dataset for instruction-guided image editing. InNeurIPS, 2023.

- MagicBrush, a fine-tuned version of InstructPix2Pix, serves as another supervised baseline method for comparison. It utilizes a manually annotated dataset for instruction-guided image editing.

Amir Hertz, Ron Mokady, Jay Tenenbaum, Kfir Aberman, Yael Pritch, and Daniel Cohen-Or. [Prompt-to-prompt image editing with cross attention control.](https://alphaxiv.org/abs/2208.01626)arXiv preprint arXiv:2208.01626, 2022.

- Prompt-to-Prompt is a key baseline as a training-free method that modifies attention maps for image editing. It allows comparing ADD-IT to another zero-shot approach.