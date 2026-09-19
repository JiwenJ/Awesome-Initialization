# Contributing

Thanks for helping keep this list useful. The goal is a compact, evidence-backed index of work that directly studies **maximal-update parametrization (μP)** and **μTransfer**, with companion collections on **Hyperball optimization** and **scale-aware hyperparameter transfer**.

## Scope

An addition to the μP collection must do at least one of the following in its main text:

- derive or extend μP, μTransfer, maximal-update scaling, Depth-μP, CompleteP, u-μP, SμPar, or a clearly identified descendant;
- implement or experimentally validate a μP parameterization, coordinate check, or proxy-to-target transfer;
- test a failure mode, limitation, or negative result of μP itself;
- materially apply μP in a model-scaling recipe, with either an explicit proxy-to-target transfer or an independent coordinate / transfer check;
- provide primary-source documentation, code, or teaching material for an included μP work.

Tensor Programs work is retained only when it directly develops a maximal-update / feature-learning parameterization or extends μP; generic NNGP, NTK, or master-theorem foundations are not enough. Likewise, a generic paper on initialization, Muon, learning-rate scaling, batch size, token horizon, schedulers, weight decay, or ordinary hyperparameter transfer is **not** in scope unless μP is an actual method, experimental control central to the result, or object of analysis. A related-work citation, analogy such as “μP-style,” or an unvalidated recipe mention is not enough.

### Hyperball Collection

The dedicated [Hyperball section](README.md#hyperball) and [guide](docs/hyperball.md) also accept papers that derive, extend, analyze, experimentally compare, criticize, or materially apply the Hyperball wrapper, including AdamH and MuonH. These entries do not require an independent μP result. Generic Muon, spherical constraints, or a related-work-only Hyperball citation does not qualify for its direct-paper table; a small, explicitly labeled related-reading list may explain relevant distinctions.

Count a formal paper once per collection. Treat the original author note, its living 2.1 / 2.2 versions, and the formal Hyperball paper as one research lineage; notes and talks belong in resources. State first-public dates and identify later versions when the relevant result was added. Distinguish author implementations, framework integrations, community experiments, and unverified artifacts.

### Scale-Aware Hyperparameter-Transfer Collection

The dedicated [scale-aware HPT section](README.md#scale-aware-hyperparameter-transfer) and [guide](docs/hyperparameter-transfer.md) accept work that transfers optimization hyperparameters from affordable proxy runs to larger or otherwise more expensive target runs. A direct paper must derive, estimate, validate, or falsify a rule over at least one stated scale axis, such as width, depth, parameters, model shape, tokens, compute, batch size, schedule length, sparsity, expert configuration, adaptation rank, or post-training scale.

Cross-dataset AutoML transfer, ordinary transfer learning, neural architecture search, and performance-only scaling laws do not qualify unless the main text also contains a substantive optimization-hyperparameter rule. Theory-only work may qualify when it explicitly derives a scale-dependent optimum or invariant recipe and clearly states the model or optimizer setting. Describe narrow theory, embedded paper sections, and empirical fits at their demonstrated scope rather than as universal laws.

Keep the Hyperball paper, resource, and artifact tables identical between `README.md` and `docs/hyperball.md`, and synchronize its direct-paper table with `papers/hyperball.bib`. Keep the existing μP tables synchronized between `README.md` and `docs/mup-transfer.md`, with `papers/mup-transfer.bib` for its papers. Keep the complementary HPT paper, resource, and artifact tables identical between `README.md` and `docs/hyperparameter-transfer.md`, and synchronize its direct-paper table with `papers/hyperparameter-transfer.bib`. Papers already counted by μP or Hyperball should be cross-linked from the HPT guide rather than duplicated in its bibliography. A work included in more than one strict collection retains the same citation key and metadata; overlapping rows are not additional unique works.

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
- Confirm substantive relevance to the collection being updated; a related-work mention alone is insufficient.
- For application reports, identify the proxy, target, transferred quantities, and validation evidence when the paper provides them.
- For implementation links, mention the framework and the paper or method it supports.
