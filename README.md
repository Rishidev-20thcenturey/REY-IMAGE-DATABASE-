<div align="center">

<img src="preview.gif" alt="REY-IMAGE preview animation" width="100%">

**Efficient text-to-image research — built from scratch.**

</div>

---

# RAY-IMAGE

**An efficient text-to-image generation system, built from scratch.**

---

## Overview

RAY-IMAGE is an ongoing research project exploring how a capable image-generation system can be built with limited compute through careful engineering, compact representations, and measured experimentation.

The project is developed on free-tier cloud GPUs and focuses on incremental, reproducible progress rather than simply increasing model size.

---

## What It Does

RAY-IMAGE takes a natural-language prompt and uses it to generate an image.

| Input | Target Output |
| :--- | :--- |
| Natural-language prompt | Up to 256×256 at the current stage |

Higher-resolution generation is part of the future roadmap.

---

## Architecture

The public architecture can be summarized as three cooperating parts:

| Component | Purpose |
| :--- | :--- |
| **Text Encoder** | Converts the prompt into a semantic representation |
| **Diffusion Transformer** | Generates and refines an image representation under text and timestep conditioning |
| **VAE** | Converts between images and a compact latent representation |

### Simplified Pipeline

```text
Text Prompt
    │
    ▼
Text Encoder
    │
    ▼
Text Features ───────────┐
                         │
Random Latent Noise      │
    │                    │
    └──────► Transformer ◄┘
                 │
                 ▼
          Image Latent
                 │
                 ▼
               VAE
                 │
                 ▼
             Output Image
```

The model operates primarily in **latent space**, allowing the expensive generation stage to work on a smaller representation than the final RGB image.

A lightweight form of gated text conditioning is used so that text information can influence the generation process without requiring the transformer to operate directly on raw pixels.

> Detailed layer dimensions, training schedules, and internal research techniques are intentionally kept private while development is active.

---

## The N-Series

RAY-IMAGE is developed through a sequence of numbered experiments.

| Phase | Focus |
| :--- | :--- |
| **N0–N4** | Foundation and diagnostics |
| **N5** | Improved text conditioning |
| **N6** | Higher-capacity latent representation |
| **N7** | Improved latent-space generation |
| **N8** | Real-world data and larger-scale architecture |
| **N9–N12** | Resolution, scaling, control, and deployment |

Each phase is intended to produce a measurable engineering result before the next major change is introduced.

---

## Current Status

| Component | Status |
| :--- | :--- |
| Toy-scale validation | ✅ Complete |
| Real-world VAE | ✅ Trained |
| Text conditioning | ✅ Integrated |
| Larger DiT architecture | ✅ Built |
| Real-world DiT training | 🔄 In progress |
| Public model release | 🔜 Planned |

**Current development data:** Open image datasets, including MONET.

**Compute:** Free-tier cloud GPUs.

---

## What Makes This Different

### Built Around Efficiency

The generation model works in a compact latent representation instead of directly modeling full-resolution RGB images.

### Research-Driven Development

Failures are investigated with small diagnostic experiments before committing significant compute to a new training run.

### Built From Scratch

The image-generation core is developed rather than starting from a pretrained image generator.

### Designed for Constrained Hardware

The project is structured around free-tier and consumer-class GPU environments, making checkpointing and efficient experiments important from the beginning.

---

## Roadmap

| Phase | Goal | Status |
| :--- | :--- | :--- |
| **N8** | Real-world image generation at the current resolution | 🔄 Current |
| **N9** | Progressive resolution scaling | Planned |
| **N10** | Larger public model release | Planned |
| **N11** | Optional sparse scaling | Planned |
| **N12** | Better control and efficient deployment | Planned |

Future work will focus on prompt alignment, composition, image quality, and controllable generation.

---

## Why Build From Scratch?

RAY-IMAGE is also an engineering and research exercise: understanding how the pieces of an image-generation system work together, how failures arise, and how useful results can be achieved under strict compute constraints.

The long-term goal is to turn those experiments into a practical, reproducible image-generation system.

---

## Tech Stack

| Layer | Tool |
| :--- | :--- |
| **Framework** | PyTorch |
| **Compute** | Kaggle / free-tier GPU infrastructure |
| **Code hosting** | GitHub |
| **Model hosting** | Hugging Face |
| **Data pipeline** | Hugging Face Datasets / open datasets |

---

## License

Apache 2.0 — free for research and commercial use.

---

## Contact

For collaboration, research inquiries, or questions:
**rishirajc406@gmail.com**

---

*Built from scratch, one experiment at a time.*