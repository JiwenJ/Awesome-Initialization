# μP, Scale-Aware HPT, and Hyperball Audit — 2026-10-20

This update follows [CONTRIBUTING.md](../CONTRIBUTING.md) and extends the [October 15 audit](search-audit-2026-10-15.md) through **October 20, 2026**, with backward checks of missing papers and resources. It adds **8 distinct papers / technical reports**, **2 learning resources**, and **2 implementation / report artifacts**. Three paper additions were first public on October 15–16; five recover older omissions. The cutoff records a public-source search with stated access limits, not guaranteed exhaustive coverage.

| Collection | Papers before → after | Resources before → after | Artifacts before → after |
|---|---:|---:|---:|
| μP / μTransfer | 171 → 173 | 63 → 63 | 102 → 102 |
| Complementary scale-aware HPT | 52 → 58 | 16 → 18 | 28 → 28 |
| Hyperball | 15 → 15 | 26 → 26 | 28 → 30 |

The three bibliographies contain **243 distinct papers / reports**. Their 246 collection rows include three shared μP/Hyperball works: HyperP, MACRO, and the iso-depth looped-LM study. Resource and artifact counts are collection entries and may overlap across topics.

## Search and Inclusion

The incremental search covered μP / muP / μTransfer / maximal updates, private optimization, LR sensitivity, model/depth/batch/token/schedule transfer, Hyperball / AdamH / MuonH aliases, recent arXiv records, official proceedings and author pages, and public code/PR histories. A comparison with the community μP index checked older omissions and alternate identifiers. Already indexed work, including hybrid Transformer–SSM transfer, was not added again.

The strict μP collection requires a substantive method, analysis, or experiment. Complementary HPT requires an explicit optimization-hyperparameter rule over a scale axis. Hyperball implementations were checked for normalized, radius-scaled steps and sphere retraction, rather than accepted by name alone. No training runs or third-party test suites were executed.

## Two μP Additions

| First public date | Paper and primary evidence | Demonstrated scope |
|---|---|---|
| 2026-10-16 | [Towards Hyperparameter Transfer for Differentially Private Optimization](https://arxiv.org/html/2610.22976v1), Sections 3–4 and Appendices C.2 / C.7 | DP-μP combines spectral update scaling with layer-wise SNR matching. Experiments include MLPs, ViTs and character-level Shakespeare GPT-2 models from 76M to 788M. Theory assumes noise-dominated updates; batch tests fix update count and rescale noise; depth remains unresolved. Private proxy search also consumes privacy budget. Its Muon auxiliary routing is not a drop-in Kimi-Muon-plus-AdamW recipe. |
| 2026-10-15 | [Measuring the local shape of learning-rate transfer curves](https://arxiv.org/html/2610.21814v1), Sections 3–5, Figure 7 and Appendix D | Conditional edge-of-stability and heuristic fixed-point expressions estimate LR-curve gradients. Layerwise profiles and explicit μP ResNet width sweeps test output-multiplier sensitivity. These diagnostics do not establish a universal optimal-LR formula. |

Both primary arXiv histories list v1 only at this cutoff. Secondary-site update timestamps are not manuscript revisions. No author-code release was verified for either addition.

## Six Complementary HPT Additions

| First verified public date | Paper / report | Qualifying result and limitation |
|---|---|---|
| 2026-10-15 | [Frontier Pre-training at Microsoft AI](https://microsoft.ai/news/frontier-pre-training-at-microsoft-ai/), official report and accompanying PDF, pp. 5–7 | Joint LR / LR×WD sweeps and quadratic fits produce step/TPP-dependent laws plus an active-size dropout rule. The authors report intermediate-scale validation before the MAI-Base-2 application; a frontier-scale optimum sweep and refittable raw sweep data are not released. |
| 2026-01-31 | [Over-Alignment vs Over-Fitting](https://arxiv.org/abs/2602.00827), reviewed v2 / ICML 2026, Section 5.4 and Figure 6 | A two-layer ReLU bound motivates width/data output-multiplier scaling, compared with five-layer CNN optima. Those optima use test loss. Its prior exclusion from strict μP remains valid; the separate multiplier-transfer result qualifies for complementary HPT. |
| 2025-11 | [Training compute-optimal transformer encoder models](https://aclanthology.org/2025.emnlp-main.1804/), Section 3 / Figure 2 and Appendix D | 512 Sobol configurations yield compute-dependent LR/batch fits used for OptiBERT. A wide near-optimal region makes fits noisy; larger-model quality does not independently verify target LR optimality. Only month-level proceedings-date precision was established. |
| 2024-11-25 | [Towards Precise Scaling Laws for Video Diffusion Transformers](https://arxiv.org/html/2411.17470v2), Section 3 / Figures 2–4, CVPR 2025 | Fits LR/batch rules over 17M–260M models and token budgets, then checks predictions with a 1.07B target grid at 4B/10B tokens. These are constant-LR, validation-loss experiments. |
| 2024-09-07 | [Optimization Hyper-parameter Laws for Large Language Models](https://arxiv.org/html/2409.04777v4), reviewed v4, May 20, 2026 | Uses small-scale MoE schedule features to rank peak-LR, warmup and schedule-family candidates at held-out model/data scales. The reviewed version adds explicit candidate-ranking and divergence tests; this is an empirical surrogate, not a universal closed-form optimum. |
| 2024-08-20 | [Scaling Law with Learning Rate Annealing](https://proceedings.neurips.cc/paper_files/paper/2025/file/830b1abc6d2da85f23d41169fa44d185-Paper-Conference.pdf), reviewed NeurIPS 2025 version, Section 4 / Appendix I | Short-curve fits predict longer horizons and unseen schedules, including WSD cooldown selection. The paper documents collapsed schedules from unconstrained mathematical optimization of the approximate formula. |

The new resource rows are [LoRA Without Regret](https://thinkingmachines.ai/blog/lora/) (September 29, 2025) and [Tinker optimizer guidance](https://tinker-docs.thinkingmachines.ai/tinker/optimizers/) (living documentation checked October 20). They explain rank/initialization/LR transfer and optimizer-dependent batch rules. The approximate LoRA/full-fine-tuning LR ratio is empirical; Tinker's linear-to-zero rank tests and constant-LR batch tests have different protocols. Neither resource supplies a universal AdamW/Dimuon conversion. No additional experiment-code artifact was verified for this HPT pass.

## Hyperball Implementations and Existing Updates

No new direct Hyperball paper or post-October-15 revision was verified among the fourteen existing arXiv histories. The two artifact additions are explicitly community implementations / reports:

- [DETR PR #11](https://github.com/troydutton/detr/pull/11), merged October 19: the [pinned MuonH source](https://github.com/troydutton/detr/blob/2a770f9be4cbb9ddf68e86cd0832fe787bb56323/src/optimizers/muonh.py) caches first-step radii, normalizes directions, radius-scales steps, and retracts. Its reported 16-epoch MuonH versus 24-epoch AdamW comparison changes LR/schedule and lacks repeat variance. Base-Muon tests were inspected; Hyperball-specific tests were not verified.
- [UFSM](https://github.com/SuperOptimizer/ufsm/blob/0b5da92b17ebebc1a1f144853378e55781751b3e/DESIGN.md), released October 16: selected GroupNorm-fed 3D convolutions use the normalized radius-scaled wrapper and checkpointed radii. The 2,000-step Hyperball arm is slightly worse than Muon; the limited tuning and repeats do not establish a general optimizer ranking.

Existing rows now record [MarinSkyRL #1002](https://github.com/marin-community/MarinSkyRL/pull/1002) (merged October 17, global clipping across optimizer groups), [NeMo #312](https://github.com/NVIDIA-NeMo/Emerging-Optimizers/pull/312) (open October 20, registry-validation repair), and [Paddle #79844](https://github.com/PaddlePaddle/Paddle/pull/79844) (merged October 15, generic mixed-sharding interfaces). The existing [rollfast 1.3.0](https://pypi.org/project/rollfast/1.3.0/) entry records a release whose Hyperball file matches the earlier source pin; PRISM's `bidirectional` argument is not a new Hyperball geometry. The existing Marin digest distinguishes measured four-scale gains from a fitted crossover extrapolation. These updates add no duplicate artifact/resource counts. The [complete current Hyperball evidence is also in the root README](../README.md#hyperball-search-coverage-and-maintenance).

## Unresolved and Excluded Sources

- The SSRN [Quantitative Mean-Field Limits for the Training of Maximal-Update Neural Networks via Explicit Couplings](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=7548300) remains uncounted: its existence is confirmed by the author page, but the full text could not be retrieved.
- The bioRxiv candidate [Scaling recipes for single-cell RNA sequencing foundation models](https://doi.org/10.64898/2026.08.31.747783) has relevant indexed excerpts but returned 403/429 for full-text retrieval; it remains pending.
- The [Two Clocks author-code URL](https://github.com/WinstonLiyt/two-clocks-rlvr) still returns 404. Author code for DP-μP, the LR-gradient study, ShellElastic and σTransfer was not verified.
- Previously unresolved tutorials by [Su Jianlin](https://kexue.fm/archives/10770), [Trisham Patil](https://trishampatil.com/ai%20engineering/2026/09/06/mup-maximal-update-parameterization-hyperparameter-transfer/) and [Mohak Chugh](https://mohakchugh.is-a.dev/blog/mup-unit-scaling-hyperparameter-transfer-fp8-training/) remain outside counted tables where their bodies could not be retrieved.
- [BMuon](https://arxiv.org/abs/2610.23571) has no substantive μP result. [Predictable Scaling](https://arxiv.org/abs/2610.23453) studies architecture/performance extrapolation rather than a qualifying optimization-hyperparameter transfer rule. Generic Tensor Programs limits, spherical-optimization terminology, graph HyperBall, derivative summaries and future course sessions are not promoted by keyword matches alone.

Direct source pages and records were available even when arXiv search endpoints, bioRxiv bodies, and some author/tutorial hosts were not. Search access and indexing limits preclude an absolute-completeness claim.

## Validation

Validation checks the nine mirrored paper/resource/artifact tables, exact paper-to-BibTeX URL correspondence with the existing DOI/publisher alias, unique keys, shared metadata, balanced bibliography records, reverse dates, local file links and section anchors, current counts, and whitespace. The README includes the full current Hyperball guide/audit. Unrelated local work is excluded from the publication commit.
