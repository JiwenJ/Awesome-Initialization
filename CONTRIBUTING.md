# Contributing

Thanks for helping keep this list useful. The goal is a compact, evidence-backed index for neural-network initialization, scaling parameterizations, and cross-scale hyperparameter transfer.

## Scope

Good additions usually fit one of these buckets:

- initialization schemes and scaling limits;
- SP, NTK, mean-field, muP, muTransfer, or Tensor Programs papers;
- coordinate-check methods and implementation notes;
- empirical studies of learning-rate, weight-decay, scheduler, embedding/readout, width, depth, batch size, sparsity, MoE, GQA, LoRA, diffusion, or optimizer transfer;
- clear negative results or failed transfer cases.

## Entry Format

Please include:

- paper or resource title;
- source link, preferably arXiv or official project page;
- year;
- one sentence explaining why it matters;
- tags that describe the mechanism or transfer axis.

Suggested table row:

```markdown
| 2026 | Paper Title | One-sentence contribution; include arXiv ID or official URL. | tags |
```

Suggested BibTeX entry:

```bibtex
@article{lastname2026shorttitle,
  title   = {Paper Title},
  author  = {Last, First and Other, Author},
  journal = {arXiv preprint arXiv:xxxx.xxxxx},
  year    = {2026},
  url     = {source URL}
}
```

## Quality Bar

- Prefer primary sources over blog-only summaries.
- Avoid duplicate entries under different titles.
- Keep descriptions factual and specific.
- If the method only loosely relates to muP or transfer, say why it belongs.
- For implementation links, mention the framework and the paper or method it supports.
