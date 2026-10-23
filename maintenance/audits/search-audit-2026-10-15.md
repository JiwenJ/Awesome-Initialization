# μP, Scale-Aware HPT, and Hyperball Audit — 2026-10-15

This update follows [CONTRIBUTING.md](../../CONTRIBUTING.md), extends the [October 9 audit](search-audit-2026-10-09.md) through **October 15, 2026**, and checks older omissions. It adds **9 distinct papers**: two direct μP extensions and seven complementary HPT studies. Revisions of an existing paper and updates within an existing implementation family are not new papers or independent implementations. The cutoff describes a best-effort public-source search, not guaranteed coverage of private, inaccessible, unindexed, or newly posted work.

| Collection | Papers before → after | Resources before → after | Artifacts before → after |
|---|---:|---:|---:|
| μP / μTransfer | 169 → 171 | 62 → 63 | 102 → 102 |
| Complementary scale-aware HPT | 45 → 52 | 14 → 16 | 26 → 28 |
| Hyperball | 15 → 15 | 25 → 26 | 27 → 28 |

The bibliographies now contain **235 distinct papers**, not 238: HyperP, MACRO and the iso-depth looped-LM study remain shared by the μP and Hyperball collections. The update also adds **4 resource rows** and **3 artifact rows**; these are collection entries, not independent reproductions.

## Scope and Search

The incremental window was October 9–15, with backward checks prompted by recent batch-size discussions and references. Queries covered μP / muP / μTransfer / maximal updates, elastic parameterizations, uncertainty transfer, depth transfer, learning-rate schedules, token horizons, repeated data, optimal batch size, critical batch size, RLVR, Hyperball / AdamH / MuonH, and author implementation records. Sources included arXiv metadata and full text, official author/institutional pages, GitHub source and PR metadata, and the existing community μP index. First-public dates are kept distinct from later revisions, venue years, and secondary-site timestamps.

The three collections retain their separate inclusion rules. A generic Muon paper does not qualify for Hyperball. A generic Tensor Programs limit does not itself constitute a new μP recipe. Direct HPT entries must estimate, derive, test, or falsify a hyperparameter rule over an explicit scale axis. The review did not run training sweeps or third-party optimizer implementations.

## Two Direct μP Extensions

| Paper | Primary evidence and demonstrated boundary |
|---|---|
| [ShellElastic](https://arxiv.org/html/2610.15124v1) — first public October 10 | Sections 3.2 / 4.2, Figure 3, and Appendices C / H introduce μE for simultaneously maximal updates in nested elastic subnetworks and test proxy learning-rate / initialization transfer across width and shell configurations. The transfer sweeps hold depth at 32; larger main-training runs are not separate target-optimum sweeps. The promised code/config/log release was not verified as available. |
| [σTransfer](https://arxiv.org/html/2610.11668v1) — first public October 8 | Sections 4–6 and Appendices I / K rescale prior covariance under μP and transfer Laplace prior precision and posterior-derived decisions. This is uncertainty-hyperparameter transfer, not a new optimizer-LR rule. The formal result concerns fixed-depth finite-step ReLU MLPs; GPT readouts and the public u-μP 1B→7B pair are empirical extensions, and the public pair changes depth as well as width. |

The [CompleteP researcher note](https://dmartinezt.github.io/research_notes/nn_parametrization/completep.html), dated September 18, is now readable and added as a learning resource. It follows the corrected January 2026 CompleteP v4, including the two-matrix nonlinear interaction criterion and optimizer-dependent depth prescriptions. It is not another paper. Previously identified `mamba.py` PR #50 is already represented in both resources and artifacts, so it is not added again.

## Seven Complementary HPT Additions

| Paper | Qualifying result and scope |
|---|---|
| [Two Clocks](https://arxiv.org/html/2610.18853v1) — October 13, 2026 | Sections 3 / 5 test square-root LR scaling failures in multi-step GRPO; Section 6 evaluates a small-to-large rollout-batch schedule in Qwen2.5. Rollout batch, optimizer batch, fresh-response count and optimizer-step count are distinct axes. The ramp resets optimizer state, so its result is not an isolated batch-only improvement or a universal optimal-LR law. |
| [Asymptotic Risk of Learning-Rate Schedules](https://arxiv.org/html/2610.16518v1) — October 12, 2026 | Section 5.2 / Equation 19 derives LR transfer ratios across WSD cooldown fractions, and Section 6 studies batch and stability effects. The analysis concerns streaming least-squares SGD with discretely distributed features. Its near-critical-batch extrapolation is heuristic; the formulas are not established AdamW/LLM transfer rules. |
| [Scaling Repeated-Data Pretraining](https://arxiv.org/html/2610.15965v1) — October 11, 2026 | Section 3.2 / Table 1 tests failure of single-pass optimal-batch laws in repeated-data training; Appendix D.1 fits batch and stopping-epoch laws over model and unique-data scale. The 153M–1.5B labels are nominal untied sizes (approximately 102M–1.28B after tying in Appendix A.1); measured runs are distinct from the illustrative 70B / 10T extrapolation and fitted GPU-time estimates. |
| [Depth Transfer via L-Smoothness](https://arxiv.org/html/2610.14344v1) — October 9, 2026 | Bounds aggregate sensitivity and smoothness under inverse-depth residual scaling on bounded domains and tests Muon/MOGA depth-LR transfer. GPT-2 experiments use 315M tokens and cosine decay to 10% of peak; the ResNet validation-optimum drift in Appendix C must not be hidden by training-loss plots. Curvature bounds motivate a scale but do not prove invariant finite-run optimal LR. |
| [Convergence Bound and Critical Batch Size of Muon Optimizer](https://arxiv.org/html/2507.01598v5) — July 2, 2025; v5 June 8, 2026 | Sections 4–5 derive lower bounds on the sample-complexity-optimal batch and examine momentum, decay and width trends, including 320M language modeling. Unknown problem quantities prevent the bound from being an absolute CBS estimator. The reviewed title and bounds are from v5, not an earlier version. |
| [Critical Batch Size Revisited](https://arxiv.org/html/2505.23971v3) — May 29, 2025 | Sections 3–4 measure local CBS using equal 2B-token branches with an empirical 0.01 loss tolerance in OLMo 1B/7B. Batch warmup with square-root LR adjustment saves 43% gradient steps in the 1B experiment, not necessarily 43% FLOPs or wall time. The window, recovery assumption and absence of independently optimal LR sweeps are explicit. |
| [How Does Critical Batch Size Scale in Pre-training?](https://arxiv.org/html/2410.21676v4) — October 29, 2024 | Sections 2–3 tune batch-specific settings, use a 20% steps-to-target overhead definition, and decouple model/data effects in 85M–1.2B C4 models. Section 4.1 invokes a general infinite-width limit, using μP as a motivating example; a uniform-over-batch CBS consequence is described as an expectation rather than a separately proved universal transfer guarantee. It is indexed in complementary HPT, not counted as a new strict μP method. |

The two official CBS explainers from [Ai2](https://allenai.org/blog/critical-batch-size) and [Kempner](https://kempnerinstitute.harvard.edu/research/deeper-learning/how-does-critical-batch-size-scale-in-pre-training-decoupling-data-and-model-size/) are learning resources. The [CBS author repository](https://github.com/hlzhang109/critical-batch-size) provides training and sweep configurations; [Asymptotic_risk](https://github.com/noecolomban/Asymptotic_risk) provides least-squares recursions, simulations, stored results and figure scripts. Sources were inspected, not executed.

CBS is not a single interchangeable number: fixed-target efficiency thresholds, checkpoint-local loss-recovery thresholds, gradient-noise proxies, and peaks of optimal-LR curves answer different questions. Batch units, schedule, token budget, target loss, tuning protocol and tolerance must accompany a reported value. The HPT guide now records this distinction.

## Hyperball Revisions, Teaching Material, and Implementation Updates

No new direct Hyperball paper was verified. The fourteen existing arXiv histories were checked; [The Best Optimizer Depends on Batch Size](https://arxiv.org/abs/2610.08975) is newly revised to v2 on October 12. Its Appendix F repeat/median selection protocol must be distinguished from the [released companion-data provenance](https://github.com/dangxingyu/batch-size-blog/blob/dcef47e3f1e947e4a853870b5f2d83651f634c9e/batch-size/data/provenance.json), which still describes mostly single-run Hyperball endpoints and observed ranges rather than confidence intervals. Full repeated training logs were not independently verified. The [canonical author site](https://batch-size-scaling.com/) replaces its old domain without adding a resource count.

The recovered official [Stanford CS312 Lecture 3](https://deep-learning-alchemy.github.io/assets/slides/lecture_03.pdf) and [Assignment 2](https://deep-learning-alchemy.github.io/assets/assignments/a2_hyperparameter_scaling.pdf) form one teaching resource. Its [pinned assignment release](https://github.com/deep-learning-alchemy/assignments/tree/069e18c52c93a0c6626b00f1c79de070ba30275f) forms one code/data artifact: an AdamH implementation, routing/norm/checkpoint tests, and 24 supplied seed-42 AdamH runs over 153.6M–614.4M tokens. The held-out transfer task is an exercise, not a published independent transfer result. Neither its tests nor its training runs were executed in this audit.

Two existing integration rows were updated, not counted again:

- [Paddle #79850](https://github.com/PaddlePaddle/Paddle/pull/79850), merged October 12, separately updates logical slices of fused parameters, correcting the difference between a combined norm constraint and separate gate/up constraints. Source and tests were inspected; no loss gain is inferred. The release cherry-pick #79851 is closed unmerged and the sharding proposal #79844 remains unmerged.
- [MarinSkyRL #939](https://github.com/marin-community/MarinSkyRL/pull/939), merged October 9 UTC, adds native Megatron MuonH/AdamH grouping and continuation. Whole-logical-QKV, separate gate/up and per-expert norms, CPU momentum offload and FP32 master weights are recipe details, not evidence of independently verified RL-quality improvement. Tensor parallelism remains restricted to size one.

The closed-unmerged [Dion MuonH proposal #71](https://github.com/microsoft/dion/pull/71) is not upstream support. Unofficial CS312 derivative notes are not counted separately. Existing bioRxiv/Notion/OpenReview access limits are not silently treated as resolved. The [maintenance archive includes the complete current Hyperball evidence](hyperball-history.md#hyperball-search-coverage-and-maintenance).

## Exclusions and Access Limits

- The [Two Clocks author-code URL](https://github.com/WinstonLiyt/two-clocks-rlvr) returned 404 and is not counted as an available implementation.
- The primary arXiv history for [Depth Transfer via L-Smoothness](https://arxiv.org/abs/2610.14344) lists v1 on October 9. A secondary index's October 14 timestamp is not treated as a manuscript revision.
- ShellElastic's promised release and an unverified σTransfer implementation are not counted as artifacts. A paper can qualify before its code is released, but the two evidence categories must remain separate.
- The SSRN candidate [Quantitative Mean-Field Limits for the Training of Maximal-Update Neural Networks via Explicit Couplings](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=7548300), by Frederick Altrock and Steffen Dereich, was posted October 2 and is listed on the [official author page](https://www.uni-muenster.de/Stochastik/Arbeitsgruppen/Dereich/). Its delivery PDF returned 403; title/abstract relevance alone is not enough for promotion into the direct-paper table.
- The previously unresolved tutorials by [Su Jianlin](https://kexue.fm/archives/10770), [Trisham Patil](https://trishampatil.com/ai%20engineering/2026/09/06/mup-maximal-update-parameterization-hyperparameter-transfer/), and [Mohak Chugh](https://mohakchugh.is-a.dev/blog/mup-unit-scaling-hyperparameter-transfer-fp8-training/) remain uncounted where full-body retrieval failed. October news about *Mix, Don't Tune* does not change that already indexed paper's May 13 first-public date.
- Generic geometric optimization, performance-only scaling laws, ordinary cross-dataset transfer, physics μP/Muon matches, automatic summaries and duplicate paper versions remain outside the respective direct collections.

## Validation and Publication

Validation checks the nine paper/resource/artifact table mirrors, exact paper-to-BibTeX URL correspondence (allowing an existing DOI/publisher alias), duplicate keys, shared metadata, balanced records, reverse chronological dates, local file links and section anchors, snapshot counts, and whitespace. Source/evidence reviews distinguish conditional theory, author-reported transfer, implementation changes, and independent performance evidence. The unrelated untracked `docs/hyperball-rms-matching-proof.md` is preserved and excluded from publication.
