# μP, Scale-Aware HPT, and Hyperball Audit — 2026-09-24

This incremental public-source audit follows the repository's [inclusion rules](../CONTRIBUTING.md). It combines recent-paper discovery with backward checks for omitted foundational work. It does not claim exhaustive coverage of private, unindexed, or inaccessible material.

| Collection | Papers before → after | Resources before → after | Artifacts before → after |
|---|---:|---:|---:|
| μP / μTransfer | 161 → 162 | 54 → 55 | 96 → 97 |
| Complementary scale-aware HPT | 17 → 27 | 10 → 11 | 14 → 19 |
| Hyperball | 13 → 13 | 22 → 22 | 24 → 24 |

This adds 11 papers, two teaching resources, and six implementation/artifact entries. HyperP and MACRO occur in both the μP and Hyperball collections, leaving **200 distinct paper records** across the three bibliographies. Resource and artifact collections also overlap and must not be summed as unique links.

## Scope and Method

- Searched μP, muP, μTransfer, maximal-update parameterization, hyperparameter transfer, learning-rate/batch-size scaling, optimizer memory, Hyperball, AdamH, MuonH, HyperP, and HyperTransfer, including September 2026 records and references in recent papers.
- Inspected primary paper metadata and relevant methods, experiments, or appendices; preferred author pages, official repositories, and versioned code over automated summaries.
- Kept first-public dates separate from later manuscript dates. Counted papers, teaching resources, and implementation families separately; code presence does not establish validated transfer.
- Preserved the existing split: direct μP evidence in the μP collection, substantive Hyperball evidence in Hyperball, and complementary scale-aware optimization rules in HPT. Ordinary transfer learning and performance-only scaling laws remain out of scope.

## Newly Included Papers

| Collection | Paper | Evidence and limitation |
|---|---|---|
| μP | [Mix, Don't Tune](https://arxiv.org/html/2605.13225v1) | Sections 3 and 4.3, Appendix B, and the scale-specific sweeps compare AdamW learning-rate/weight-decay transfer from a 150M proxy with independent tuning at 380M–1.43B. Width and depth both change; this is an application study, not new μP theory or a pure-width controlled experiment. First public May 13, not the later date printed in the manuscript. |
| HPT | [Does Step Law Transfer to Small-Scale Language Models?](https://arxiv.org/html/2609.27581) | Tests and recalibrates the published learning-rate/batch-size law below 59M parameters. Tokenizer, data, and recipe also differ from the original work, so the negative result is not attributable solely to model width. |
| HPT | [Optimizer Memory Schedules for Outscaling the Overtraining Axis](https://arxiv.org/html/2609.04577) | Section 3.2 and Appendix E.4 fit horizon-dependent decay prescriptions and study momentum memory. Learning rate is still searched at target settings; this is not zero-shot transfer of the whole recipe. Hyperball is contextual related work, not an evaluated method. |
| HPT | [Configuration-to-Performance Scaling Law with Neural Ansatz](https://arxiv.org/html/2602.10300) | Sections 3.3 and 4.2 use the learned configuration surrogate to choose learning rate and batch size at held-out model/data scales. This substantive optimization experiment distinguishes it from a loss-only scaling law. |
| HPT | [Small Batch Size Training for Language Models](https://arxiv.org/html/2507.07101) | Transfers the second-moment coefficient by preserving its token-based half-life across batch sizes. Its learning-rate observations should not be rewritten as a universal square-root batch law. |
| HPT | [Resolving Discrepancies in Compute-Optimal Scaling of Language Models](https://arxiv.org/html/2406.19146) | Section 3.5 and Appendices G.4–G.5 fit learning-rate and batch-size prescriptions with smaller models and check larger settings, including a 901M neighborhood sweep. The evidence concerns the tested compute-optimal training regime. |
| HPT | [How to Scale Your EMA](https://arxiv.org/html/2307.13813) | Derives and validates model/teacher EMA scaling with batch size. This EMA is distinct from Adam's internal second-moment accumulator; the table does not repeat the questionable weight-decay expression in Appendix C.5. |
| HPT | [Tune As You Scale](https://arxiv.org/html/2306.08055) | Section 4.2 and Appendix C.2 fit optimization-hyperparameter trends along a compute-performance frontier, alongside model and token scales. CARBS is included for this scale-aware use, not for generic cross-task Bayesian optimization or as a verified μP transfer method. |
| HPT | [On the SDEs and Scaling Rules for Adaptive Gradient Algorithms](https://arxiv.org/html/2205.10287) | Derives coordinated batch-scaling rules for Adam/RMSProp learning rate, moment coefficients, and numerical stabilizer, with experiments. Changing only learning rate is not the full prescription. |
| HPT | [Don't Decay the Learning Rate, Increase the Batch Size](https://arxiv.org/abs/1711.00489) | Studies conversion between learning-rate and batch-size schedules, including momentum-aware rules. The finite experimental evidence is not exact equivalence for every optimizer and scale. |
| HPT | [Accurate, Large Minibatch SGD](https://arxiv.org/abs/1706.02677) | Classic SGD linear learning-rate/batch-size scaling with warmup, validated on ImageNet/ResNet. Included as batch-axis transfer, not as μP or evidence for arbitrary model-scale transfer. |

## Resource and Implementation Checks

- [Chenyu Zheng's weight-decay tutorial](https://chen-yu-zheng.github.io/blog/2026/WD-EMA/) connects the EMA-timescale interpretation to μP/CompleteP learning-rate rules and weight-decay prescriptions. It is teaching material, not an additional paper.
- [LM Engine](https://github.com/open-lm-engine/lm-engine) has source-verified μP-style [initialization](https://github.com/open-lm-engine/lm-engine/blob/abb9092812dd69228cba53835d27153f6b674519/lm_engine/training/modeling_utils/init_utils.py), [optimizer groups](https://github.com/open-lm-engine/lm-engine/blob/abb9092812dd69228cba53835d27153f6b674519/configs/param-groups/mup.yml), and [readout scaling](https://github.com/open-lm-engine/lm-engine/blob/abb9092812dd69228cba53835d27153f6b674519/lm_engine/training/mixins/dense/main.py). The entry does not claim independent transfer validation for every supported architecture.
- [microsoft/mup](https://github.com/microsoft/mup) was archived on September 21 and is now read-only. Both indexes label its historical reference status explicitly.
- [Megatron Core's optimizer documentation](https://docs.nvidia.com/megatron-core/developer-guide/latest/apidocs/core/core.optimizer.html) distinguishes optimizer-specific μP overrides. Muon-managed matrices do not receive Adam-style overrides; the [hybrid model source](https://github.com/NVIDIA/Megatron-LM/blob/main/megatron/core/models/hybrid/hybrid_model.py) labels its μP support experimental. Native configuration support is not a universal transfer guarantee.
- Linked repositories and released data were inspected for provenance and stated purpose. Training sweeps, third-party regression tests, and external benchmarks were not rerun.

## Exclusions and Unresolved Leads

- [Rigel's official report](https://github.com/open-lm-engine/open-lm-engine.github.io/blob/a76b625e37143d5630e0c034fb120fa432048c03/src/content/blog/rigel.mdx) states that it uses μP but does not supply a qualifying proxy-to-target sweep or coordinate check. The report is not a new direct μP paper, and it explicitly does not use Hyperball.
- [SpectralShift](https://arxiv.org/abs/2609.14320) remains a lead: an internal context-length-dependent learning-rate heuristic alone does not yet establish the required proxy-to-target hyperparameter validation.
- [NSFT](https://arxiv.org/abs/2609.25655) was not added on the basis of sparse-update learning-rate compensation alone; direct scale-transfer evidence remains unverified.
- Trisham Patil's September μP tutorial was discoverable in an index, but its body was not retrieved; no resource was added from a search snippet alone. Automatic summaries and unverified community claims were not promoted to primary evidence.
- Prior unresolved OpenReview candidates remain unresolved where full text could not be inspected. Failed access is not evidence of absence.

## Hyperball Increment

See the complete [September 24 Hyperball audit in the root README](../README.md#hyperball-incremental-audit--2026-09-24) for version checks, the Palingenesis warmup fix, and the unresolved LR-Transfer-Trajectory dataset. The Hyperball inventory remains 13 papers, 22 resources/reports, and 24 implementation/artifact families.

## Maintenance Validation

The update synchronizes root and companion paper/resource/artifact tables, checks counts against BibTeX, checks citation-key and URL duplication, and validates local links and whitespace. Existing historical audit snapshots retain their original dates and counts.
