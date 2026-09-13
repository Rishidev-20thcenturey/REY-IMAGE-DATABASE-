# RAY-IMAGE

**An efficient text-to-image generation system, built from scratch.**

---

## Overview

RAY-IMAGE is an ongoing research project building a capable image-generation system from the ground up. The goal is not to compete with massive proprietary models through raw scale, but to explore how **careful architecture, efficient latent representations, and disciplined training** can produce strong results on modest hardware.

The project is developed on free-tier cloud GPUs with a focus on reproducible experiments, measurable diagnostics, and incremental improvements.

---

## What It Does

RAY-IMAGE maps a natural-language prompt to an image through a text-conditioned latent generation pipeline.

| Input | Target Output |
| :--- | :--- |
| Natural-language prompt | Image up to 256×256 at the current N8 stage |

The longer-term roadmap targets higher resolutions and more capable conditioning.

---

## Architecture

RAY-IMAGE uses a **latent-space flow-matching architecture** with three main components:

| Component | Current design | Role |
| :--- | :--- | :--- |
| **Text Encoder** | Qwen3-4B, frozen, 8-bit | Converts a prompt into token-level semantic embeddings |
| **DiT v2** | ~180M-class Transformer, 18 blocks, 768 hidden size, 12 heads | Predicts the transformation of noisy image latents under text + timestep conditioning |
| **VAE v2** | 16-channel latent autoencoder | Converts 256×256 RGB images ↔ 16×32×32 latent tensors |

### Data Flow

```text
Prompt
  │
  ▼
Qwen3-4B Text Encoder
  │  token embeddings [B, L, 2560]
  ▼
Gated Cross-Attention
  │
Random latent noise [B, 16, 32, 32]
  │
  ▼
DiT v2
  │  256 spatial tokens × 768 dimensions
  │  timestep conditioning
  ▼
Predicted latent transformation
  │
  ▼
VAE v2 Decoder
  │
  ▼
256×256 RGB image
```

### DiT v2 at a Glance

The current real-world DiT architecture is designed around the VAE's 16×32×32 latent space:

- **Input:** `[B, 16, 32, 32]`
- **Patch size:** `2×2`
- **Spatial token grid:** `16×16`
- **Token count:** `256`
- **Transformer width:** `768`
- **Transformer depth:** `18` blocks
- **Attention heads:** `12`
- **Text context:** `2560`-dimensional Qwen embeddings
- **Conditioning:** sinusoidal timestep embedding + pooled text conditioning + token-level cross-attention
- **Cross-attention:** gated residual path for controllable text influence
- **Output:** reconstructed latent field `[B, 16, 32, 32]`

### VAE v2 at a Glance

The VAE was upgraded from the earlier 4-channel toy representation to a larger real-world latent space:

```text
256×256×3 RGB
      │
      ▼
  VAE Encoder
      │
      ▼
  16×32×32 latent
      │
      ▼
  VAE Decoder
      │
      ▼
256×256×3 RGB
```

This gives the DiT substantially more latent capacity than the original N0/N2 4×8×8 representation while keeping the transformer operating on a compact spatial grid.

---

## The N-Series Methodology

RAY-IMAGE is developed through a numbered experiment series (N0 → N12). Each phase introduces a controlled engineering or research change and is validated before moving forward.

| Phase | Focus |
| :--- | :--- |
| **N0–N4** | Foundation — VAE, baseline DiT, flow matching, diagnostics |
| **N5** | Gated cross-attention for stronger text conditioning |
| **N5.1–N5.2** | Evaluation calibration and bottleneck diagnosis |
| **N6** | Higher-capacity VAE with perceptual and adversarial losses |
| **N7** | DiT retraining on improved latent representations |
| **N8** | Real-world data, 256×256 VAE, Qwen conditioning, DiT v2 |
| **N9–N12** | Progressive resolution, larger models, control, deployment |

Each phase aims to produce a concrete artifact rather than only a research note.

---

## Current Status

| Component | Status |
| :--- | :--- |
| Toy-scale pipeline | ✅ Complete |
| 16-channel VAE | ✅ Complete |
| Real-world VAE v2 at 256×256 | ✅ Trained |
| Qwen3-4B conditioning | ✅ Integrated |
| DiT v2 architecture | ✅ Built |
| Real-world DiT training | 🔄 Next |
| Public model release | 🔜 Planned |

**Current training data:** MONET and other compatible open image datasets.

**Development hardware:** Kaggle free-tier GPUs, including T4-class acceleration.

---

## What Makes This Different

### 1. No pretrained image generator

The image-generation core is built from first principles. The project does not start from an existing pretrained image generator and fine-tune it.

### 2. Research-driven iteration

When a model fails, the approach is to isolate the failure mode with a diagnostic experiment before spending more compute.

### 3. Latent efficiency

The expensive transformer operates on compact latent tensors rather than full-resolution RGB pixels. At N8, a 256×256 image is represented by only **16×32×32 latent values** before patch tokenization.

### 4. Frozen language conditioning

The text encoder is separated from image-model training, allowing the DiT to concentrate its capacity on learning the image-generation problem while reusing a strong semantic representation.

### 5. Resumable experimentation

Training is designed around checkpointed, chunked sessions so experiments can survive free-tier runtime limits without throwing away progress.

---

## Roadmap

| Phase | Goal | Status |
| :--- | :--- | :--- |
| **N8** | Real-world 256×256 generation | 🔄 Current |
| **N9** | Progressive resolution scaling (512×512) | Planned |
| **N10** | Public 500M-parameter model release | Planned |
| **N11** | Optional sparse-MoE scaling | Planned |
| **N12** | Layout control, reasoning features, quantized deployment | Planned |

Future evaluation will focus on areas such as prompt alignment, text rendering, spatial composition, and controllable layout.

Detailed training recipes and internal architecture notes remain private during active research.

---

## Why Build From Scratch?

Modern AI tooling makes it easy to call or fine-tune existing models. RAY-IMAGE is an experiment in learning what happens when the underlying image-generation system is engineered directly.

The project is intended to:

- understand diffusion and flow-matching systems at the implementation level;
- study efficient training under strict compute constraints;
- develop architecture through measured experiments rather than scale alone; and
- eventually release a useful, reproducible image-generation model.

---

## Tech Stack

| Layer | Tool |
| :--- | :--- |
| **Framework** | PyTorch |
| **Text conditioning** | Qwen3-4B |
| **Compute** | Kaggle GPU free tier |
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