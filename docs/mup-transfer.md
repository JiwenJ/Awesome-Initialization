# μTransfer / μP Hyperparameter Transfer

> μP = **Maximal Update Parametrization**.  
> μTransfer = tune hyperparameters on a small μP proxy model and transfer them to a larger μP target model.

Snapshot: **2026-06-29**.

This page focuses on μP as a practical tool for **cross-scale hyperparameter transfer**, especially learning-rate transfer in Transformers and related architectures.

## One-paragraph summary

Standard parameterization often makes the best learning rate, initialization scale, and sometimes weight decay change with width. μP changes the scaling rules so that hidden-layer updates stay maximally large but stable as width grows. In that parameterization, many useful hyperparameters become approximately width-invariant. This motivates μTransfer: run sweeps on a cheap small model, then reuse the selected hyperparameters on the expensive large model.

## Terminology

- **SP** — standard parameterization, the usual finite-width training recipe.
- **NTK / lazy limit** — scaling regime where very wide networks behave like kernels and do little feature learning.
- **μP / muP** — maximal-update parameterization designed to preserve feature learning in the infinite-width limit.
- **μTransfer / muTransfer** — zero-shot hyperparameter transfer enabled by μP.
- **Coordinate check** — practical diagnostic: track activations / logits / update magnitudes across widths to confirm that the μP implementation scales correctly.
- **Base model / delta model / target model** — common μP implementation pattern: define a base shape, a slightly wider delta shape, and the target shape to infer scaling multipliers.

## Core reading path

| Order | Resource | Why it matters |
|---:|---|---|
| 1 | [Feature Learning in Infinite-Width Neural Networks](https://arxiv.org/abs/2011.14522) | Tensor Programs IV foundation: classifies infinite-width feature-learning vs kernel-like behavior. |
| 2 | [Tensor Programs V: Tuning Large Neural Networks via Zero-Shot Hyperparameter Transfer](https://arxiv.org/abs/2203.03466) | Main μTransfer paper: introduces the practical recipe for transferring hyperparameters from small to large models under μP. |
| 3 | [Tensor Programs IVb: Adaptive Optimization in the Infinite-Width Limit](https://arxiv.org/abs/2308.01814) | Extends the Tensor Programs analysis to adaptive optimizers such as Adam. |
| 4 | [Tensor Programs VI: Feature Learning in Infinite-Depth Neural Networks](https://arxiv.org/abs/2310.02244) | Introduces Depth-μP and discusses why depth transfer is harder than width transfer, especially for modern residual blocks. |
| 5 | [Quantifying Hyperparameter Transfer and the Importance of Embedding Layer Learning Rate](https://arxiv.org/abs/2605.21486) | Recent critical analysis: proposes metrics for transfer quality and highlights embedding-layer learning rate as a key bottleneck in AdamW LM training. |

## Foundational papers

| Year | Paper | Notes | Tags |
|---:|---|---|---|
| 2020 | [Feature Learning in Infinite-Width Neural Networks](https://arxiv.org/abs/2011.14522) | Shows how non-kernel, feature-learning infinite-width limits can be obtained with Tensor Programs. | Tensor Programs, feature learning |
| 2022 | [Tensor Programs V: Tuning Large Neural Networks via Zero-Shot Hyperparameter Transfer](https://arxiv.org/abs/2203.03466) | Introduces μTransfer; demonstrates transfer on Transformer and ResNet settings. | μP, μTransfer, LLMs |
| 2023 | [Tensor Programs IVb: Adaptive Optimization in the Infinite-Width Limit](https://arxiv.org/abs/2308.01814) | Generalizes analysis beyond SGD to adaptive optimizers. | Adam, adaptive optimization |
| 2023 | [Tensor Programs VI: Feature Learning in Infinite-Depth Neural Networks](https://arxiv.org/abs/2310.02244) | Studies depthwise parameterizations; proposes Depth-μP for single-layer residual blocks and discusses limitations for deeper blocks. | Depth-μP, ResNets, Transformers |

## Recent papers and extensions

| Year | Paper | Main contribution | Tags |
|---:|---|---|---|
| 2024 | [Super Consistency of Neural Network Landscapes and Learning Rate Transfer](https://arxiv.org/abs/2402.17457) | Studies why learning-rate transfer can occur by analyzing Hessian sharpness and landscape consistency under μP-like feature-learning limits. | sharpness, landscape |
| 2024 | [Sparse maximal update parameterization: A holistic approach to sparse training dynamics](https://arxiv.org/abs/2405.15743) | SμPar extends maximal-update ideas to sparse neural networks and transfers hyperparameters across width and sparsity. | sparsity, SμPar |
| 2024 | [u-μP: The Unit-Scaled Maximal Update Parametrization](https://arxiv.org/abs/2407.17465) | Combines μP with Unit Scaling; aims for simpler defaults and low-precision / FP8-friendly training. | unit scaling, FP8 |
| 2024 | [Power Scheduler: A Batch Size and Token Number Agnostic Learning Rate Scheduler](https://arxiv.org/abs/2408.13359) | Studies learning-rate transfer across batch size and token count; combines with μP for broader hyperparameter portability. | scheduler, tokens, batch |
| 2025 | [Scaling Diffusion Transformers Efficiently via μP](https://arxiv.org/abs/2505.15270) | Generalizes μP to diffusion Transformer families such as DiT, U-ViT, PixArt-α, and MMDiT. | diffusion, DiT |
| 2025 | [Maximal Update Parametrization and Zero-Shot Hyperparameter Transfer for Fourier Neural Operators](https://arxiv.org/abs/2506.19396) | Derives μTransfer-FNO for scaling Fourier Neural Operators by Fourier modes. | neural operators, PDE |
| 2026 | [Learning Rate Scaling across LoRA Ranks and Transfer to Full Finetuning](https://arxiv.org/abs/2602.06204) | Proposes maximal-update-style rules for LoRA rank scaling and transfer from LoRA to full finetuning. | LoRA, finetuning |
| 2026 | [Hyperparameter Transfer Laws for Non-Recurrent Multi-Path Neural Networks](https://arxiv.org/abs/2602.07494) | Introduces effective-depth scaling laws for multi-path architectures and learning-rate transfer across depth and width. | depth, multi-path |
| 2026 | [Rethinking Language Model Scaling under Transferable Hypersphere Optimization](https://arxiv.org/abs/2603.28743) | HyperP: transfer laws under Frobenius-sphere constraints with Muon; an adjacent alternative to μP-style transfer. | Muon, HyperP, MoE |
| 2026 | [Scaling Probabilistic Transformer via Efficient Cross-Scale Hyperparameter Transfer](https://arxiv.org/abs/2604.25409) | Applies μP-style transfer to Probabilistic Transformers and scales them to larger MLM settings. | probabilistic Transformer |
| 2026 | [GQA-μP: The maximal parameterization update for grouped query attention](https://arxiv.org/abs/2605.15290) | Derives μP scalings for grouped-query attention and studies transfer over GQA repetition and weight decay. | GQA, attention |
| 2026 | [Quantifying Hyperparameter Transfer and the Importance of Embedding Layer Learning Rate](https://arxiv.org/abs/2605.21486) | Defines transfer-quality metrics and argues that embedding learning-rate scaling explains much of μP's practical AdamW benefit in LMs. | embedding LR, AdamW |

## Implementation resources

| Resource | Notes |
|---|---|
| [microsoft/mup](https://github.com/microsoft/mup) | Reference PyTorch implementation for μP / μTransfer. |
| [EleutherAI/nanoGPT-mup](https://github.com/EleutherAI/nanoGPT-mup) | Minimal GPT-style implementation for experiments and coordinate checks. |
| [EleutherAI/nanoGPT-mup/tree/supar](https://github.com/EleutherAI/nanoGPT-mup/tree/supar) | Minimal implementation for SμPar sparse maximal update parameterization. |
| [microsoft/ArchScale](https://github.com/microsoft/ArchScale) | Released with HyperP / hypersphere optimization transfer work. |

## Practical checklist for μTransfer experiments

1. **Decide which axes transfer.** Classic μP targets width transfer. Depth, batch size, sequence length, token budget, sparsity, LoRA rank, GQA groups, and optimizer changes need additional rules or validation.
2. **Use matched model families.** Keep architecture, tokenizer, data distribution, objective, optimizer, schedule shape, and normalization choices as close as possible between proxy and target.
3. **Separate parameter groups.** Embeddings, hidden matrices, readout/output projection, biases, norm parameters, and special architectural parameters may need distinct initialization and learning-rate multipliers.
4. **Run coordinate checks.** Check activation scales, logits, gradients, and update magnitudes across at least two small widths before trusting the transfer.
5. **Tune on a proxy sweep.** Sweep learning rate first; then consider weight decay, warmup, schedule constants, dropout, batch size, and initialization multipliers.
6. **Validate a narrow target band.** Even with μTransfer, run a small sanity sweep around the transferred learning rate on the largest feasible target when budget allows.
7. **Watch embeddings.** Recent work suggests embedding-layer learning rate can dominate transfer quality in AdamW language-model training, so treat embeddings as first-class hyperparameters.
8. **Document failures.** Failed transfer cases are useful: record which axis changed, which parameter groups were scaled, and which coordinate checks failed.

## Known limitations and open questions

- **Width is better understood than depth.** Depth-μP works cleanly in restricted residual-block settings, but modern multi-layer blocks and Transformers introduce complications.
- **Embedding and output layers are special.** Transfer can appear to fail because embeddings/readouts are not scaled or optimized consistently.
- **Optimizer dependence matters.** AdamW, Muon, SGD, and hypersphere-constrained updates can have different transfer laws.
- **Schedule and data scaling are separate axes.** μP does not automatically solve transfer across token budget, batch size, or schedule length.
- **Architecture-specific derivations are often needed.** GQA, MoE, LoRA, sparsity, diffusion objectives, and neural operators all introduce extra scaling axes.
- **Theory vs practice gap remains.** Recent empirical work questions whether the standard μP explanation fully accounts for AdamW LM transfer, especially around embeddings.

## Search keywords

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
- `SμPar`
- `HyperP Muon`
