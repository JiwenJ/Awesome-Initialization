# μTransfer / μP Hyperparameter Transfer

> μP = **Maximal Update Parametrization**.  
> μTransfer = tune hyperparameters on a small μP proxy model and transfer them to a larger μP target model.

Snapshot: **2026-06-29**. The current two-year sweep covers papers from **2024-06-29 to 2026-06-29**; earlier 2024 rows are retained as near-cutoff context.

This page focuses on μP as a practical tool for **cross-scale hyperparameter transfer**, especially learning-rate transfer in Transformers and related architectures.

## Contents

- [Summary](#summary)
- [Terminology](#terminology)
- [Core reading path](#core-reading-path)
- [Paper timeline](#paper-timeline)
- [Implementation resources](#implementation-resources)
- [Practical checklist](#practical-checklist)
- [Known limitations and open questions](#known-limitations-and-open-questions)
- [Search keywords](#search-keywords)

## Summary

Standard parameterization often makes the best learning rate, initialization scale, and sometimes weight decay change with width. μP changes the scaling rules so that hidden-layer updates stay maximally large but stable as width grows. In that parameterization, many useful hyperparameters become approximately width-invariant. This motivates μTransfer: run sweeps on a cheap small model, then reuse the selected hyperparameters on the expensive large model.

| Axis | Practical question |
|---|---|
| Width | Does the tuned learning rate remain stable when hidden dimension grows? |
| Depth | Does the transfer law survive deeper residual structure or multi-layer blocks? |
| Parameter groups | Are embeddings, readouts, biases, norms, and hidden matrices scaled separately? |
| Optimizer | Does the rule hold under AdamW, SGD, Muon, or constrained optimization? |
| Data and schedule | Does changing batch size, token budget, warmup, or schedule length break transfer? |
| Architecture | Does the model introduce GQA, MoE, LoRA, diffusion objectives, sparsity, or neural-operator axes? |

## Terminology

| Term | Meaning |
|---|---|
| SP | Standard parameterization, the usual finite-width training recipe. |
| NTK / lazy limit | Scaling regime where very wide networks behave like kernels and do little feature learning. |
| μP / muP | Maximal-update parameterization designed to preserve feature learning in the infinite-width limit. |
| μTransfer / muTransfer | Zero-shot hyperparameter transfer enabled by μP. |
| Coordinate check | Diagnostic that tracks activations, logits, gradients, or update magnitudes across widths to confirm that the μP implementation scales correctly. |
| Base / delta / target model | Common implementation pattern: define a base shape, a slightly wider delta shape, and the target shape to infer scaling multipliers. |

## Core Reading Path

| Order | Resource | Why it matters |
|---:|---|---|
| 1 | [Feature Learning in Infinite-Width Neural Networks](https://arxiv.org/abs/2011.14522) | Tensor Programs IV foundation: classifies infinite-width feature-learning vs kernel-like behavior. |
| 2 | [Tensor Programs V: Tuning Large Neural Networks via Zero-Shot Hyperparameter Transfer](https://arxiv.org/abs/2203.03466) | Main μTransfer paper: introduces the practical recipe for transferring hyperparameters from small to large models under μP. |
| 3 | [Tensor Programs IVb: Adaptive Optimization in the Infinite-Width Limit](https://arxiv.org/abs/2308.01814) | Extends the Tensor Programs analysis to adaptive optimizers such as Adam. |
| 4 | [Tensor Programs VI: Feature Learning in Infinite-Depth Neural Networks](https://arxiv.org/abs/2310.02244) | Introduces Depth-μP and discusses why depth transfer is harder than width transfer, especially for modern residual blocks. |
| 5 | [Quantifying Hyperparameter Transfer and the Importance of Embedding Layer Learning Rate](https://arxiv.org/abs/2605.21486) | Recent critical analysis: proposes metrics for transfer quality and highlights embedding-layer learning rate as a key bottleneck in AdamW LM training. |

## Paper Timeline

The table is ordered by arXiv `published` date in reverse chronological order, matching the style of `JiwenJ/Awesome-Optimizers`.

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

## Implementation Resources

| Resource | Notes |
|---|---|
| [microsoft/mup](https://github.com/microsoft/mup) | Reference PyTorch implementation for μP / μTransfer. |
| [EleutherAI/nanoGPT-mup](https://github.com/EleutherAI/nanoGPT-mup) | Minimal GPT-style implementation for experiments and coordinate checks. |
| [EleutherAI/nanoGPT-mup/tree/supar](https://github.com/EleutherAI/nanoGPT-mup/tree/supar) | Minimal implementation for SμPar sparse maximal update parameterization. |
| [microsoft/ArchScale](https://github.com/microsoft/ArchScale) | Released with HyperP / hypersphere optimization transfer work. |

## Practical Checklist

1. **Decide which axes transfer.** Classic μP targets width transfer. Depth, batch size, sequence length, token budget, sparsity, LoRA rank, GQA groups, and optimizer changes need additional rules or validation.
2. **Use matched model families.** Keep architecture, tokenizer, data distribution, objective, optimizer, schedule shape, and normalization choices as close as possible between proxy and target.
3. **Separate parameter groups.** Embeddings, hidden matrices, readout/output projection, biases, norm parameters, and special architectural parameters may need distinct initialization and learning-rate multipliers.
4. **Run coordinate checks.** Check activation scales, logits, gradients, and update magnitudes across at least two small widths before trusting the transfer.
5. **Tune on a proxy sweep.** Sweep learning rate first; then consider weight decay, warmup, schedule constants, dropout, batch size, and initialization multipliers.
6. **Validate a narrow target band.** Even with μTransfer, run a small sanity sweep around the transferred learning rate on the largest feasible target when budget allows.
7. **Watch embeddings.** Recent work suggests embedding-layer learning rate can dominate transfer quality in AdamW language-model training, so treat embeddings as first-class hyperparameters.
8. **Document failures.** Failed transfer cases are useful: record which axis changed, which parameter groups were scaled, and which coordinate checks failed.

## Known Limitations and Open Questions

- **Width is better understood than depth.** Depth-μP works cleanly in restricted residual-block settings, but modern multi-layer blocks and Transformers introduce complications.
- **Embedding and output layers are special.** Transfer can appear to fail because embeddings/readouts are not scaled or optimized consistently.
- **Optimizer dependence matters.** AdamW, Muon, SGD, and hypersphere-constrained updates can have different transfer laws.
- **Schedule and data scaling are separate axes.** μP does not automatically solve transfer across token budget, batch size, or schedule length.
- **Architecture-specific derivations are often needed.** GQA, MoE, LoRA, sparsity, diffusion objectives, and neural operators all introduce extra scaling axes.
- **Theory vs practice gap remains.** Recent empirical work questions whether the standard μP explanation fully accounts for AdamW LM transfer, especially around embeddings.

## Search Keywords

Use these when extending the list:

- `maximal update parametrization`
- `maximal update parameterization`
- `muP hyperparameter transfer`
- `μTransfer`
- `Tensor Programs V`
- `Depth-μP`
- `coordinate check muP`
- `embedding learning rate muP`
- `GQA-μP`
- `u-μP`
- `μP^2`
- `CompleteP`
- `AM-μP`
- `MSSP`
- `SμPar`
- `HyperP Muon`
