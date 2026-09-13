# RAY-IMAGE

**An efficient text-to-image generation system, built from scratch.**

---

## Overview

RAY-IMAGE is an ongoing research project building a capable image generation system from the ground up. The goal is not to compete with massive proprietary models on raw scale, but to demonstrate that **thoughtful architecture and efficient training can produce strong results on modest hardware.**

Everything is developed and trained on **free-tier cloud GPUs** with no external funding. Every architectural decision is documented. Every failure is diagnosed. Every checkpoint is versioned.

---

## What It Does

RAY-IMAGE generates images from text prompts.

| Input | Output |
| :--- | :--- |
| Natural language prompt (e.g., "a neon-lit street at night") | Generated image at up to 256×256 resolution (scaling in progress) |

The system supports:
- Text-to-image generation
- Prompt-faithful composition
- Efficient inference on consumer hardware
- Progressive resolution scaling (256 → 512 → 1024 in progress)

---

## Architecture Overview

RAY-IMAGE is a **latent diffusion system** composed of three core components:

| Component | Role |
| :--- | :--- |
| **Text Encoder** | Converts prompts into semantically rich embeddings (frozen, pretrained) |
| **Diffusion Transformer (DiT)** | Transforms random noise into an image latent, guided by text |
| **Variational Autoencoder (VAE)** | Compresses real images into compact latents and expands latents back into images |

The full pipeline:

```

Text Prompt → Text Encoder → Text Embedding
↓
Random Noise → DiT → Denoised Latent
↓
VAE Decoder
↓
Final Image

```

### Key Design Principles

- **Latent-space diffusion** — training in compressed space rather than pixel space for efficiency
- **Frozen pretrained text encoder** — leveraging existing language understanding
- **Gated conditioning** — architectural modifications for stable text-to-image alignment
- **Efficient VAE** — engineered for a balance of compression and reconstruction quality

---

## The N-Series Methodology

RAY-IMAGE is developed through a numbered experiment series (N0 → N12). Each phase adds exactly one controlled change and is validated before progressing.

| Phase Range | Focus |
| :--- | :--- |
| **N0–N4** | Foundation — VAE, baseline DiT, diagnostics |
| **N5** | Architecture — gated cross-attention |
| **N5.1–N5.2** | Debugging — evaluator audit, bottleneck diagnosis |
| **N6** | Upgrade — improved VAE architecture |
| **N7** | Retrain — DiT on improved latents |
| **N8** | Scaling — real-world data, larger resolution |
| **N9–N12** | Progressive resolution, scaling, deployment |

Each phase produces a **working artifact**, not just a research note.

---

## Current Status

| Component | Status |
| :--- | :--- |
| Core pipeline (VAE + DiT + text encoder) | ✅ Complete |
| Toy-scale validation (64×64, 12 classes) | ✅ Complete |
| Real-world VAE (256×256, MONET) | ✅ Trained |
| Real-world DiT | 🔄 In progress |
| Public release | 🔜 Planned |

**Training data:** MONET (104.9M image-text pairs, Apache 2.0)

**Hardware:** Kaggle T4 x2 (free tier, 30 hrs/week)

**Development time:** ~2 weeks so far

---

## What Makes This Different

1. **No pretrained image models.** The VAE, DiT, and full training pipeline are built from first principles — not fine-tuned from an existing checkpoint.

2. **Scientific debugging.** When something breaks, a diagnostic test isolates the cause before any retraining. Every problem has a root cause, and every root cause is documented.

3. **Constrained-hardware focus.** The entire system is designed to train and run on consumer GPUs. Efficiency is not an afterthought — it's the foundation.

4. **Full reproducibility.** Code, checkpoints, metrics, and experiment logs are versioned. Nothing is hidden behind a "magic" step.

---

## Roadmap

| Phase | Goal |
| :--- | :--- |
| **N8** (current) | Train DiT on real-world VAE latents at 256×256 |
| **N9** | Progressive resolution scaling (512×512) |
| **N10** | Public 500M-parameter model release |
| **N11** | Sparse MoE scaling for capacity (optional) |
| **N12** | Reasoning, layout control, quantized deployment |

**Target capability benchmarks:**
- Text rendering accuracy
- Prompt alignment
- Spatial reasoning
- Layout control

Detailed technical roadmap is available to collaborators.

---

## Why Build From Scratch?

Modern AI tooling makes it easy to fine-tune existing models. Much harder — and much rarer — is understanding and building the underlying systems.

RAY-IMAGE exists to:
- Understand diffusion models at the mathematical level
- Learn to train efficiently on constrained hardware
- Develop the skills to build the next generation of AI systems

Anyone can use an API. Building the system underneath it is where the real engineering lives.

---

## Tech Stack

| Layer | Tool |
| :--- | :--- |
| **Framework** | PyTorch |
| **Compute** | Kaggle (free tier, T4 x2) |
| **Code hosting** | GitHub |
| **Model hosting** | Hugging Face |
| **Data** | Open datasets (Apache 2.0) |

---

## License

Apache 2.0 — free for research and commercial use.

---

## Contact

For collaboration, research inquiries, or questions:  
**rishirajc406@gmail.com**

---

*Built from scratch, one experiment at a time.*