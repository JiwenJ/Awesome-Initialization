# Awesome Initialization

Curated resources on neural-network initialization, scaling parameterizations, and cross-scale hyperparameter transfer.

Current focus: **μP / muP — Maximal Update Parametrization** and **μTransfer — zero-shot hyperparameter transfer**.

> Snapshot date: 2026-06-29.

## Contents

- [μTransfer / μP Hyperparameter Transfer](docs/mup-transfer.md)
- [BibTeX references](papers/mup-transfer.bib)

## Why μP matters

μP is not just an initialization trick. It is a scaling-aware parameterization recipe covering initialization scales, learning-rate multipliers, readout/embedding treatment, and optimizer parameter groups. The practical goal of μTransfer is:

1. Convert the target architecture to μP.
2. Tune key hyperparameters on a small proxy model.
3. Transfer those hyperparameters to a much larger model without re-running the expensive sweep.

This is especially relevant for large Transformers, diffusion Transformers, sparse models, neural operators, and recent work on optimizer- or architecture-specific scaling laws.

## Start here

For a quick reading path, use:

1. **Theory foundation** — [Feature Learning in Infinite-Width Neural Networks](https://arxiv.org/abs/2011.14522)
2. **Core μTransfer paper** — [Tensor Programs V: Tuning Large Neural Networks via Zero-Shot Hyperparameter Transfer](https://arxiv.org/abs/2203.03466)
3. **Adaptive optimizers** — [Tensor Programs IVb: Adaptive Optimization in the Infinite-Width Limit](https://arxiv.org/abs/2308.01814)
4. **Depth scaling** — [Tensor Programs VI: Feature Learning in Infinite-Depth Neural Networks](https://arxiv.org/abs/2310.02244)
5. **Recent practical caveat** — [Quantifying Hyperparameter Transfer and the Importance of Embedding Layer Learning Rate](https://arxiv.org/abs/2605.21486)

## Recent μP / μTransfer directions

- **Width transfer for LLMs** — original μTransfer framing and practical Transformer results.
- **Depth transfer** — Depth-μP, effective-depth laws, and limitations in modern multi-layer residual blocks.
- **Embedding and readout scaling** — recent evidence that embedding-layer learning rate can dominate apparent μP gains in AdamW language-model training.
- **Architecture-specific μP** — GQA-μP, diffusion Transformers, probabilistic Transformers, Fourier neural operators.
- **Training-system variants** — u-μP for unit-scaled / low-precision training, SμPar for sparse training, HyperP for hypersphere optimization with Muon.
- **Schedule and data scaling** — combining μP with token-/batch-size-aware learning-rate schedules.

## Implementation links

- [microsoft/mup](https://github.com/microsoft/mup) — reference PyTorch implementation mentioned by Tensor Programs V.
- [EleutherAI/nanoGPT-mup](https://github.com/EleutherAI/nanoGPT-mup) — compact GPT-style implementation useful for experiments.
- [EleutherAI/nanoGPT-mup/tree/supar](https://github.com/EleutherAI/nanoGPT-mup/tree/supar) — minimal SμPar implementation for sparse maximal update parameterization.
- [microsoft/ArchScale](https://github.com/microsoft/ArchScale) — codebase released with HyperP / hypersphere-transfer work.

## Contributing

Useful additions:

- papers on initialization, scaling limits, μP, μTransfer, Tensor Programs, SP/NTK/mean-field parameterizations;
- implementation notes and coordinate-check scripts;
- empirical reports on learning-rate, weight-decay, scheduler, embedding/readout, width, depth, sequence length, batch size, sparsity, MoE, GQA, LoRA, or diffusion transfer;
- short summaries of failed transfer cases.

Prefer adding source links, arXiv IDs, and a one-sentence reason why the resource matters.
