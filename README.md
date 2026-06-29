# Awesome Initialization

> Curated resources on neural-network initialization, scaling parameterizations, and cross-scale hyperparameter transfer.

[![Awesome](https://awesome.re/badge-flat2.svg)](https://awesome.re)
[![Scope](https://img.shields.io/badge/scope-initialization%20%7C%20scaling%20%7C%20transfer-2f6f9f)](#scope)
[![PRs welcome](https://img.shields.io/badge/PRs-welcome-2ea44f)](CONTRIBUTING.md)

This repository tracks papers, implementation notes, and practical checklists for initialization and scaling recipes that make neural-network training more predictable across model sizes.

Current focus: **μP / muP**, **μTransfer**, and adjacent hyperparameter-transfer methods.

> Snapshot: **2026-06-29**.

## Contents

- [Start here](#start-here)
- [Scope](#scope)
- [Topic map](#topic-map)
- [Paper timeline](#paper-timeline)
- [Learning resources and blogs](#learning-resources-and-blogs)
- [Code and implementations](#code-and-implementations)
- [Repository layout](#repository-layout)
- [Contributing](#contributing)

## Start Here

| Order | Theme | Resource |
|---:|---|---|
| 1 | Theory foundation | [Feature Learning in Infinite-Width Neural Networks](https://arxiv.org/abs/2011.14522) |
| 2 | Core μTransfer recipe | [Tensor Programs V: Tuning Large Neural Networks via Zero-Shot Hyperparameter Transfer](https://arxiv.org/abs/2203.03466) |
| 3 | Adaptive optimizers | [Tensor Programs IVb: Adaptive Optimization in the Infinite-Width Limit](https://arxiv.org/abs/2308.01814) |
| 4 | Depth scaling | [Tensor Programs VI: Feature Learning in Infinite-Depth Neural Networks](https://arxiv.org/abs/2310.02244) |
| 5 | Practical caveat | [Quantifying Hyperparameter Transfer and the Importance of Embedding Layer Learning Rate](https://arxiv.org/abs/2605.21486) |

For the detailed guide and practical checklist, see [μTransfer / μP Hyperparameter Transfer](docs/mup-transfer.md).

## Scope

μP is not just an initialization trick. It is a scaling-aware parameterization recipe covering initialization scales, learning-rate multipliers, readout and embedding treatment, and optimizer parameter groups. The practical goal of μTransfer is:

1. Convert the target architecture to μP.
2. Tune key hyperparameters on a small proxy model.
3. Transfer those hyperparameters to a much larger model without re-running the expensive sweep.

The broader scope includes neural-network initialization, scaling limits, SP / NTK / mean-field parameterizations, coordinate checks, and empirical reports on when hyperparameter transfer succeeds or fails.

## Topic Map

| Topic | What to look for |
|---|---|
| Width transfer | Original μTransfer framing and practical Transformer results. |
| Depth transfer | Depth-μP, effective-depth laws, and limits in modern residual blocks. |
| Embedding / readout scaling | Cases where embedding-layer learning rate or output scaling controls transfer quality. |
| Architecture-specific μP | GQA, diffusion Transformers, probabilistic Transformers, Fourier neural operators, sparse models, and LoRA. |
| Optimizer-specific transfer | Adaptive optimizers, Muon / hypersphere optimization, and optimizer-dependent scaling rules. |
| Schedule and data scaling | Token-budget, batch-size, warmup, and learning-rate schedule transfer. |

## Paper Timeline

The table is ordered by arXiv `published` date in reverse chronological order. The current two-year sweep covers papers from **2024-06-29 to 2026-06-29**; earlier rows are retained as foundational context.

| Date | Paper | Main contribution | Tags |
|---|---|---|---|
| 2026-06-02 | [Unlocking Feature Learning in Gated Delta Networks at Scale](https://arxiv.org/abs/2606.04048) | Derives μP scaling rules for Gated Delta Networks and validates width learning-rate transfer under AdamW and SGD. | Gated Delta Networks, sequence models |
| 2026-05-23 | [Feature Learning in Wide Neural Networks under μP](https://arxiv.org/abs/2605.24710) | Studies identifiability, sparse-dictionary structure, and mean-field feature-learning limits for wide two-layer networks under μP. | mean field, identifiability |
| 2026-05-20 | [Quantifying Hyperparameter Transfer and the Importance of Embedding Layer Learning Rate](https://arxiv.org/abs/2605.21486) | Defines transfer-quality metrics and argues that embedding learning-rate scaling explains much of μP's practical AdamW benefit in LMs. | embedding LR, AdamW |
| 2026-05-14 | [GQA-μP: The maximal parameterization update for grouped query attention](https://arxiv.org/abs/2605.15290) | Derives μP scalings for grouped-query attention and studies transfer over GQA repetition and weight decay. | GQA, attention |
| 2026-05-13 | [How to Scale Mixture-of-Experts: From muP to the Maximally Scale-Stable Parameterization](https://arxiv.org/abs/2605.14200) | Analyzes MoE scaling regimes, shows where μP transfer can fail, and derives MSSP for robust learning-rate transfer. | MoE, MSSP |
| 2026-04-28 | [Scaling Probabilistic Transformer via Efficient Cross-Scale Hyperparameter Transfer](https://arxiv.org/abs/2604.25409) | Applies μP-style transfer to Probabilistic Transformers and scales them to larger MLM settings. | probabilistic Transformer |
| 2026-03-30 | [Rethinking Language Model Scaling under Transferable Hypersphere Optimization](https://arxiv.org/abs/2603.28743) | HyperP: transfer laws under Frobenius-sphere constraints with Muon; an adjacent alternative to μP-style transfer. | Muon, HyperP, MoE |
| 2026-02-28 | [Spectral Condition for μP under Width-Depth Scaling](https://arxiv.org/abs/2603.00541) | Builds a unified spectral recipe for μP under joint width-depth scaling, including practical multi-transformation residual blocks. | width-depth, spectral conditions |
| 2026-02-24 | [Extending μP: Spectral Conditions for Feature Learning Across Optimizers](https://arxiv.org/abs/2602.20937) | Uses spectral conditions to derive μP-style transfer rules for AdamW, ADOPT, LAMB, Sophia, Shampoo, and Muon. | optimizers, spectral conditions |
| 2026-02-07 | [Hyperparameter Transfer Laws for Non-Recurrent Multi-Path Neural Networks](https://arxiv.org/abs/2602.07494) | Introduces effective-depth scaling laws for multi-path architectures and learning-rate transfer across depth and width. | depth, multi-path |
| 2026-02-05 | [Learning Rate Scaling across LoRA Ranks and Transfer to Full Finetuning](https://arxiv.org/abs/2602.06204) | Proposes maximal-update-style rules for LoRA rank scaling and transfer from LoRA to full finetuning. | LoRA, finetuning |
| 2026-01-13 | [Controlled LLM Training on Spectral Sphere](https://arxiv.org/abs/2601.08393) | Proposes a spectral-sphere optimizer designed to align both weights and updates with μP-style scale control. | spectral constraints, optimizer |
| 2025-12-28 | [Understanding the Mechanisms of Fast Hyperparameter Transfer](https://arxiv.org/abs/2512.22768) | Formalizes fast hyperparameter transfer and studies when μP transfer is compute-efficient versus when it fails. | mechanism, fast transfer |
| 2025-12-26 | [Completed Hyperparameter Transfer across Modules, Width, Depth, Batch and Duration](https://arxiv.org/abs/2512.22382) | Extends CompleteP-style transfer to modules, width, depth, batch size, training duration, and per-module hyperparameters. | CompleteP, batch, duration |
| 2025-11-03 | [A Proof of Learning Rate Transfer under μP](https://arxiv.org/abs/2511.01734) | Proves width learning-rate transfer for linear MLPs under μP and contrasts it with SP and NTP. | theory, LR transfer |
| 2025-10-21 | [Weight Decay may matter more than muP for Learning Rate Transfer in Practice](https://arxiv.org/abs/2510.19093) | Challenges the practical mechanism of μP transfer in LLM settings and argues weight decay often stabilizes representation updates after early training. | critique, weight decay |
| 2025-10-17 | [Robust Layerwise Scaling Rules by Proper Weight Decay Tuning](https://arxiv.org/abs/2510.15262) | Extends μP-style transfer into the AdamW steady state by coupling matrix learning-rate scaling with weight-decay scaling. | weight decay, AdamW |
| 2025-10-05 | [Arithmetic-Mean μP for Modern Architectures](https://arxiv.org/abs/2510.04327) | Replaces per-layer maximal-update constraints with an average update criterion for CNNs and ResNets, yielding width-robust depth laws. | AM-μP, CNNs, ResNets |
| 2025-06-24 | [Maximal Update Parametrization and Zero-Shot Hyperparameter Transfer for Fourier Neural Operators](https://arxiv.org/abs/2506.19396) | Derives μTransfer-FNO for scaling Fourier Neural Operators by Fourier modes. | neural operators, PDE |
| 2025-06-17 | [Optimal Embedding Learning Rate in LLMs: The Effect of Vocabulary Size](https://arxiv.org/abs/2506.15025) | Derives how the embedding learning rate should scale with vocabulary size, complementing recent μP transfer caveats for language models. | embedding LR, vocabulary |
| 2025-05-21 | [Scaling Diffusion Transformers Efficiently via μP](https://arxiv.org/abs/2505.15270) | Generalizes μP to diffusion Transformer families such as DiT, U-ViT, PixArt-α, and MMDiT. | diffusion, DiT |
| 2025-05-02 | [Don't be lazy: CompleteP enables compute-efficient deep transformers](https://arxiv.org/abs/2505.01618) | Proposes CompleteP for depth-wise hyperparameter transfer while avoiding lazy learning in deep Transformers. | CompleteP, depth, Transformers |
| 2025-03-12 | [Global Convergence and Rich Feature Learning in L-Layer Infinite-Width Neural Networks under μP Parametrization](https://arxiv.org/abs/2503.09565) | Proves global convergence while preserving rich feature learning for L-layer infinite-width networks trained with SGD under μP. | theory, global convergence |
| 2024-11-11 | [Warmstarting for Scaling Language Models](https://arxiv.org/abs/2411.07340) | Studies μTransfer-compatible warmstarting from smaller language models via shrink, zero-padding, and μP-scaled perturbations. | warmstart, LLMs |
| 2024-11-04 | [Local Loss Optimization in the Infinite Width](https://arxiv.org/abs/2411.02001) | Introduces μP-style stable parameterizations for predictive coding and target propagation under local-loss training. | local learning, predictive coding |
| 2024-10-31 | [μP²: Effective Sharpness Aware Minimization Requires Layerwise Perturbation Scaling](https://arxiv.org/abs/2411.00075) | Extends maximal-update ideas to SAM by scaling layerwise perturbations so learning rate and perturbation radius transfer jointly. | SAM, perturbation scaling |
| 2024-08-23 | [Power Scheduler: A Batch Size and Token Number Agnostic Learning Rate Scheduler](https://arxiv.org/abs/2408.13359) | Studies learning-rate transfer across batch size and token count; combines with μP for broader hyperparameter portability. | scheduler, tokens, batch |
| 2024-07-24 | [u-μP: The Unit-Scaled Maximal Update Parametrization](https://arxiv.org/abs/2407.17465) | Combines μP with Unit Scaling; aims for simpler defaults and low-precision / FP8-friendly training. | unit scaling, FP8 |
| 2024-07-08 | [Scaling Exponents Across Parameterizations and Optimizers](https://arxiv.org/abs/2407.05872) | Large empirical/theoretical study of learning-rate scaling across optimizers and parameterizations; argues that transfer can occur beyond classical μP and highlights Adam epsilon scaling. | scaling exponents, optimizers |
| 2024-05-24 | [Sparse maximal update parameterization: A holistic approach to sparse training dynamics](https://arxiv.org/abs/2405.15743) | SμPar extends maximal-update ideas to sparse neural networks and transfers hyperparameters across width and sparsity. | sparsity, SμPar |
| 2024-02-27 | [Super Consistency of Neural Network Landscapes and Learning Rate Transfer](https://arxiv.org/abs/2402.17457) | Studies why learning-rate transfer can occur by analyzing Hessian sharpness and landscape consistency under μP-like feature-learning limits. | sharpness, landscape |
| 2023-10-03 | [Tensor Programs VI: Feature Learning in Infinite-Depth Neural Networks](https://arxiv.org/abs/2310.02244) | Studies depthwise parameterizations; proposes Depth-μP for single-layer residual blocks and discusses limitations for deeper blocks. | Depth-μP, ResNets, Transformers |
| 2023-08-03 | [Tensor Programs IVb: Adaptive Optimization in the Infinite-Width Limit](https://arxiv.org/abs/2308.01814) | Generalizes analysis beyond SGD to adaptive optimizers. | Adam, adaptive optimization |
| 2022-03-07 | [Tensor Programs V: Tuning Large Neural Networks via Zero-Shot Hyperparameter Transfer](https://arxiv.org/abs/2203.03466) | Introduces μTransfer; demonstrates transfer on Transformer and ResNet settings. | μP, μTransfer, LLMs |
| 2020-11-30 | [Feature Learning in Infinite-Width Neural Networks](https://arxiv.org/abs/2011.14522) | Shows how non-kernel, feature-learning infinite-width limits can be obtained with Tensor Programs. | Tensor Programs, feature learning |

## Learning Resources and Blogs

| Resource | Type | Notes |
|---|---|---|
| [The Practitioner's Guide to the Maximal Update Parameterization](https://www.cerebras.ai/blog/the-practitioners-guide-to-the-maximal-update-parameterization) | Guide / blog | Practical implementation guide linked by `EleutherAI/nanoGPT-mup`; useful for coordinate checks and small GPT experiments. |
| [μTransfer: A technique for hyperparameter tuning of enormous neural networks](https://www.microsoft.com/en-us/research/blog/%C2%B5transfer-a-technique-for-hyperparameter-tuning-of-enormous-neural-networks/) | Blog | Microsoft Research explainer for μTransfer and the Tensor Programs V workflow. |
| [francesco-innocenti/mup-papers](https://github.com/francesco-innocenti/mup-papers) | Curated list | Active community list of mean-field / maximal-update parameterisation papers, grouped by theory and extensions. |
| [unit-scaling documentation](https://graphcore-research.github.io/unit-scaling) | Documentation | Docs and examples for the PyTorch unit-scaling library used by u-μP. |

## Code and Implementations

| Resource | Related method | Notes |
|---|---|---|
| [microsoft/mup](https://github.com/microsoft/mup) | μP / μTransfer | Reference PyTorch implementation mentioned by Tensor Programs V. |
| [EleutherAI/nanoGPT-mup](https://github.com/EleutherAI/nanoGPT-mup) | μP for GPTs | Compact GPT-style implementation with marked μP changes, examples, and coordinate-check scripts. |
| [EleutherAI/nanoGPT-mup/tree/supar](https://github.com/EleutherAI/nanoGPT-mup/tree/supar) | SμPar | Minimal implementation for sparse maximal update parameterization. |
| [EleutherAI/nanoGPT-mup/tree/completep](https://github.com/EleutherAI/nanoGPT-mup/tree/completep) | CompleteP | Minimal implementation for CompleteP depth-wise transfer experiments. |
| [graphcore-research/unit-scaling](https://github.com/graphcore-research/unit-scaling) | u-μP | PyTorch library for Unit-Scaled Maximal Update Parameterization. |
| [LithiumDA/muTransfer-FNO](https://github.com/LithiumDA/muTransfer-FNO) | μTransfer-FNO | Official implementation for zero-shot hyperparameter transfer in Fourier Neural Operators. |
| [muTransfer-FNO data](https://huggingface.co/datasets/LDA1020/muTransfer-FNO-data/tree/main) | μTransfer-FNO | Dataset release used by the μTransfer-FNO experiments. |
| [ML-GSAI/Width-Depth-muP](https://github.com/ML-GSAI/Width-Depth-muP) | Width-depth μP | Official implementation for spectral conditions under joint width-depth scaling. |
| [microsoft/ArchScale](https://github.com/microsoft/ArchScale) | HyperP / architecture scaling | Codebase released with HyperP, SqrtGate, and MuonH scaling experiments. |
| [Unakar/Spectral-Sphere-Optimizer](https://github.com/Unakar/Spectral-Sphere-Optimizer) | Spectral Sphere Optimizer | Official implementation for spectral-sphere μP-aligned optimization. |
| [Unakar/Megatron-LM/tree/SSO_main](https://github.com/Unakar/Megatron-LM/tree/SSO_main) | Spectral Sphere Optimizer | Megatron-LM implementation branch for large-scale SSO experiments. |
| [Melina-Jingting/mup-equinox](https://github.com/Melina-Jingting/mup-equinox) | JAX / Equinox μP | Lightweight community library bringing μP-style modules and base-shape utilities to Equinox. |

## Repository Layout

| Path | Purpose |
|---|---|
| [docs/mup-transfer.md](docs/mup-transfer.md) | Main μP / μTransfer reading guide, paper timeline, resources, code links, and practical checklist. |
| [papers/mup-transfer.bib](papers/mup-transfer.bib) | BibTeX references for the μP / μTransfer collection. |
| [papers/README.md](papers/README.md) | Notes on maintaining reference files. |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Contribution scope and entry template. |

## Contributing

Useful additions include papers, implementation notes, coordinate-check scripts, empirical transfer reports, and short writeups of failed transfer cases. Please include source links, arXiv IDs when available, and a one-sentence reason why the resource matters.

See [CONTRIBUTING.md](CONTRIBUTING.md) for the suggested format.
