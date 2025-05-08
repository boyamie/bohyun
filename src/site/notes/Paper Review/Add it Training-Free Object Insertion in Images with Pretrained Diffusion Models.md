---
{"dg-publish":true,"permalink":"/paper-review/add-it-training-free-object-insertion-in-images-with-pretrained-diffusion-models/"}
---

NVIDIA, 텔아비브 대학교, 바르일란 대학교의 연구진에 의해 쓰여진 논문이다. ICLR 2025에 accept되었다!
# Introduction
textual instruction을 기반으로 image editing으로 object insertion task는 generative AI의 발전에도 불구하고 여전히 challenging하다. 기존 장면에 객체를 자연스럽게 배치하려면, object properties뿐 아니라 scene context도 이해해야 한다. 즉 물리적 제약, 공간적 관계, 의미론적 타당성("affordance"라는 개념)을 이해해야 한다.

ADD-IT (Attention-Driven Diffusion for Image Translation)는 이러한 문제를 해결하기 위해 고안된 training-free이다. 이 방식은 pretrained 디퓨전 모델을 활용한다. 기존의 방식들과 달리 task-specific fine-tuning 없이도 객체를 장면에 자연스럽게 삽입할 수 있다.
ADD-IT는 original scene을 보존하면서 semantically plausible(적절한)위치에 새로운 object insertion하는 기술을 도입했다. 

1. weighted extended self-attention
2. structure transfer
3. subject-guided latent blending
# Background and Related Work

현재 이미지 내 object insertion은 보통 두 가지 categories로 나뉜다.

1. pretrained text-to-image diffusion models
    diffusion models이 가진 일반적인 지식을 활용한다. existing scenes 내에 새로운 객체를 정확하게 align하는 데 어려움이 있다.  
    예시로는 **Prompt-to-Prompt**와 **SDEdit**이 있다. 이들은 객체 생성은 가능하지만 종종 implausible(비현실적) 위치에 객체를 배치하거나 original scene's integrity를 유지하는데 실패한다.

2. Task-specific fine-tuned models
    **InstructPix2Pix**나 **MagicBrush**와 같은 기법은 large image editing 데이터셋을 학습한다. 
    이 방식은 그들의 training domain에서는 효과적이지만, open-world object insertion에는 일반화가 어렵고 incorrect object placements나 시각적 artifacts가 발생할 수 있다.

두 approach 모두 "affordance" 문제에 한계를 보인다.
affordance: 객체가 scene 내에서 where 그리고 how 자연스럽게 놓여야 하는가를 이해하는 능력
이는 source image의 structure에 대한 adherence(충실함)과 new element의 incorporate(삽입) 자유도를 동시에 만족시키는 데 어려움이 있기 때문이다.

# The ADD-IT Approach

ADD-IT은 Stable Diffusion과 같은 pretrained diffusion models을 활용하여 별도의 additional training없이 object insertion을 수행하는 training-free pipeline을 제안한다.
scene coherence(일관성)을 유지하면서 high-quality object insertion을 위해서 다음 세가지 주요한 방법이 있다.

1. Weighted Extended Self-Attention (WESA)
2. Structure Transfer
3. Subject-Guided Latent Blending

실제(real) 이미지의 경우 디퓨전 모델의 latent space상에서 이미지를 복원하는 inversion step을 거친다.
ADD-IT는 real image와 AI-generated image 모두에 적용 가능하며 사용자 피드백을 반영해 step-by-step으로 image modification할 수 있는 기능도 지원한다.

# Weighted Extended Self Attention

ADD-IT의 core innovation는 Weighted Extended Self-Attention (WESA) mechanism이다. diffusion model 내에서 attention이 작동하는 방식을 수정해서 text prompt와 원본 이미지의 정보를 동시에 활용하도록 확장한다. 기존 diffusion model에서의 attention은 cross-attention으로 text prompts to image features, while self-attention allows different regions of an image to influence each other.

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