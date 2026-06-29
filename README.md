# Awesome Initialization

> Curated resources on neural-network initialization, scaling parameterizations, and cross-scale hyperparameter transfer.

[![Awesome](https://awesome.re/badge-flat2.svg)](https://awesome.re)
[![Scope](https://img.shields.io/badge/scope-initialization%20%7C%20scaling%20%7C%20transfer-2f6f9f)](#scope)
[![PRs welcome](https://img.shields.io/badge/PRs-welcome-2ea44f)](CONTRIBUTING.md)

This repository tracks papers, implementation notes, engineering reports, and practical checklists for initialization and scaling recipes that make neural-network training more predictable across model sizes.

Current focus: **μP / muP**, maximal-update scaling, and adjacent hyperparameter-transfer methods.

> Snapshot: 2026-06-29

## Start Here

| Order | Theme | Resource |
|---:|---|---|
| 1 | Theory foundation | https://arxiv.org/abs/2011.14522 |
| 2 | Core μTransfer recipe | https://arxiv.org/abs/2203.03466 |
| 3 | Adaptive optimizers | https://arxiv.org/abs/2308.01814 |
| 4 | Depth scaling | https://arxiv.org/abs/2310.02244 |
| 5 | Empirical LR transfer | https://arxiv.org/abs/2404.05728 |
| 6 | Embedding LR caveat | https://arxiv.org/abs/2605.21486 |

---

## Full μP Literature Timeline (Restored)

| Date | Paper | Contribution | Tags |
|---|---|---|---|
| 2026-06-16 | On residual scaling of looped transformers | Loop depth transfer stability | Looped Transformers |
| 2026-06-02 | Gated Delta Networks scaling | Width LR transfer validation | Feature learning |
| 2026-05-23 | Wide NN under μP | Mean-field + identifiability | Theory |
| 2026-05-20 | Embedding LR importance | Embedding dominates transfer | LLM training |
| 2026-05-19 | Toto 2.0 scaling | u-μP in time series | Applications |
| 2026-05-18 | Scale-invariant optimization | Norm geometry view | Optimizers |
| 2026-05-14 | GQA-μP | Attention transfer rules | Transformers |
| 2026-05-13 | MoE scaling laws | MSSP extension | MoE |
| 2026-05-11 | Dense associative memories | HPT extension | Memory models |
| 2026-05-08 | Spectral dynamics | Outlier escape + LR transfer | Spectral |
| 2026-04-29 | νGPT scaling | normalized transformer transfer | Transformers |
| 2026-04-28 | Probabilistic Transformer μP | cross-scale transfer | Diffusion/MLM |
| 2026-03-30 | HyperP | Muon + hypersphere optimization | Alternatives |
| 2026-03-16 | optimizer scaling theory | batch/LR laws | Theory |
| 2026-03-10 | operator norm scaling | matrix view of μP | Optimizers |
| 2026-02-28 | spectral μP conditions | unified transfer laws | Theory |
| 2026-02-24 | multi-path NN transfer | effective depth laws | Architectures |
| 2026-02-11 | μpscaling warm start | model growth transfer | LLM scaling |
| 2026-02-07 | predictive coding limits | BP equivalence | Theory |
| 2026-02-05 | LoRA rank transfer | PEFT scaling laws | LoRA |
| 2026-01-31 | feature strength | generalization regimes | Theory |
| 2026-01-28 | MoE μP | expert scaling rules | MoE |
| 2026-01-25 | IMU-1 LLM recipe | practical μP training | LLM |
| 2026-01-13 | spectral sphere optimizer | optimizer alignment | Optimizers |
| 2026-01-08 | learnable multipliers | replacing fixed μP scaling | LLMs |
| 2026-01-04 | Muon++ spectral conditions | stable training dynamics | Optimizers |
| 2025-12-31 | concept latent scaling | decoupled μP | Applications |
| 2025-12-28 | fast hyperparameter transfer | limits of μP | Theory |
| 2025-12-26 | CompleteP extension | module-level transfer | Deep nets |
| 2025-12-20 | optimization survey | large-scale optimizers | Survey |
| 2025-12-05 | matrix-preconditioned transfer | Shampoo/SOAP/Muon | Optimizers |
| 2025-11-23 | 1.3B SLM scaling | proxy-to-target transfer | LLM |
| 2025-11-03 | proof of LR transfer | theoretical guarantee | Theory |
| 2025-10-21 | weight decay critique | μP limitations | Critique |
| 2025-10-17 | AdamW scaling rules | refined μP | Optimizers |
| 2025-10-05 | AM-μP | averaged update rule | CNNs |
| 2025-08-13 | MoE μ-parametrization | expert scaling laws | MoE |
| 2025-07-11 | optimizer comparison | proxy tuning | LLM |
| 2025-07-02 | scaling collapse dynamics | compute-optimal behavior | Theory |
| 2025-06-24 | μTransfer-FNO | PDE operators scaling | Scientific ML |
| 2025-06-17 | embedding LR scaling | vocab-size correction | LLM |
| 2025-05-28 | large LR under SP | challenge to μP view | Critique |
| 2025-05-21 | diffusion μP | DiT / MMDiT scaling | Diffusion |
| 2025-05-19 | weight decay scaling | batch + WD laws | Optimizers |
| 2025-05-19 | predictive coding μP | deep network scaling | Theory |
| 2025-05-04 | Muon + μP efficiency | practical pipeline | Optimizers |
| 2025-05-02 | CompleteP | depth transfer | Deep nets |
| 2025-03-12 | infinite-width convergence | theory guarantee | Theory |
| 2025-02-24 | function-space LR | LoRA + width transfer | Theory |
| 2025-02-09 | FP8 μP scaling | unit scaling | Systems |
| 2025-02-04 | deep linear dynamics | full transfer theory | Theory |
| 2024-11-11 | warmstarting LM | shrink/expand transfer | LLM |
| 2024-11-04 | local loss μP | alternative training | Theory |
| 2024-10-31 | SAM μP extension | perturbation scaling | Optimization |
| 2024-10-06 | feature strength SGD | LR regime shifts | Theory |
| 2024-08-23 | batch/token LR scheduler | schedule transfer | Optimizers |
| 2024-07-31 | physics lectures | theoretical foundation | Notes |
| 2024-07-24 | u-μP | unit scaling | Systems |
| 2024-07-08 | scaling exponents | optimizer comparison | Theory |
| 2024-05-24 | SμPar | sparse μP | Sparse |
| 2024-02-27 | landscape consistency | LR transfer theory | Theory |
| 2023-10-03 | Depth-μP | depth scaling | Core theory |
| 2023-08-03 | Adam infinite width | optimizer theory | Core theory |
| 2022-03-07 | Tensor Programs V | μTransfer origin | Core |
| 2020-11-30 | infinite-width feature learning | foundation | Core |

---

## Learning Resources

(unchanged)

## Code and Implementations

(unchanged)

## Contributing

(unchanged)
