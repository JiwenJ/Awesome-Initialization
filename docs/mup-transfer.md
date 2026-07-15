# μTransfer / μP Hyperparameter Transfer

> μP = **Maximal Update Parametrization**.  
> μTransfer = tune hyperparameters on a small μP proxy model and transfer them to a larger μP target model.

Snapshot: **2026-07-15**. This page's full paper list contains **149** μP / μTransfer and closely related entries, with supplementary learning and implementation links below. The current two-year sweep covers papers from **2024-07-15 to 2026-07-15**; earlier rows are retained as foundational context.

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
| Architecture | Does the model introduce GNNs, GQA, MoE, LoRA, diffusion objectives, sparsity, or neural-operator axes? |

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

The table is ordered by arXiv `published` date, OpenReview public date, or venue date in reverse chronological order, matching the style of `JiwenJ/Awesome-Optimizers`.

Coverage includes core μP / maximal-update papers, closely related hyperparameter-transfer and token-horizon / schedule-scaling work, OpenReview / venue-only papers, and application reports that materially use μP-style scaling. Non-μP papers are included only when their main result directly addresses a transfer axis tracked here and are tagged as adjacent; unrelated keyword hits such as particle-physics `μp` or generic non-ML uses are excluded.

The **2026-07-15** audit cross-checked exact and broad arXiv searches, recent-category sweeps, OpenReview and venue records, full-text application reports, official project and model pages, and the active `francesco-innocenti/mup-papers` index. Alternate arXiv, OpenReview, workshop, and venue records for the same work are collapsed into one entry.

| Date | Paper | Main contribution | Tags |
|---|---|---|---|
| 2026-07-12 | [WSqD: A Horizon-Free Learning Rate Schedule for Large Model Training](https://arxiv.org/abs/2607.10959) | Replaces WSD's constant phase with a shifted inverse-square-root base schedule, reusing one peak learning rate across multiple language-model training horizons. | token horizon, horizon-free LR, WSD |
| 2026-07-08 | [Optimal Learning Rate Scaling Depends on Data in Deep Scalar Linear Networks](https://arxiv.org/abs/2607.07884) | Shows that depth-wise learning-rate scaling can depend on data even in deep scalar linear networks, marking a limit of data-agnostic transfer rules; the [earlier Sci4DL record](https://openreview.net/forum?id=ij3ZS20xMS) is retained for version history. | depth, LR scaling, negative result |
| 2026-07-06 | [Hyperparameter Transfer in Graph Neural Networks](https://arxiv.org/abs/2607.05017) | Develops transfer parameterizations for GNNs under SGD, Adam, and AdamW, yielding stable feature updates and learning-rate transfer across width and depth. | GNNs, transfer parameterization |
| 2026-06-28 | [On the Nonlinearity of Learning Rate Scaling for LLM Training](https://arxiv.org/abs/2606.29158) | Shows optimal learning-rate scaling for GPT-style LMs is nonlinear in model/data scale and that effective learning rate plus data-scale extrapolation gives cleaner transfer. | LR scaling, effective LR |
| 2026-06-16 | [On the Residual Scaling of Looped Transformers: Stability and Transferability](https://arxiv.org/abs/2606.18524) | Derives residual scaling for weight-tied looped Transformers so learning rates transfer across loop counts. | looped Transformers, residual scaling |
| 2026-06-16 | [Learning Rate Transfer and Feature Learning Across Depth for Constrained Spectral Optimizers: Complete Scion](https://openreview.net/forum?id=TGiJpGVPNA) | Extends CompleteP-style depth scaling to constrained spectral optimizers such as Scion, supporting width- and depth-wise learning-rate transfer. | Scion, depth transfer |
| 2026-06-15 | [Fantastic Pretraining Optimizers and Where to Find Them II: Hyperball Optimization](https://arxiv.org/abs/2606.16899) | Introduces Hyperball, an Adam/Muon wrapper fixing weight and update Frobenius norms, improving learning-rate transfer across width and depth. | Hyperball, Muon, LR transfer |
| 2026-06-11 | [LoRA-Muon: Spectral Steepest Descent on the Low-Rank Manifold](https://arxiv.org/abs/2606.12921) | Derives a spectral low-rank Muon update whose optimal learning rate transfers across LoRA rank, model width, depth, and factor rescaling. | LoRA, Muon, LR transfer |
| 2026-06-02 | [Unlocking Feature Learning in Gated Delta Networks at Scale](https://arxiv.org/abs/2606.04048) | Derives μP scaling rules for Gated Delta Networks and validates width learning-rate transfer under AdamW and SGD. | Gated Delta Networks, sequence models |
| 2026-05-30 | [Memory-Efficient LLM Training with Dynamic Sparsity: From Stability to Practical Scaling](https://arxiv.org/abs/2606.00888) | Derives density-aware learning-rate and effective-fan-in scaling from μP / SμPar to stabilize dynamic sparse LLM pretraining. | sparsity, density-aware LR, ICML 2026 |
| 2026-05-29 | [Why Routers Freeze: Infinite Width Learning Dynamics for Mixture of Experts](https://openreview.net/forum?id=AhwB471ARJ) | Uses Tensor Programs to show router saturation under standard parameterization and derives μP-MoE scaling for stable router dynamics. | MoE, routers, OpenReview |
| 2026-05-29 | [Fast Learning Rate Transfer for Gradient Descent in Sketched Linear Regression](https://openreview.net/forum?id=WMHm9bkItE) | Analyzes Yang et al.-style hyperparameter transfer in a solvable sketched-linear model and proves fast learning-rate transfer regimes. | LR transfer, sketched linear regression |
| 2026-05-23 | [Feature Learning in Wide Neural Networks under μP](https://arxiv.org/abs/2605.24710) | Studies identifiability, sparse-dictionary structure, and mean-field feature-learning limits for wide two-layer networks under μP. | mean field, identifiability |
| 2026-05-22 | [Complete-muE: Optimal Hyperparameter Transfer and Scaling for MoE Models](https://arxiv.org/abs/2605.23893) | Extends hyperparameter transfer across dense FFN and MoE settings via active-width μP and expert-capacity scaling. | MoE, Complete-muE |
| 2026-05-21 | [AMUSE: Anytime Muon with Stable Gradient Evaluation](https://arxiv.org/abs/2605.22432) | Combines Muon with schedule-free averaging to support anytime training without a predetermined token horizon. | adjacent optimizer, Muon, anytime training |
| 2026-05-20 | [Quantifying Hyperparameter Transfer and the Importance of Embedding Layer Learning Rate](https://arxiv.org/abs/2605.21486) | Defines transfer-quality metrics and argues that embedding learning-rate scaling explains much of μP's practical AdamW benefit in LMs. | embedding LR, AdamW |
| 2026-05-19 | [Toto 2.0: Time Series Forecasting Enters the Scaling Era](https://arxiv.org/abs/2605.20119) | Reports a time-series foundation-model scaling recipe using a u-μP hyperparameter-transfer pipeline from 4M to 2.5B parameters. | application report, u-μP |
| 2026-05-18 | [ScheduleFree+: Scaling Learning-Rate-Free & Schedule-Free Learning to Large Language Models](https://arxiv.org/abs/2605.19095) | Scales schedule-free training to longer-duration LLM runs and studies model averaging as an alternative to WSD and fixed-horizon schedules. | adjacent schedule, anytime training |
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
| 2026-02-10 | [Configuration-to-Performance Scaling Law with Neural Ansatz](https://arxiv.org/abs/2602.10300) | Learns a full-configuration-to-loss scaling law from open pretraining logs and supports joint hyperparameter tuning across model, data, optimizer, and schedule settings. | adjacent scaling law, joint tuning |
| 2026-02-07 | [Hyperparameter Transfer Laws for Non-Recurrent Multi-Path Neural Networks](https://arxiv.org/abs/2602.07494) | Introduces effective-depth scaling laws for multi-path architectures and learning-rate transfer across depth and width. | depth, multi-path |
| 2026-02-07 | [On the Infinite Width and Depth Limits of Predictive Coding Networks](https://arxiv.org/abs/2602.07697) | Shows predictive-coding networks share width- and depth-stable feature-learning parameterizations with backpropagation. | predictive coding, width-depth |
| 2026-02-06 | [Optimal Learning-Rate Schedules under Functional Scaling Laws: Power Decay and Warmup-Stable-Decay](https://arxiv.org/abs/2602.06797) | Derives horizon-dependent optimal schedules and identifies regimes where the optimum is power decay versus warmup-stable-decay. | WSD, token horizon, schedules |
| 2026-02-05 | [Learning Rate Scaling across LoRA Ranks and Transfer to Full Finetuning](https://arxiv.org/abs/2602.06204) | Proposes maximal-update-style rules for LoRA rank scaling and transfer from LoRA to full finetuning. | LoRA, finetuning |
| 2026-02-04 | [Theory of Optimal Learning Rate Schedules and Scaling Laws for a Random Feature Model](https://arxiv.org/abs/2602.04774) | Derives horizon-dependent optimal LR schedules, including power-decay and WSD-like regimes, and shows when base LR transfers across training horizons. | token horizon, optimal schedule, theory |
| 2026-02-03 | [Anytime Pretraining: Horizon-Free Learning-Rate Schedules with Weight Averaging](https://arxiv.org/abs/2602.03702) | Shows that constant or inverse-square-root LR with weight averaging can track tuned cosine performance across unknown training horizons. | horizon-free LR, weight averaging |
| 2026-01-31 | [Over-Alignment vs Over-Fitting: The Role of Feature Learning Strength in Generalization](https://arxiv.org/abs/2602.00827) | Studies how feature-learning strength affects generalization and identifies regimes where stronger feature learning over-aligns. | feature learning strength |
| 2026-01-28 | [Hyperparameter Transfer with Mixture-of-Expert Layers](https://arxiv.org/abs/2601.20205) | Proposes a MoE Transformer parameterization for transferring hyperparameters across width, depth, expert count, and expert size. | MoE, HPT |
| 2026-01-25 | [IMU-1: Sample-Efficient Pre-training of Small Language Models](https://arxiv.org/abs/2602.02522) | Documents a small-LM training recipe combining NorMuon, cautious weight decay, and μP parameterization. | application report, LLM |
| 2026-01-15 | [On the origin of neural scaling laws: from random graphs to natural language](https://arxiv.org/abs/2601.10684) | Studies scaling laws in simplified Transformer settings and reports preliminary evidence that maximal-update parameterization can be more parameter-efficient than standard parameterization. | scaling laws, application report |
| 2026-01-13 | [Universal Dynamics of Warmup Stable Decay: understanding WSD beyond Transformers](https://arxiv.org/abs/2601.09000) | Compares WSD dynamics and landscape geometry across language models and CNNs, identifying behavior shared beyond Transformers. | WSD, loss landscape |
| 2026-01-13 | [Controlled LLM Training on Spectral Sphere](https://arxiv.org/abs/2601.08393) | Proposes a spectral-sphere optimizer designed to align both weights and updates with μP-style scale control. | spectral constraints, optimizer |
| 2026-01-08 | [How to Set the Learning Rate for Large-Scale Pre-training?](https://arxiv.org/abs/2601.05049) | Compares fitted LR scaling laws with transfer, extending μTransfer to MoE, depth, weight decay, and token horizons while documenting large-scale limits. | μTransfer, MoE, token horizon |
| 2026-01-08 | [How to Set the Batch Size for Large-Scale Pre-training?](https://arxiv.org/abs/2601.05034) | Revises critical-batch theory for WSD and derives minimum and optimal batch-size behavior plus a dynamic batch-size schedule. | WSD, batch size, scaling law |
| 2026-01-08 | [Learnable Multipliers: Freeing the Scale of Language Model Matrix Layers](https://arxiv.org/abs/2601.04890) | Generalizes μP multipliers into learnable row/column scaling factors for language-model matrix layers. | multipliers, LLMs |
| 2026-01-04 | [Towards a Principled Muon under μP: Ensuring Spectral Conditions throughout Training](https://arxiv.org/abs/2601.01306) | Develops Muon++ to maintain μP spectral conditions throughout training without repeated weight normalization. | Muon, spectral conditions |
| 2025-12-31 | [Dynamic Large Concept Models: Latent Reasoning in an Adaptive Semantic Space](https://arxiv.org/abs/2512.24617) | Uses decoupled μP parameterization for zero-shot hyperparameter transfer across widths and compression regimes. | application report, decoupled μP |
| 2025-12-28 | [Understanding the Mechanisms of Fast Hyperparameter Transfer](https://arxiv.org/abs/2512.22768) | Formalizes fast hyperparameter transfer and studies when μP transfer is compute-efficient versus when it fails. | mechanism, fast transfer |
| 2025-12-26 | [Completed Hyperparameter Transfer across Modules, Width, Depth, Batch and Duration](https://arxiv.org/abs/2512.22382) | Extends CompleteP-style transfer to modules, width, depth, batch size, training duration, and per-module hyperparameters; the ICLR 2026 version appeared under the earlier title [Transfer Paramatters](https://openreview.net/forum?id=elB9k4nTL1). | CompleteP, batch, duration |
| 2025-12-24 | [Feature Learning Dynamics in Infinite-Depth Neural Networks](https://arxiv.org/abs/2512.21075) | Gives a rigorous feature-learning limit for one-layer ResNets under depth-μP and quantifies how forward-backward weight-reuse effects vanish with depth. | depth-μP, infinite depth, theory |
| 2025-12-20 | [Towards Guided Descent: Optimization Algorithms for Training Neural Networks At Scale](https://arxiv.org/abs/2512.18373) | Surveys practical large-scale optimization methods, including maximal-update parameterization as part of modern training workflows. | survey, optimization |
| 2025-12-05 | [Hyperparameter Transfer Enables Consistent Gains of Matrix-Preconditioned Optimizers Across Scales](https://arxiv.org/abs/2512.05620) | Studies μP-style hyperparameter transfer for Shampoo, SOAP, Muon, and related matrix-preconditioned optimizers. | preconditioners, optimizers |
| 2025-11-23 | [Xmodel-2.5: 1.3B Data-Efficient Reasoning SLM](https://arxiv.org/abs/2511.19496) | Uses μP to transfer hyperparameters from a 20M proxy to a 1.3B small language model. | application report, SLM |
| 2025-11-14 | [Training Neural Networks at Any Scale](https://arxiv.org/abs/2511.11163) | Reviews scale-agnostic optimization, μP, coordinate checks, and parameterization-aware rules for keeping training behavior stable across model scales. | survey, scale-agnostic optimization |
| 2025-11-07 | [Scaling depth capacity via zero/one-layer model expansion](https://arxiv.org/abs/2511.04981) | Studies depth expansion, initialization of new layers, hyperparameter transfer, and schedule timing for progressive large-model training. | depth expansion, initialization |
| 2025-11-03 | [A Proof of Learning Rate Transfer under μP](https://arxiv.org/abs/2511.01734) | Proves width learning-rate transfer for linear MLPs under μP and contrasts it with SP and NTP. | theory, LR transfer |
| 2025-10-21 | [Weight Decay may matter more than muP for Learning Rate Transfer in Practice](https://arxiv.org/abs/2510.19093) | Challenges the practical mechanism of μP transfer in LLM settings and argues weight decay often stabilizes representation updates after early training. | critique, weight decay |
| 2025-10-17 | [Robust Layerwise Scaling Rules by Proper Weight Decay Tuning](https://arxiv.org/abs/2510.15262) | Extends μP-style transfer into the AdamW steady state by coupling matrix learning-rate scaling with weight-decay scaling. | weight decay, AdamW |
| 2025-10-05 | [Arithmetic-Mean μP for Modern Architectures](https://arxiv.org/abs/2510.04327) | Replaces per-layer maximal-update constraints with an average update criterion for CNNs and ResNets, yielding width-robust depth laws. | AM-μP, CNNs, ResNets |
| 2025-10-04 | [Optimal Scaling Needs Optimal Norm](https://arxiv.org/abs/2510.03871) | Studies model/data-size hyperparameter transfer for Adam and Scion through an output-layer operator-norm invariant. | norm transfer, Adam, Scion |
| 2025-09-29 | [Scaling with Collapse: Efficient and Predictable Training of LLM Families](https://arxiv.org/abs/2509.25087) | Validates μP training-curve collapse at LLM scale when tokens per parameter, LR schedule, and AdamW timescale are scaled consistently. | μP application, TPP, AdamW |
| 2025-09-23 | [Functional Scaling Laws in Kernel Regression: Loss Dynamics and Learning Rate Schedules](https://arxiv.org/abs/2509.19189) | Models full loss trajectories under arbitrary LR schedules and derives explicit scaling relations for constant, decay, and WSD schedules. | functional scaling law, WSD |
| 2025-09-18 | [CompleteP for RL: Maintaining Feature Learning When Scaling Deep Reinforcement Learning](https://openreview.net/forum?id=3uXAGWqCny) | Applies CompleteP to non-stationary reinforcement learning and demonstrates learning-rate transfer plus feature and policy consistency across width and depth; accepted at ICML 2026, with the [earlier ICLR submission](https://openreview.net/forum?id=Wuy631kHwH) retained for version history. | CompleteP, reinforcement learning, ICML 2026 |
| 2025-09-02 | [Fantastic Pretraining Optimizers and Where to Find Them](https://arxiv.org/abs/2509.02046) | Benchmarks optimizer tuning across model scales and data-to-model ratios, showing why blind hyperparameter transfer and intermediate-budget comparisons can mislead. | adjacent optimizer study, token budget |
| 2025-09-01 | [LongCat-Flash Technical Report](https://arxiv.org/abs/2509.01322) | Reports a large MoE training framework combining hyperparameter transfer, model-growth initialization, stability controls, and deterministic training. | application report, MoE |
| 2025-08-13 | [μ-Parametrization for Mixture of Experts](https://arxiv.org/abs/2508.09752) | Derives μ-parameterization rules for MoE models and demonstrates learning-rate transfer across model sizes. | MoE, μTransfer |
| 2025-08-02 | [Training Dynamics of the Cooldown Stage in Warmup-Stable-Decay Learning Rate Scheduler](https://arxiv.org/abs/2508.01483) | Analyzes cooldown shape, AdamW beta2, and the bias-variance trade-off that controls the final WSD loss drop. | WSD, cooldown, AdamW |
| 2025-07-30 | [Falcon-H1: A Family of Hybrid-Head Language Models Redefining Efficiency and Performance](https://arxiv.org/abs/2507.22448) | Reports a 0.5B–34B hybrid attention / SSM language-model family trained with an architecture-specific μP recipe for smooth cross-scale training. | application report, hybrid LLM, μP |
| 2025-07-23 | [WSM: Decay-Free Learning Rate Schedule via Checkpoint Merging for LLM Pre-training](https://arxiv.org/abs/2507.17634) | Connects LR decay to checkpoint averaging and proposes Warmup-Stable-and-Merge as a horizon-flexible alternative to WSD. | checkpoint merging, horizon-free schedule |
| 2025-07-11 | [Pre-Training LLMs on a budget: A comparison of three optimizers](https://arxiv.org/abs/2507.08472) | Uses maximal update parametrization and proxy models to tune AdamW, Lion, and Sophia for budget LLM pretraining. | optimizer comparison, LLM |
| 2025-07-02 | [Scaling Collapse Reveals Universal Dynamics in Compute-Optimally Trained Neural Networks](https://arxiv.org/abs/2507.02119) | Studies universal training dynamics and when suboptimal hyperparameter scaling breaks compute-optimal collapse. | scaling laws, dynamics |
| 2025-06-24 | [Maximal Update Parametrization and Zero-Shot Hyperparameter Transfer for Fourier Neural Operators](https://arxiv.org/abs/2506.19396) | Derives μTransfer-FNO for scaling Fourier Neural Operators by Fourier modes. | neural operators, PDE |
| 2025-06-17 | [Optimal Embedding Learning Rate in LLMs: The Effect of Vocabulary Size](https://arxiv.org/abs/2506.15025) | Derives how the embedding learning rate should scale with vocabulary size, complementing recent μP transfer caveats for language models. | embedding LR, vocabulary |
| 2025-06-09 | [MiniCPM4: Ultra-Efficient LLMs on End Devices](https://arxiv.org/abs/2506.07900) | Uses μP-backed small-model hyperparameter searches in the ModelTunnel / ScalingBench pipeline to validate transfer before training end-device LLMs. | application report, SLM, model wind tunnel |
| 2025-06-08 | [A Stable Whitening Optimizer for Efficient Neural Network Training](https://arxiv.org/abs/2506.07254) | Introduces SPlus and uses shape-aware scaling to enable learning-rate transfer across network width. | whitening optimizer, width transfer |
| 2025-05-28 | [On the Surprising Effectiveness of Large Learning Rates under Standard Width Scaling](https://arxiv.org/abs/2505.22491) | Analyzes feature learning under standard parameterization and explains large-learning-rate behavior beyond classical infinite-width predictions. | SP, feature learning |
| 2025-05-26 | [Variational Deep Learning via Implicit Regularization](https://arxiv.org/abs/2505.20235) | Extends maximal-update parameterization to variational neural networks through covariance initialization and learning-rate scalings that preserve width transfer. | variational inference, μP extension |
| 2025-05-21 | [Scaling Diffusion Transformers Efficiently via μP](https://arxiv.org/abs/2505.15270) | Generalizes μP to diffusion Transformer families such as DiT, U-ViT, PixArt-α, and MMDiT. | diffusion, DiT |
| 2025-05-19 | [Power Lines: Scaling Laws for Weight Decay and Batch Size in LLM Pre-training](https://arxiv.org/abs/2505.13738) | Derives scaling laws for weight decay and batch size as model size, data size, and batch size change. | weight decay, batch size |
| 2025-05-19 | [Gluon: Making Muon & Scion Great Again!](https://arxiv.org/abs/2505.13416) | Studies LMO-based optimizers such as Muon and Scion, emphasizing hyperparameter transferability and practical LLM training behavior. | Muon, Scion |
| 2025-05-19 | [μPC: Scaling Predictive Coding to 100+ Layer Networks](https://arxiv.org/abs/2505.13124) | Uses Depth-μP-style parameterization to train very deep predictive-coding networks. | predictive coding, Depth-μP |
| 2025-05-04 | [Practical Efficiency of Muon for Pretraining](https://arxiv.org/abs/2505.02222) | Studies Muon plus maximal update parameterization and gives a telescoping algorithm for efficient μP hyperparameter transfer. | Muon, μP |
| 2025-05-02 | [Don't be lazy: CompleteP enables compute-efficient deep transformers](https://arxiv.org/abs/2505.01618) | Proposes CompleteP for depth-wise hyperparameter transfer while avoiding lazy learning in deep Transformers. | CompleteP, depth, Transformers |
| 2025-05-01 | [On the Provable Separation of Scales in Maximal Update Parameterization](https://openreview.net/forum?id=csB1njlpjM) | Provides theory for why μP can separate macro-variables from micro-variables, supporting small-scale hyperparameter tuning. | theory, scale separation |
| 2025-04-07 | [Dion: Distributed Orthonormalized Updates](https://arxiv.org/abs/2504.05295) | Scales orthonormalized updates for sharded LLM training while preserving robust hyperparameter transfer. | optimizer, orthonormal updates |
| 2025-03-12 | [Global Convergence and Rich Feature Learning in L-Layer Infinite-Width Neural Networks under μP Parametrization](https://arxiv.org/abs/2503.09565) | Proves global convergence while preserving rich feature learning for L-layer infinite-width networks trained with SGD under μP. | theory, global convergence |
| 2025-03-06 | [Predictable Scale: Part I, Step Law -- Optimal Hyperparameter Scaling Law in Large Language Model Pretraining](https://arxiv.org/abs/2503.04715) | Fits joint power laws for optimal learning rate over model and data size and for optimal batch size over data size. | adjacent scaling law, LR, batch size |
| 2025-02-24 | [Function-Space Learning Rates](https://arxiv.org/abs/2502.17405) | Introduces FLeRM, a function-space learning-rate matching method for hyperparameter transfer across widths, depths, and LoRA ranks. | function-space LR, FLeRM |
| 2025-02-21 | [Straight to Zero: Why Linearly Decaying the Learning Rate to Zero Works Best for LLMs](https://arxiv.org/abs/2502.15938) | Uses μP proxy-to-target transfer to isolate how schedule shape, tokens per parameter, and weight decay affect LLM pretraining. | adjacent schedule, linear decay, μP application |
| 2025-02-12 | [Distillation Scaling Laws](https://arxiv.org/abs/2502.08606) | Uses a simplified μP parameterization to keep teacher and student hyperparameters comparable across a large distillation scaling study. | application report, distillation, scaling laws |
| 2025-02-09 | [μnit Scaling: Simple and Scalable FP8 LLM Training](https://arxiv.org/abs/2502.05967) | Proposes μnit Scaling for FP8 LLM training with simple width-wise hyperparameter transfer and matched training/inference numerics. | FP8, u-μP |
| 2025-02-07 | [Gemstones: A Model Suite for Multi-Faceted Scaling Laws](https://arxiv.org/abs/2502.06857) | Releases more than 4,000 checkpoints and studies joint width-depth learning-rate transfer plus scalable initialization across model shapes. | width-depth transfer, scaling-law dataset |
| 2025-02-04 | [Deep Linear Network Training Dynamics from Random Initialization: Data, Width, Depth, and Hyperparameter Transfer](https://arxiv.org/abs/2502.02531) | Analyzes deep linear networks and captures maximum-update feature learning plus width/depth hyperparameter transfer effects. | deep linear networks, theory |
| 2025-01-23 | [A thorough reproduction and evaluation of μP](https://openreview.net/forum?id=AFxEdJwQcp) | TMLR reproduction study evaluating claimed μP benefits, implementation sensitivity, and transfer behavior. | reproduction, TMLR |
| 2024-12-02 | [Scaling Law for Language Models Training Considering Batch Size](https://arxiv.org/abs/2412.01505) | Fits batch-size scaling laws under fixed-compute and fixed-data regimes and validates extrapolation across model scales. | adjacent scaling law, batch size |
| 2024-11-11 | [Warmstarting for Scaling Language Models](https://arxiv.org/abs/2411.07340) | Studies μTransfer-compatible warmstarting from smaller language models via shrink, zero-padding, and μP-scaled perturbations. | warmstart, LLMs |
| 2024-11-06 | [Crystal: Illuminating LLM Abilities on Language and Code](https://arxiv.org/abs/2411.04156) | Transfers a layer-wise μP recipe from smaller and shallower proxies to language-and-code pretraining targets; the earlier [LLM360 report](https://arxiv.org/abs/2312.06550) provides project history. | application report, code LLM, μTransfer |
| 2024-11-04 | [Local Loss Optimization in the Infinite Width](https://arxiv.org/abs/2411.02001) | Introduces μP-style stable parameterizations for predictive coding and target propagation under local-loss training. | local learning, predictive coding |
| 2024-10-31 | [μP²: Effective Sharpness Aware Minimization Requires Layerwise Perturbation Scaling](https://arxiv.org/abs/2411.00075) | Extends maximal-update ideas to SAM by scaling layerwise perturbations so learning rate and perturbation radius transfer jointly. | SAM, perturbation scaling |
| 2024-10-29 | [How Does Critical Batch Size Scale in Pre-training?](https://arxiv.org/abs/2410.21676) | Finds that critical batch size scales primarily with data rather than model width, using μP / infinite-width invariance as the cross-scale baseline. | batch size, data scaling, μP baseline |
| 2024-10-28 | [Modular Duality in Deep Learning](https://arxiv.org/abs/2410.21265) | Unifies μP- and Shampoo-style operator-norm scaling and derives width-invariant update and learning-rate rules for scalable optimizers. | modular duality, operator norms, optimizer scaling |
| 2024-10-08 | [Time Transfer: On Optimal Learning Rate and Batch Size In The Infinite Data Limit](https://arxiv.org/abs/2410.05838) | Shows that optimal LR and critical batch size evolve with pretraining token budget even under μP, with the measured critical batch size growing proportionally to tokens. | μP, token horizon, batch size |
| 2024-10-07 | [Understanding Warmup-Stable-Decay Learning Rates: A River Valley Loss Landscape Perspective](https://arxiv.org/abs/2410.05192) | Explains WSD's stable and decay phases, and proposes WSD-S for producing checkpoints at multiple compute budgets from one main training branch. | WSD, compute budget, loss landscape |
| 2024-10-06 | [The Optimization Landscape of SGD Across the Feature Learning Strength](https://arxiv.org/abs/2410.04642) | Studies how final-layer scaling controls feature-learning strength and changes optimal learning-rate regimes. | feature learning strength |
| 2024-09-30 | [Scaling Optimal LR Across Token Horizons](https://arxiv.org/abs/2409.19913) | Studies how optimal learning rate scales with token horizon and how to transfer LR across training durations. | token horizon, LR transfer |
| 2024-09-25 | [On Feature Learning in Structured State Space Models](https://openreview.net/forum?id=aQv5AbN1wF) | Shows that standard μP and spectral scaling conditions do not directly guarantee feature learning for structured state-space models such as Mamba. | SSM, Mamba, OpenReview |
| 2024-08-29 | [Large-Scale Multi-omic Biosequence Transformers for Modeling Protein-Nucleic Acid Interactions](https://arxiv.org/abs/2408.16245) | Tunes a smaller proxy and applies μP learning-rate scaling to train OmniBioTE, a 250B-token multi-omic Transformer. | application report, biosequence, μP |
| 2024-08-23 | [Power Scheduler: A Batch Size and Token Number Agnostic Learning Rate Scheduler](https://arxiv.org/abs/2408.13359) | Studies learning-rate transfer across batch size and token count; combines with μP for broader hyperparameter portability. | scheduler, tokens, batch |
| 2024-07-31 | [TASI Lectures on Physics for Machine Learning](https://arxiv.org/abs/2408.00082) | Lecture notes covering neural-network theory, feature learning, and maximal update parameterization from a physics perspective. | lecture notes, theory |
| 2024-07-24 | [u-μP: The Unit-Scaled Maximal Update Parametrization](https://arxiv.org/abs/2407.17465) | Combines μP with Unit Scaling; aims for simpler defaults and low-precision / FP8-friendly training. | unit scaling, FP8 |
| 2024-07-08 | [Scaling Exponents Across Parameterizations and Optimizers](https://arxiv.org/abs/2407.05872) | Large empirical/theoretical study of learning-rate scaling across optimizers and parameterizations; argues that transfer can occur beyond classical μP and highlights Adam epsilon scaling. | scaling exponents, optimizers |
| 2024-06-13 | [Why Warmup the Learning Rate? Underlying Mechanisms and Improvements](https://arxiv.org/abs/2406.09405) | Compares warmup, sharpness, and stable learning-rate ranges under standard and μP parameterizations, including a simple Adam μP setting. | adjacent schedule, warmup, sharpness |
| 2024-06-12 | [The Impact of Initialization on LoRA Finetuning Dynamics](https://arxiv.org/abs/2406.08447) | Uses μP asymptotics to show how LoRA initialization direction changes the maximal stable learning-rate scaling with model width. | LoRA, initialization, LR scaling |
| 2024-06-10 | [Compute Better Spent: Replacing Dense Layers with Structured Matrices](https://arxiv.org/abs/2406.06248) | Uses maximal-update insights to set initialization and learning-rate scaling for structured matrix layers. | structured matrices, initialization |
| 2024-05-31 | [μLO: Compute-Efficient Meta-Generalization of Learned Optimizers](https://arxiv.org/abs/2406.00153) | Derives μP for learned optimizer architectures and improves generalization to wider, deeper, and longer-horizon tasks. | learned optimizers, μLO |
| 2024-05-24 | [Infinite Limits of Multi-head Transformer Dynamics](https://arxiv.org/abs/2405.15712) | Derives width, depth, head, and attention-scaling limits for feature-learning Transformer dynamics. | Transformers, DMFT |
| 2024-05-24 | [Sparse maximal update parameterization: A holistic approach to sparse training dynamics](https://arxiv.org/abs/2405.15743) | SμPar extends maximal-update ideas to sparse neural networks and transfers hyperparameters across width and sparsity. | sparsity, SμPar |
| 2024-05-23 | [Surge Phenomenon in Optimal Learning Rate and Batch Size Scaling](https://arxiv.org/abs/2405.14578) | Derives and validates the non-monotonic relationship between optimal learning rate and batch size for Adam-style optimizers. | adjacent optimizer scaling, batch size |
| 2024-05-22 | [How to set AdamW's weight decay as you scale model and dataset size](https://arxiv.org/abs/2405.13698) | Connects AdamW weight decay to EMA timescale and derives how the hyperparameter should scale with model and data size. | AdamW, weight decay |
| 2024-04-30 | [The lazy (NTK) and rich (μP) regimes: a gentle tutorial](https://arxiv.org/abs/2404.19719) | Tutorial explaining the richness scale between lazy NTK training and active μP feature learning. | tutorial, feature learning |
| 2024-04-09 | [MiniCPM: Unveiling the Potential of Small Language Models with Scalable Training Strategies](https://arxiv.org/abs/2404.06395) | Reports model wind-tunnel experiments and scalable training strategies for small language models. | application report, SLM |
| 2024-04-08 | [An Empirical Study of μP Learning Rate Transfer](https://arxiv.org/abs/2404.05728) | Empirically tests whether μTransfer gives near-optimal learning-rate transfer for Transformer architectures. | empirical, LR transfer |
| 2024-02-27 | [Super Consistency of Neural Network Landscapes and Learning Rate Transfer](https://arxiv.org/abs/2402.17457) | Studies why learning-rate transfer can occur by analyzing Hessian sharpness and landscape consistency under μP-like feature-learning limits. | sharpness, landscape |
| 2024-02-27 | [Principled Architecture-aware Scaling of Hyperparameters](https://arxiv.org/abs/2402.17440) | Derives architecture-aware initialization and maximal-learning-rate scaling rules from equalized preactivation updates. | architecture-aware HPT |
| 2024-02-19 | [LoRA+: Efficient Low Rank Adaptation of Large Models](https://arxiv.org/abs/2402.12354) | Uses infinite-width / μP scaling to derive different learning rates for LoRA's two adapter matrices, preserving efficient feature learning as width grows. | LoRA, finetuning, LR scaling |
| 2024-01-05 | [DeepSeek LLM: Scaling Open-Source Language Models with Longtermism](https://arxiv.org/abs/2401.02954) | Reports fitted optimal learning-rate and batch-size behavior across model, data, and compute scales as an adjacent large-scale empirical reference. | application report, adjacent scaling law |
| 2023-12-19 | [On the Parameterization of Second-Order Optimization Effective Towards the Infinite Width](https://arxiv.org/abs/2312.12226) | Derives μP-like feature-learning parameterizations for second-order optimizers such as K-FAC and Shampoo. | second-order, K-FAC, Shampoo |
| 2023-12-10 | [Feature-Learning Networks Are Consistent Across Widths At Realistic Scales](https://proceedings.neurips.cc/paper_files/paper/2023/hash/03600ae6c3392fd65ad7c3a90c6f7ce8-Abstract-Conference.html) | Shows finite-width feature-learning networks can remain behaviorally consistent across realistic widths, supporting μP-style scaling. | finite width, NeurIPS |
| 2023-10-26 | [A Spectral Condition for Feature Learning](https://arxiv.org/abs/2310.17813) | Shows how spectral-norm scaling yields feature learning and gives an elementary derivation of maximal update parametrization. | spectral condition, theory |
| 2023-10-03 | [Tensor Programs VI: Feature Learning in Infinite-Depth Neural Networks](https://arxiv.org/abs/2310.02244) | Studies depthwise parameterizations; proposes Depth-μP for single-layer residual blocks and discusses limitations for deeper blocks. | Depth-μP, ResNets, Transformers |
| 2023-09-28 | [Depthwise Hyperparameter Transfer in Residual Networks: Dynamics and Scaling Limit](https://arxiv.org/abs/2309.16620) | Combines μP with residual branch scaling to transfer hyperparameters across width and depth. | depth transfer, ResNets, ViTs |
| 2023-09-20 | [BTLM-3B-8K: 7B Parameter Performance in a 3B Parameter Model](https://arxiv.org/abs/2309.11568) | Tunes learning rate, initialization, embedding, and output multipliers on a 40M μP proxy before transferring them to a 3B, 627B-token language model. | application report, LLM, μTransfer |
| 2023-08-30 | [Jais and Jais-chat: Arabic-Centric Foundation and Instruction-Tuned Open Generative Large Language Models](https://arxiv.org/abs/2308.16149) | Tunes batch size, learning rate, initialization, and embedding / logit multipliers on a 40M μTransfer proxy before scaling to 13B. | application report, Arabic LLM, μTransfer |
| 2023-08-03 | [Tensor Programs IVb: Adaptive Optimization in the Infinite-Width Limit](https://arxiv.org/abs/2308.01814) | Generalizes analysis beyond SGD to adaptive optimizers. | Adam, adaptive optimization |
| 2023-05-13 | [Depth Dependence of μP Learning Rates in ReLU MLPs](https://arxiv.org/abs/2305.07810) | Studies how maximal-update learning rates depend on depth under mean-field initialization. | depth, learning rate |
| 2023-04-14 | [nanoLM: an Affordable LLM Pre-training Benchmark via Accurate Loss Prediction across Scales](https://arxiv.org/abs/2304.06875) | Introduces μScaling, using μP to predict large-model pretraining loss from smaller counterparts. | μScaling, loss prediction |
| 2023-04-06 | [Cerebras-GPT: Open Compute-Optimal Language Models Trained on the Cerebras Wafer-Scale Cluster](https://arxiv.org/abs/2304.03208) | Open LLM family and engineering report discussing μP as a way to improve scaling and pretraining efficiency. | application report, LLM |
| 2022-10-10 | [Meta-Principled Family of Hyperparameter Scaling Strategies](https://arxiv.org/abs/2210.04909) | Derives a one-parameter family interpolating between NTK and mean-field / maximal-update hyperparameter scaling. | scaling strategies, theory |
| 2022-05-26 | [A Framework for Overparameterized Learning](https://arxiv.org/abs/2205.13507) | Proves learning-rate transfer across widths in an overparameterized lazy-training framework, providing adjacent theory for width-scaling transfer. | overparameterization, LR transfer |
| 2022-03-07 | [Tensor Programs V: Tuning Large Neural Networks via Zero-Shot Hyperparameter Transfer](https://arxiv.org/abs/2203.03466) | Introduces μTransfer; demonstrates transfer on Transformer and ResNet settings. | μP, μTransfer, LLMs |
| 2021-10-29 | [Training Integrable Parameterizations of Deep Neural Networks in the Infinite-Width Limit](https://arxiv.org/abs/2110.15596) | Studies mean-field integrable parameterizations and shows one training method is equivalent to a modification of μP. | mean field, integrable parameterizations |
| 2020-11-30 | [Feature Learning in Infinite-Width Neural Networks](https://arxiv.org/abs/2011.14522) | Shows how non-kernel, feature-learning infinite-width limits can be obtained with Tensor Programs. | Tensor Programs, feature learning |
| 2020-09-22 | [Tensor Programs III: Neural Matrix Laws](https://arxiv.org/abs/2009.10685) | Establishes neural matrix laws and a Tensor Programs master theorem used to analyze dependencies between weights and activations at infinite width. | Tensor Programs, foundation |
| 2020-06-25 | [Tensor Programs II: Neural Tangent Kernel for Any Architecture](https://arxiv.org/abs/2006.14548) | Uses Tensor Programs to derive deterministic infinite-width neural tangent kernels for general architectures. | Tensor Programs, NTK foundation |
| 2019-10-28 | [Tensor Programs I: Wide Feedforward or Recurrent Neural Networks of Any Architecture are Gaussian Processes](https://arxiv.org/abs/1910.12478) | Introduces the general Tensor Programs language and Gaussian-process limits that underpin the later μP series. | Tensor Programs, NNGP foundation |

## Learning Resources and Blogs

| Resource | Type | Notes |
|---|---|---|
| [The Practitioner's Guide to the Maximal Update Parameterization](https://www.cerebras.ai/blog/the-practitioners-guide-to-the-maximal-update-parameterization) | Guide / blog | Practical implementation guide linked by `EleutherAI/nanoGPT-mup`; useful for coordinate checks and small GPT experiments. |
| [Train an LLM using Maximal Update Parameterization](https://training-api.cerebras.ai/en/latest/wsc/Model-zoo/tutorials/mup/mup_docs.html) | Documentation | End-to-end Cerebras guide to proxy-to-target μTransfer, base dimensions, initialization scaling, layer-wise learning-rate groups, supported models, and validation practice. |
| [On infinitely wide neural networks that exhibit feature learning](https://www.microsoft.com/en-us/research/blog/on-infinitely-wide-neural-networks-that-exhibit-feature-learning/) | Research blog | Microsoft Research introduction to feature learning at infinite width and the original maximal-update construction. |
| [μTransfer: A technique for hyperparameter tuning of enormous neural networks](https://www.microsoft.com/en-us/research/blog/%C2%B5transfer-a-technique-for-hyperparameter-tuning-of-enormous-neural-networks/) | Blog | Microsoft Research explainer for μTransfer and the Tensor Programs V workflow. |
| [Greg Yang's Tensor Programs and μP reading guide](https://thegregyang.com/) | Author guide | First-author map of Tensor Programs IV–VI, μP / μTransfer papers and code, and a recommended entry point into the theory. |
| [Infinite Limits of Neural Networks](https://kempnerinstitute.harvard.edu/research/deeper-learning/infinite-limits-of-neural-networks/) | Institutional guide | Kempner Institute comparison of NTK and μP feature-learning limits, realistic-width convergence, and hyperparameter transfer across width and depth. |
| [How Does Critical Batch Size Scale in Pre-training?](https://kempnerinstitute.harvard.edu/research/deeper-learning/how-does-critical-batch-size-scale-in-pre-training-decoupling-data-and-model-size/) | Research blog | Kempner Institute explanation of why critical batch size scales with data rather than model width under the study's μP-controlled setup. |
| [Infinite Widths (& Depths) Part III: The Maximal Update Parameterisation](https://francesco-innocenti.github.io/posts/2025/04/09/Infinite-Widths-%26-Depths-Part-III-The-Maximal-Update-Parameterisation/) | Researcher guide | Compact guide connecting width μP, Depth-μP, feature learning, and the main extensions. |
| [How To Scale](https://howtoscalenn.github.io/) | Independent technical guide | Broad practitioner synthesis of μP derivation, abc-parameterization, width/depth/data/horizon limits, learning-rate and batch scaling, and Muon's relation to μP; the author explicitly flags possible inaccuracies. |
| [Completed Hyperparameter Transfer across Modules, Width, Depth, Batch and Duration](https://machinelearning.apple.com/research/completed-hyperparameter) | Research page | Apple research page for Complete(d)P and transfer across width, depth, batch size, and duration. |
| [Predictable Scale: Part I](https://step-law.github.io/) | Project / tool | Interactive project page for Step Law, including fitted optimal LR and batch-size scaling across model and data scales. |
| [Anytime Pretraining: Horizon-Free Learning-Rate Schedules with Weight Averaging](https://kempnerinstitute.harvard.edu/research/deeper-learning/anytime-pretraining-horizon-free-learning-rate-schedules-with-weight-averaging/) | Research blog | Kempner Institute explanation of cosine envelopes, horizon-free schedules, weight averaging, and the WSD comparison. |
| [Rethinking Maximal Update Parametrization: Steepest Descent on the Spectral Ball](https://leloykun.github.io/ponder/rethinking-mup-spectral-ball/) | Technical essay | Geometric reinterpretation of maximal updates with spectral-ball constraints and learning-rate-transfer experiments. |
| [Rethinking Maximal Update Parametrization: Steepest Descent on Finsler-Structured Geometries](https://leloykun.github.io/ponder/steepest-descent-finsler-dual-ascent/) | Technical essay | Detailed derivation of maximal-update-aware steepest descent through Finsler geometry and dual ascent. |
| [Lecture Notes on Infinite-Width Limits of Neural Networks](https://mlschool.princeton.edu/sites/g/files/toruqf5946/files/documents/Princeton___Lecture_Notes_0.pdf) | Lecture notes | Pedagogical derivation of infinite-width limits and width-only μP for MLPs. |
| [How Width Scaling Affects Neural Networks: Generalization, Optimal Hyperparameters, Feature Learning and Beyond](https://doi.org/10.15496/publikation-114058) | Dissertation | Moritz Haas's systematic treatment of width scaling, hyperparameter transfer, feature learning, and μP / μP². |
| [Scaling and Renormalization in Statistical Learning](https://dash.harvard.edu/entities/publication/7df32c32-79fb-435b-b0c0-71e134dee5e8) | Dissertation | Alexander Atanasov's treatment of scaling laws, infinite-width feature learning, μP, and the approach of realistic networks to their limiting dynamics. |
| [A Simple Guide to Maximal Update Parameterization](https://blog.speechmatics.com/mup) | Blog | Practitioner-oriented explanation of μP motivation, scaling intuition, and implementation details. |
| [francesco-innocenti/mup-papers](https://github.com/francesco-innocenti/mup-papers) | Curated list | Active community list of mean-field / maximal-update parameterisation papers, grouped by theory and extensions. |
| [unit-scaling documentation](https://graphcore-research.github.io/unit-scaling) | Documentation | Docs and examples for the PyTorch unit-scaling library used by u-μP. |
| [Tensor Programs V / μTransfer talk](https://www.youtube.com/watch?v=z8-C42mAwBc) | Talk / video | Author talk linked from the official `microsoft/mup` repository, covering the transfer recipe, large-scale results, and theoretical motivation. |
| [Renormalizing the optimal hyperparameters of a neural network](https://indico.cern.ch/event/1065458/contributions/4647081/) | Talk / slides | Greg Yang's concise μTransfer talk with a recording, 25-page slide deck, and effective-field-theory analogy. |
| [Timothy Nguyen conversation on μP and Tensor Programs](https://www.youtube.com/watch?v=1aXOXHA7Jcw&t=2723s&ab_channel=TimothyNguyen) | Video | Long-form discussion touching Tensor Programs, μP, and scaling limits. |
| [AutoML Seminar: scaling exponents across parameterisations](https://www.youtube.com/watch?v=CnAfD7aVzLg&ab_channel=AutoMLSeminars) | Talk | Seminar companion for scaling-exponent work across parameterizations and optimizers. |

## Code and Implementations

| Resource | Related method | Notes |
|---|---|---|
| [microsoft/mup](https://github.com/microsoft/mup) | μP / μTransfer | Reference PyTorch package with base-shape tooling, model and optimizer scaling, examples, and coordinate-check utilities. |
| [microsoft/mutransformers](https://github.com/microsoft/mutransformers) | μP Transformers | Historical Microsoft demonstration for Hugging Face Transformer models; archived in June 2026 and based on Transformers 4.16.2. |
| [Cerebras/modelzoo](https://github.com/Cerebras/modelzoo) | Cerebras μP / μTransfer | Active training stack with μP / µParam scaling and configurations for GPT-style and other supported model families. |
| [NVIDIA Megatron Core μP configuration](https://docs.nvidia.com/megatron-core/developer-guide/latest/apidocs/core/core.transformer.transformer_config.html) | Production LLM μP | Native `use_mup` support with base-width, embedding, output, and attention scaling in [Megatron-LM](https://github.com/NVIDIA/Megatron-LM). |
| [zanussbaum/mup-tf](https://github.com/zanussbaum/mup-tf) | TensorFlow μP | Community TensorFlow implementation of maximal update parameterization. |
| [EleutherAI/nanoGPT-mup](https://github.com/EleutherAI/nanoGPT-mup) | μP for GPTs | Compact GPT-style implementation with marked μP changes, examples, and coordinate-check scripts. |
| [EleutherAI/nanoGPT-mup/tree/supar](https://github.com/EleutherAI/nanoGPT-mup/tree/supar) | SμPar | Minimal implementation for sparse maximal update parameterization. |
| [EleutherAI/nanoGPT-mup/tree/completep](https://github.com/EleutherAI/nanoGPT-mup/tree/completep) | CompleteP | Minimal implementation for CompleteP depth-wise transfer experiments. |
| [graphcore-research/unit-scaling](https://github.com/graphcore-research/unit-scaling) | u-μP | PyTorch library for Unit-Scaled Maximal Update Parameterization. |
| [lucaslingle/mu_transformer](https://github.com/lucaslingle/mu_transformer) | Large-scale μTransfer | Official JAX / Flax Transformer implementation with TPU/FSDP training, coordinate checks, and proxy-to-target configurations. |
| [NanChanNN/muA-lora](https://github.com/NanChanNN/muA-lora) | μA / LoRA transfer | Official code for learning-rate scaling across LoRA ranks and transfer to full finetuning over language, vision, VLM, RLVR, and diffusion tasks. |
| [lauyikfung/gated_delta_net_mup](https://github.com/lauyikfung/gated_delta_net_mup) | Gated Delta Network μP | Official implementation with AdamW / SGD width-transfer configurations and FineWeb-Edu pretraining scripts. |
| [vankadara-lab/mssp-moe](https://github.com/vankadara-lab/mssp-moe) | MSSP / MoE | Official code with coordinate checks and sweeps across width, depth, expert count, expert width, and routing regimes. |
| [ruihanxx/MOGA](https://github.com/ruihanxx/MOGA) | Operator-norm transfer | Official PyTorch implementation with GPT-2 / OpenWebText and LLaMA / C4 width-scaling and learning-rate-transfer experiments. |
| [NUS-HPC-AI-Lab/OrScale](https://github.com/NUS-HPC-AI-Lab/OrScale) | OrScale / Muon | Official optimizer package with tests, deterministic configurations, and vision plus 125M–1.1B language-model scaling experiments. |
| [charliezchen/scaling-matrix-preconditioning](https://github.com/charliezchen/scaling-matrix-preconditioning) | Matrix preconditioners | Official JAX / Optax code for μP-aware Shampoo, SOAP, and Muon transfer across width and depth. |
| [shikaiqiu/compute-better-spent](https://github.com/shikaiqiu/compute-better-spent) | Structured matrices | Official code for μP-derived initialization and learning-rate scaling of structured matrix layers across MLP, ViT, and GPT experiments. |
| [VITA-Group/principled_scaling_lr_init](https://github.com/VITA-Group/principled_scaling_lr_init) | Architecture-aware HPT | Official code for architecture-aware initialization and maximal-learning-rate scaling across MLPs, CNNs, and varied computation graphs. |
| [shikaiqiu/supercollapse](https://github.com/shikaiqiu/supercollapse) | Scaling collapse | Official JAX experiments, logs, and notebooks for reproducing scaling collapse and its breakdown under suboptimal hyperparameter scaling. |
| [bentherien/mu_learned_optimization](https://github.com/bentherien/mu_learned_optimization) | μLO | Official research code for μP learned-optimizer meta-training and cross-width / depth evaluation on Transformer and MLP tasks. |
| [edwardmilsom/function-space-learning-rates-paper](https://github.com/edwardmilsom/function-space-learning-rates-paper) | FLeRM | Official implementation with reusable code and reproduction experiments for ResMLPs, Transformers, and LoRA rank transfer. |
| [tiiuae/Falcon-H1](https://github.com/tiiuae/Falcon-H1) | Falcon-H1 application | Official project hub documenting the hybrid architecture, customized μP scaling recipe, evaluation, deployment, and finetuning resources. |
| [tiiuae/Falcon-H1 checkpoints](https://huggingface.co/collections/tiiuae/falcon-h1) | Falcon-H1 application | Official 0.5B–34B base, instruction-tuned, and quantized checkpoint collection. |
| [cerebras/btlm-3b-8k-base](https://huggingface.co/cerebras/btlm-3b-8k-base) | BTLM μP application | Released μP-trained 3B checkpoint with a PyTorch `get_mup_param_groups` continuation example and explicit μP configuration fields. |
| [QiaoXiao7282/SMET](https://github.com/QiaoXiao7282/SMET) | Dynamic sparsity / SMET | Official code and configurations for density-aware learning-rate scaling and stable dynamic sparse LLM training. |
| [InceptionAI Jais models](https://huggingface.co/inceptionai/jais-13b) | Jais μTransfer application | Official Jais 13B foundation weights and accompanying [Jais-chat checkpoint](https://huggingface.co/inceptionai/jais-13b-chat). |
| [LLM360/crystalcoder-train](https://github.com/LLM360/crystalcoder-train) | CrystalCoder μP application | Official training code with released [intermediate checkpoints](https://huggingface.co/LLM360/CrystalCoder) and [pretraining data](https://huggingface.co/datasets/LLM360/CrystalCoderDatasets). |
| [nyuolab/OmniBioTE](https://github.com/nyuolab/OmniBioTE) | Biosequence μP application | Official code for multi-omic μP scaling with released [OmniBioTE weights](https://huggingface.co/WeiHua/OmniBioTE/tree/main). |
| [OpenBMB/MiniCPM](https://github.com/OpenBMB/MiniCPM) | MiniCPM4 application | Official code and [MiniCPM4 model collection](https://huggingface.co/collections/openbmb/minicpm4) for the μP-backed ModelTunnel scaling pipeline. |
| [nikhil-ghosh-berkeley/loraplus](https://github.com/nikhil-ghosh-berkeley/loraplus) | LoRA+ / LoRA initialization | Author implementation shared by the LoRA+ and LoRA-initialization scaling papers. |
| [mcleish7/gemstone-scaling-laws](https://github.com/mcleish7/gemstone-scaling-laws) | Gemstones | Official scaling-law code with a [project explorer](https://mcleish7.github.io/gemstone-scaling-laws/) and [4,000+ model checkpoints](https://huggingface.co/collections/tomg-group-umd/gemstone-models-679408ee3f19f1d4d00e8b10). |
| [hlzhang109/critical-batch-size](https://github.com/hlzhang109/critical-batch-size) | Critical batch scaling | Official code for model- and data-scale sweeps of critical batch size under μP-controlled pretraining. |
| [dayal-kalra/why-warmup](https://github.com/dayal-kalra/why-warmup) | Warmup / μP | Official supporting code and data for warmup, sharpness, initialization, and standard-versus-μP experiments. |
| [modula-systems/modula](https://github.com/modula-systems/modula) | Modular duality | Official modular-duality software for architecture-aware operator norms and scalable optimizer construction, with [documentation](https://modula.systems/). |
| [DataDog/toto](https://github.com/DataDog/toto) | u-μP application | Official code for Toto 2.0, a time-series foundation-model family trained with a u-μP transfer pipeline. |
| [Datadog/toto-20 checkpoints](https://huggingface.co/collections/Datadog/toto-20) | u-μP application | Released Toto 2.0 model checkpoints accompanying the u-μP scaling report. |
| [LithiumDA/muTransfer-FNO](https://github.com/LithiumDA/muTransfer-FNO) | μTransfer-FNO | Official implementation for zero-shot hyperparameter transfer in Fourier Neural Operators. |
| [muTransfer-FNO data](https://huggingface.co/datasets/LDA1020/muTransfer-FNO-data/tree/main) | μTransfer-FNO | Dataset release used by the μTransfer-FNO experiments. |
| [cofe-ai/Mu-scaling](https://github.com/cofe-ai/Mu-scaling) | μScaling / nanoLM | Early μScaling code release for accurate loss prediction across scales using maximal update parametrization. |
| [cofe-ai/nanoLM](https://github.com/cofe-ai/nanoLM) | nanoLM / μScaling | Expanded official benchmark supporting GPT, LLaMA, BERT, and T5 μP grid search, cross-width transfer, and loss prediction. |
| [CofeAI/NanoData](https://huggingface.co/datasets/CofeAI/NanoData) | nanoLM data | Official 97.12B-token curated pretraining dataset for cross-architecture μScaling studies. |
| [ML-GSAI/Scaling-Diffusion-Transformers-muP](https://github.com/ML-GSAI/Scaling-Diffusion-Transformers-muP) | Diffusion Transformer μP | Official NeurIPS 2025 implementation with DiT / PixArt coordinate checks, proxy-model sweeps, and transferred large-model learning rates. |
| [GSAI-ML/DiT-muP checkpoints](https://huggingface.co/GSAI-ML/DiT-muP) | Diffusion Transformer μP | Released DiT-μP and PixArt-μP pretrained checkpoints accompanying the scaling study. |
| [step-law/steplaw](https://github.com/step-law/steplaw) | Step Law | Official code, experiment records, checkpoints, and fitting tools for optimal LR and batch-size scaling laws. |
| [ML-GSAI/Width-Depth-muP](https://github.com/ML-GSAI/Width-Depth-muP) | Width-depth μP | Official implementation for spectral conditions under joint width-depth scaling. |
| [microsoft/ArchScale](https://github.com/microsoft/ArchScale) | HyperP / architecture scaling | Codebase released with HyperP, SqrtGate, and MuonH scaling experiments. |
| [microsoft/dion](https://github.com/microsoft/dion/) | Dion | Official implementation for distributed orthonormalized updates and robust hyperparameter transfer. |
| [Unakar/Spectral-Sphere-Optimizer](https://github.com/Unakar/Spectral-Sphere-Optimizer) | Spectral Sphere Optimizer | Official implementation for spectral-sphere μP-aligned optimization. |
| [Unakar/Megatron-LM/tree/SSO_main](https://github.com/Unakar/Megatron-LM/tree/SSO_main) | Spectral Sphere Optimizer | Megatron-LM implementation branch for large-scale SSO experiments. |
| [JesseFarebro/flax-mup](https://github.com/JesseFarebro/flax-mup) | Flax / Optax μP | Community Flax and Optax implementation of maximal update parametrization. |
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
