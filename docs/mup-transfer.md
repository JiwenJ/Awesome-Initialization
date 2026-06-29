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
- [Learning resources and blogs](#learning-resources-and-blogs)
- [Code and implementations](#code-and-implementations)
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

Coverage includes core μP / maximal-update papers, closely related hyperparameter-transfer work, and application reports that materially use μP-style scaling. Unrelated keyword hits such as particle-physics `μp` or generic non-ML uses are excluded.

| Date | Paper | Main contribution | Tags |
|---|---|---|---|
| 2026-06-16 | [On the Residual Scaling of Looped Transformers: Stability and Transferability](https://arxiv.org/abs/2606.18524) | Derives residual scaling for weight-tied looped Transformers so learning rates transfer across loop counts. | looped Transformers, residual scaling |
| 2026-06-02 | [Unlocking Feature Learning in Gated Delta Networks at Scale](https://arxiv.org/abs/2606.04048) | Derives μP scaling rules for Gated Delta Networks and validates width learning-rate transfer under AdamW and SGD. | Gated Delta Networks, sequence models |
| 2026-05-23 | [Feature Learning in Wide Neural Networks under μP](https://arxiv.org/abs/2605.24710) | Studies identifiability, sparse-dictionary structure, and mean-field feature-learning limits for wide two-layer networks under μP. | mean field, identifiability |
| 2026-05-20 | [Quantifying Hyperparameter Transfer and the Importance of Embedding Layer Learning Rate](https://arxiv.org/abs/2605.21486) | Defines transfer-quality metrics and argues that embedding learning-rate scaling explains much of μP's practical AdamW benefit in LMs. | embedding LR, AdamW |
| 2026-05-19 | [Toto 2.0: Time Series Forecasting Enters the Scaling Era](https://arxiv.org/abs/2605.20119) | Reports a time-series foundation-model scaling recipe using a u-μP hyperparameter-transfer pipeline from 4M to 2.5B parameters. | application report, u-μP |
| 2026-05-18 | [Scale-Invariant Neural Network Optimization: Norm Geometry and Heavy-Tailed Noise](https://arxiv.org/abs/2605.18528) | Analyzes scale-invariant optimizers and norm geometry for parameterization-aware hyperparameter transfer under heavy-tailed noise. | scale-invariant optimization, theory |
| 2026-05-14 | [GQA-μP: The maximal parameterization update for grouped query attention](https://arxiv.org/abs/2605.15290) | Derives μP scalings for grouped-query attention and studies transfer over GQA repetition and weight decay. | GQA, attention |
| 2026-05-13 | [How to Scale Mixture-of-Experts: From muP to the Maximally Scale-Stable Parameterization](https://arxiv.org/abs/2605.14200) | Analyzes MoE scaling regimes, shows where μP transfer can fail, and derives MSSP for robust learning-rate transfer. | MoE, MSSP |
| 2026-05-11 | [Hyperparameter Transfer for Dense Associative Memories](https://arxiv.org/abs/2605.10164) | Derives hyperparameter-transfer prescriptions for Dense Associative Memories with shared weights and sharp activations. | DenseAM, HPT |
| 2026-05-08 | [Spectral Dynamics in Deep Networks: Feature Learning, Outlier Escape, and Learning Rate Transfer](https://arxiv.org/abs/2605.07870) | Tracks bulk and outlier spectral dynamics and shows μP yields width-consistent learning-rate transfer in deep linear settings. | spectral dynamics, theory |
| 2026-05-08 | [OrScale: Orthogonalised Optimization with Layer-Wise Trust-Ratio Scaling](https://arxiv.org/abs/2605.07815) | Extends Muon with trust-ratio scaling and analyzes calibration that preserves μP-style learning-rate transfer at initialization. | Muon, trust ratio |
| 2026-04-29 | [Learning Rate Transfer in Normalized Transformers](https://arxiv.org/abs/2604.27077) | Revisits μP for nGPT and proposes νGPT, enabling learning-rate transfer across width, depth, and token horizon. | nGPT, νGPT |
| 2026-04-28 | [Scaling Probabilistic Transformer via Efficient Cross-Scale Hyperparameter Transfer](https://arxiv.org/abs/2604.25409) | Applies μP-style transfer to Probabilistic Transformers and scales them to larger MLM settings. | probabilistic Transformer |
| 2026-03-30 | [Rethinking Language Model Scaling under Transferable Hypersphere Optimization](https://arxiv.org/abs/2603.28743) | HyperP: transfer laws under Frobenius-sphere constraints with Muon; an adjacent alternative to μP-style transfer. | Muon, HyperP, MoE |
| 2026-03-16 | [Deriving Hyperparameter Scaling Laws via Modern Optimization Theory](https://arxiv.org/abs/2603.15958) | Uses modern optimizer convergence bounds to derive learning-rate, momentum, and batch-size scaling laws complementary to μP. | optimizer theory, schedules |
| 2026-03-10 | [On the Width Scaling of Neural Optimizers Under Matrix Operator Norms I: Row/Column Normalization and Hyperparameter Transfer](https://arxiv.org/abs/2603.09952) | Interprets AdamW and Muon through matrix operator norms and derives width-scaling rules for optimizer hyperparameter transfer. | operator norms, optimizers |
| 2026-02-28 | [Spectral Condition for μP under Width-Depth Scaling](https://arxiv.org/abs/2603.00541) | Builds a unified spectral recipe for μP under joint width-depth scaling, including practical multi-transformation residual blocks. | width-depth, spectral conditions |
| 2026-02-26 | [Summer-22B: A Systematic Approach to Dataset Engineering and Training at Scale for Video Foundation Model](https://arxiv.org/abs/2603.00173) | Reports large-scale video foundation-model training using μP parameterization and hypersphere-constrained optimization. | application report, video |
| 2026-02-24 | [Extending μP: Spectral Conditions for Feature Learning Across Optimizers](https://arxiv.org/abs/2602.20937) | Uses spectral conditions to derive μP-style transfer rules for AdamW, ADOPT, LAMB, Sophia, Shampoo, and Muon. | optimizers, spectral conditions |
| 2026-02-11 | [μpscaling small models: Principled warm starts and hyperparameter transfer](https://arxiv.org/abs/2602.10545) | Introduces μP-motivated upscaling and warm-start methods that preserve hyperparameter transfer when growing model width. | warmstart, upscaling |
| 2026-02-07 | [Hyperparameter Transfer Laws for Non-Recurrent Multi-Path Neural Networks](https://arxiv.org/abs/2602.07494) | Introduces effective-depth scaling laws for multi-path architectures and learning-rate transfer across depth and width. | depth, multi-path |
| 2026-02-07 | [On the Infinite Width and Depth Limits of Predictive Coding Networks](https://arxiv.org/abs/2602.07697) | Shows predictive-coding networks share width- and depth-stable feature-learning parameterizations with backpropagation. | predictive coding, width-depth |
| 2026-02-05 | [Learning Rate Scaling across LoRA Ranks and Transfer to Full Finetuning](https://arxiv.org/abs/2602.06204) | Proposes maximal-update-style rules for LoRA rank scaling and transfer from LoRA to full finetuning. | LoRA, finetuning |
| 2026-01-31 | [Over-Alignment vs Over-Fitting: The Role of Feature Learning Strength in Generalization](https://arxiv.org/abs/2602.00827) | Studies how feature-learning strength affects generalization and identifies regimes where stronger feature learning over-aligns. | feature learning strength |
| 2026-01-28 | [Hyperparameter Transfer with Mixture-of-Expert Layers](https://arxiv.org/abs/2601.20205) | Proposes a MoE Transformer parameterization for transferring hyperparameters across width, depth, expert count, and expert size. | MoE, HPT |
| 2026-01-25 | [IMU-1: Sample-Efficient Pre-training of Small Language Models](https://arxiv.org/abs/2602.02522) | Documents a small-LM training recipe combining NorMuon, cautious weight decay, and μP parameterization. | application report, LLM |
| 2026-01-13 | [Controlled LLM Training on Spectral Sphere](https://arxiv.org/abs/2601.08393) | Proposes a spectral-sphere optimizer designed to align both weights and updates with μP-style scale control. | spectral constraints, optimizer |
| 2026-01-08 | [Learnable Multipliers: Freeing the Scale of Language Model Matrix Layers](https://arxiv.org/abs/2601.04890) | Generalizes μP multipliers into learnable row/column scaling factors for language-model matrix layers. | multipliers, LLMs |
| 2026-01-04 | [Towards a Principled Muon under μP: Ensuring Spectral Conditions throughout Training](https://arxiv.org/abs/2601.01306) | Develops Muon++ to maintain μP spectral conditions throughout training without repeated weight normalization. | Muon, spectral conditions |
| 2025-12-31 | [Dynamic Large Concept Models: Latent Reasoning in an Adaptive Semantic Space](https://arxiv.org/abs/2512.24617) | Uses decoupled μP parameterization for zero-shot hyperparameter transfer across widths and compression regimes. | application report, decoupled μP |
| 2025-12-28 | [Understanding the Mechanisms of Fast Hyperparameter Transfer](https://arxiv.org/abs/2512.22768) | Formalizes fast hyperparameter transfer and studies when μP transfer is compute-efficient versus when it fails. | mechanism, fast transfer |
| 2025-12-26 | [Completed Hyperparameter Transfer across Modules, Width, Depth, Batch and Duration](https://arxiv.org/abs/2512.22382) | Extends CompleteP-style transfer to modules, width, depth, batch size, training duration, and per-module hyperparameters. | CompleteP, batch, duration |
| 2025-12-20 | [Towards Guided Descent: Optimization Algorithms for Training Neural Networks At Scale](https://arxiv.org/abs/2512.18373) | Surveys practical large-scale optimization methods, including maximal-update parameterization as part of modern training workflows. | survey, optimization |
| 2025-12-05 | [Hyperparameter Transfer Enables Consistent Gains of Matrix-Preconditioned Optimizers Across Scales](https://arxiv.org/abs/2512.05620) | Studies μP-style hyperparameter transfer for Shampoo, SOAP, Muon, and related matrix-preconditioned optimizers. | preconditioners, optimizers |
| 2025-11-23 | [Xmodel-2.5: 1.3B Data-Efficient Reasoning SLM](https://arxiv.org/abs/2511.19496) | Uses μP to transfer hyperparameters from a 20M proxy to a 1.3B small language model. | application report, SLM |
| 2025-11-03 | [A Proof of Learning Rate Transfer under μP](https://arxiv.org/abs/2511.01734) | Proves width learning-rate transfer for linear MLPs under μP and contrasts it with SP and NTP. | theory, LR transfer |
| 2025-10-21 | [Weight Decay may matter more than muP for Learning Rate Transfer in Practice](https://arxiv.org/abs/2510.19093) | Challenges the practical mechanism of μP transfer in LLM settings and argues weight decay often stabilizes representation updates after early training. | critique, weight decay |
| 2025-10-17 | [Robust Layerwise Scaling Rules by Proper Weight Decay Tuning](https://arxiv.org/abs/2510.15262) | Extends μP-style transfer into the AdamW steady state by coupling matrix learning-rate scaling with weight-decay scaling. | weight decay, AdamW |
| 2025-10-05 | [Arithmetic-Mean μP for Modern Architectures](https://arxiv.org/abs/2510.04327) | Replaces per-layer maximal-update constraints with an average update criterion for CNNs and ResNets, yielding width-robust depth laws. | AM-μP, CNNs, ResNets |
| 2025-08-13 | [μ-Parametrization for Mixture of Experts](https://arxiv.org/abs/2508.09752) | Derives μ-parameterization rules for MoE models and demonstrates learning-rate transfer across model sizes. | MoE, μTransfer |
| 2025-07-11 | [Pre-Training LLMs on a budget: A comparison of three optimizers](https://arxiv.org/abs/2507.08472) | Uses maximal update parametrization and proxy models to tune AdamW, Lion, and Sophia for budget LLM pretraining. | optimizer comparison, LLM |
| 2025-07-02 | [Scaling Collapse Reveals Universal Dynamics in Compute-Optimally Trained Neural Networks](https://arxiv.org/abs/2507.02119) | Studies universal training dynamics and when suboptimal hyperparameter scaling breaks compute-optimal collapse. | scaling laws, dynamics |
| 2025-06-24 | [Maximal Update Parametrization and Zero-Shot Hyperparameter Transfer for Fourier Neural Operators](https://arxiv.org/abs/2506.19396) | Derives μTransfer-FNO for scaling Fourier Neural Operators by Fourier modes. | neural operators, PDE |
| 2025-06-17 | [Optimal Embedding Learning Rate in LLMs: The Effect of Vocabulary Size](https://arxiv.org/abs/2506.15025) | Derives how the embedding learning rate should scale with vocabulary size, complementing recent μP transfer caveats for language models. | embedding LR, vocabulary |
| 2025-05-28 | [On the Surprising Effectiveness of Large Learning Rates under Standard Width Scaling](https://arxiv.org/abs/2505.22491) | Analyzes feature learning under standard parameterization and explains large-learning-rate behavior beyond classical infinite-width predictions. | SP, feature learning |
| 2025-05-21 | [Scaling Diffusion Transformers Efficiently via μP](https://arxiv.org/abs/2505.15270) | Generalizes μP to diffusion Transformer families such as DiT, U-ViT, PixArt-α, and MMDiT. | diffusion, DiT |
| 2025-05-19 | [Power Lines: Scaling Laws for Weight Decay and Batch Size in LLM Pre-training](https://arxiv.org/abs/2505.13738) | Derives scaling laws for weight decay and batch size as model size, data size, and batch size change. | weight decay, batch size |
| 2025-05-19 | [μPC: Scaling Predictive Coding to 100+ Layer Networks](https://arxiv.org/abs/2505.13124) | Uses Depth-μP-style parameterization to train very deep predictive-coding networks. | predictive coding, Depth-μP |
| 2025-05-04 | [Practical Efficiency of Muon for Pretraining](https://arxiv.org/abs/2505.02222) | Studies Muon plus maximal update parameterization and gives a telescoping algorithm for efficient μP hyperparameter transfer. | Muon, μP |
| 2025-05-02 | [Don't be lazy: CompleteP enables compute-efficient deep transformers](https://arxiv.org/abs/2505.01618) | Proposes CompleteP for depth-wise hyperparameter transfer while avoiding lazy learning in deep Transformers. | CompleteP, depth, Transformers |
| 2025-03-12 | [Global Convergence and Rich Feature Learning in L-Layer Infinite-Width Neural Networks under μP Parametrization](https://arxiv.org/abs/2503.09565) | Proves global convergence while preserving rich feature learning for L-layer infinite-width networks trained with SGD under μP. | theory, global convergence |
| 2025-02-24 | [Function-Space Learning Rates](https://arxiv.org/abs/2502.17405) | Introduces FLeRM, a function-space learning-rate matching method for hyperparameter transfer across widths, depths, and LoRA ranks. | function-space LR, FLeRM |
| 2025-02-09 | [μnit Scaling: Simple and Scalable FP8 LLM Training](https://arxiv.org/abs/2502.05967) | Proposes μnit Scaling for FP8 LLM training with simple width-wise hyperparameter transfer and matched training/inference numerics. | FP8, u-μP |
| 2025-02-04 | [Deep Linear Network Training Dynamics from Random Initialization: Data, Width, Depth, and Hyperparameter Transfer](https://arxiv.org/abs/2502.02531) | Analyzes deep linear networks and captures maximum-update feature learning plus width/depth hyperparameter transfer effects. | deep linear networks, theory |
| 2024-11-11 | [Warmstarting for Scaling Language Models](https://arxiv.org/abs/2411.07340) | Studies μTransfer-compatible warmstarting from smaller language models via shrink, zero-padding, and μP-scaled perturbations. | warmstart, LLMs |
| 2024-11-04 | [Local Loss Optimization in the Infinite Width](https://arxiv.org/abs/2411.02001) | Introduces μP-style stable parameterizations for predictive coding and target propagation under local-loss training. | local learning, predictive coding |
| 2024-10-31 | [μP²: Effective Sharpness Aware Minimization Requires Layerwise Perturbation Scaling](https://arxiv.org/abs/2411.00075) | Extends maximal-update ideas to SAM by scaling layerwise perturbations so learning rate and perturbation radius transfer jointly. | SAM, perturbation scaling |
| 2024-10-06 | [The Optimization Landscape of SGD Across the Feature Learning Strength](https://arxiv.org/abs/2410.04642) | Studies how final-layer scaling controls feature-learning strength and changes optimal learning-rate regimes. | feature learning strength |
| 2024-08-23 | [Power Scheduler: A Batch Size and Token Number Agnostic Learning Rate Scheduler](https://arxiv.org/abs/2408.13359) | Studies learning-rate transfer across batch size and token count; combines with μP for broader hyperparameter portability. | scheduler, tokens, batch |
| 2024-07-31 | [TASI Lectures on Physics for Machine Learning](https://arxiv.org/abs/2408.00082) | Lecture notes covering neural-network theory, feature learning, and maximal update parameterization from a physics perspective. | lecture notes, theory |
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
