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
- [Implementation links](#implementation-links)
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

For the full guide, including the current two-year μP paper sweep, see [μTransfer / μP Hyperparameter Transfer](docs/mup-transfer.md).

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

## Implementation Links

| Resource | Notes |
|---|---|
| [microsoft/mup](https://github.com/microsoft/mup) | Reference PyTorch implementation mentioned by Tensor Programs V. |
| [EleutherAI/nanoGPT-mup](https://github.com/EleutherAI/nanoGPT-mup) | Compact GPT-style implementation useful for experiments and coordinate checks. |
| [EleutherAI/nanoGPT-mup/tree/supar](https://github.com/EleutherAI/nanoGPT-mup/tree/supar) | Minimal SμPar implementation for sparse maximal update parameterization. |
| [microsoft/ArchScale](https://github.com/microsoft/ArchScale) | Codebase released with HyperP / hypersphere-transfer work. |

## Repository Layout

| Path | Purpose |
|---|---|
| [docs/mup-transfer.md](docs/mup-transfer.md) | Main μP / μTransfer reading guide, paper map, and practical checklist. |
| [papers/mup-transfer.bib](papers/mup-transfer.bib) | BibTeX references for the μP / μTransfer collection. |
| [papers/README.md](papers/README.md) | Notes on maintaining reference files. |
| [CONTRIBUTING.md](CONTRIBUTING.md) | Contribution scope and entry template. |

## Contributing

Useful additions include papers, implementation notes, coordinate-check scripts, empirical transfer reports, and short writeups of failed transfer cases. Please include source links, arXiv IDs when available, and a one-sentence reason why the resource matters.

See [CONTRIBUTING.md](CONTRIBUTING.md) for the suggested format.
