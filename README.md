# HEaD+ : Hallucination Early Detection in Diffusion Models

[![Paper](https://img.shields.io/badge/Paper-IJCV%202026-blue)](https://link.springer.com/article/10.1007/s11263-025-02622-0)
[![Project Page](https://img.shields.io/badge/Project-Page-green)](https://aimagelab.github.io/HEaD/)

Official repository for the paper **"Hallucination Early Detection in Diffusion Models"** published at the International Journal of Computer Vision (IJCV) 2026.

## TL;DR

**HEaD+** predicts whether diffusion models will hallucinate (miss objects) **mid-generation**. If a hallucination is detected, it restarts with a new seed, saving up to **32% generation time** while achieving **6-8% more complete images** with all requested objects. It is model-agnostic and works with any diffusion model without retraining: both UNet-based (Stable Diffusion, TokenCompose) and DiT-based (PixArt-α).

## The Problem

Diffusion models often fail when generating images with multiple objects. With just 4 objects in the prompt, Stable Diffusion 1.4 produces a *complete* image (showing all objects) only **27%** of the time. However, testing different seeds shows that at least one produces a complete image in **80%** of prompts. The challenge is finding that good seed without wasting time on failed generations.

## Method

HEaD+ operates at a critical timestep during the diffusion process, combining three signals to predict whether each requested object will appear in the final image:

- **Predicted Final Image (PFI)**: a forecast of the final image at an intermediate step
- **Cross-Attention Maps**: show where the model is "looking" for each object
- **Textual Embeddings**: CLIP features of the requested objects

A lightweight Transformer Decoder processes these inputs to predict object presence. If any object is predicted missing, generation restarts with a new seed, catching failures early.

## Key Results

| Metric | Value |
|--------|-------|
| Increase in complete generations (4 objects, SD1.4) | **+8%** |
| Time saved when aiming for complete generation | **32%** |
| Images in InsideGen dataset | **45,000** |
| Retraining needed | **0** (works out-of-the-box) |

## InsideGen Dataset

We release **InsideGen**, a dataset of 45,000 generated images with cross-attention maps and Predicted Final Images at multiple timesteps. Each image includes hallucination labels for prompts with 2-7 objects.

**Timesteps captured:** 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 12, 14, 16, 18, 20, 25, 40

### Download

- [Stable Diffusion 1.4](https://ailb-web.ing.unimore.it/publicfiles/InsideGen/InsideGenSD1_4.tar.gz)
- [Stable Diffusion 2.1](https://ailb-web.ing.unimore.it/publicfiles/InsideGen/InsideGenSD2_1.tar.gz)

## Citation

If you find this work useful, please cite:

```bibtex
@article{betti2026head,
  title={Hallucination Early Detection in Diffusion Models},
  author={Betti, Federico and Baraldi, Lorenzo and Baraldi, Lorenzo and Cucchiara, Rita and Sebe, Nicu},
  journal={International Journal of Computer Vision},
  volume={134},
  pages={35},
  year={2026},
  publisher={Springer}
}
```

## Authors

- [Federico Betti](https://scholar.google.com/citations?user=Ms5ctkUAAAAJ&hl=en)* (University of Trento)
- [Lorenzo Baraldi](https://scholar.google.com/citations?user=nw7mrA0AAAAJ&hl=en)* (University of Pisa)
- [Lorenzo Baraldi](https://scholar.google.com/citations?user=V4RuMvsAAAAJ&hl=en) (University of Modena and Reggio Emilia)
- [Rita Cucchiara](https://scholar.google.com/citations?user=OM3sZEoAAAAJ&hl=en) (University of Modena and Reggio Emilia)
- [Nicu Sebe](https://scholar.google.com/citations?user=stFCYOAAAAAJ&hl=en) (University of Trento)

*Equal contribution

## License

This project is licensed under the terms of the MIT license.
