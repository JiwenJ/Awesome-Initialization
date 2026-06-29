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

## 🔥 Extended μP Literature Addendum (Deep Research 2024–2026)

This section ensures coverage of additional μP-related literature discovered via Deep Research sweep.

### Key missing / emphasized works

- https://arxiv.org/abs/2309.16620 — Depthwise Hyperparameter Transfer in Residual Networks (critical depth-transfer baseline)
- https://arxiv.org/abs/2404.05728 — Empirical Study of μP Learning Rate Transfer (core validation of μTransfer)
- https://arxiv.org/abs/2407.05872 — Scaling Exponents Across Parameterizations and Optimizers
- https://arxiv.org/abs/2411.07340 — Warmstarting for Scaling Language Models
- https://arxiv.org/abs/2405.15743 — Sparse μP (SμPar)
- https://arxiv.org/abs/2407.17465 — u-μP (unit scaling)
- https://arxiv.org/abs/2505.15270 — μP for Diffusion Transformers (DiT / MMDiT)
- https://arxiv.org/abs/2602.07494 — Multi-path / effective depth transfer laws
- https://arxiv.org/abs/2602.06204 — LoRA rank transfer under μP-style scaling
- https://arxiv.org/abs/2603.28743 — HyperP (Muon + hypersphere alternative)

### Blog / engineering layer additions

- Cerebras μP practitioner guide
- Microsoft μTransfer blog (Tensor Programs V workflow)
- Graphcore unit-scaling / u-μP docs
- kexue.fm μP analysis series
- Megatron-LM scaling discussion issues

### Interpretation

- μP is stable for width transfer
- embedding LR is a dominant correction factor
- optimizer geometry (Muon / AdamW / Shampoo) is a first-class axis
- modern systems are multi-axis transfer systems, not single-law systems

---

## Scope

μP is a scaling-aware parameterization covering initialization, learning-rate scaling, optimizer grouping, embedding/readout scaling, and coordinate checks.

---

## Topic Map

- Width transfer
- Depth transfer (Depth-μP, CompleteP)
- Embedding scaling
- Optimizer-dependent transfer
- Architecture-specific μP (MoE, GQA, diffusion, LoRA)
- Post-μP frameworks (u-μP, HyperP, SμPar)

---

## Paper Timeline

(unchanged from previous version for stability)

---

## Code and Implementations

(unchanged)

---

## Contributing

Add missing papers, implementations, and empirical reports.
