# Hyperball Maintenance History

Historical coverage notes and incremental records moved from the root README. Dates and source-status statements retain their original cutoffs; they are not fresh verification.

### Hyperball Search Coverage and Maintenance

The **October 20** incremental audit retains **15 papers / 26 resources** and adds two community implementation/report entries, for **30 artifacts**: [DETR](https://github.com/troydutton/detr/pull/11), merged October 19, and [UFSM](https://github.com/SuperOptimizer/ufsm/commit/4dbf699e37481c7bdba1d8d31465b6548a8d83a1), released October 16. Both implement the normalized radius-scaled sphere wrapper; their reported detection and segmentation comparisons have different schedules or limited tuning/repeats, so they do not establish a general Hyperball advantage. The existing [Marin digest](https://marin.readthedocs.io/en/latest/reports/agent-moe-experiments/) is expanded with the measured-versus-extrapolated distinction from its [four-scale MuonH experiment](https://github.com/marin-community/marin/issues/5596#issuecomment-4422416900), without counting that existing resource twice.

The current search covers public sources through **2026-10-20**, including Hyperball/AdamH/MuonH aliases, all fourteen indexed arXiv submission histories, author/project reports, and relevant GitHub source/PR histories. No new direct Hyperball paper or indexed arXiv revision was verified after October 15. [MarinSkyRL #1002](https://github.com/marin-community/MarinSkyRL/pull/1002), merged October 17 UTC, adds shared global clipping across optimizer groups; [NeMo #312](https://github.com/NVIDIA-NeMo/Emerging-Optimizers/pull/312), opened October 20, remains an unmerged registry-validation repair. Paddle's previously pending [#79844](https://github.com/PaddlePaddle/Paddle/pull/79844) merged on October 15: it adds generic per-group inner-optimizer and mixed-sharding interfaces, not a newly demonstrated Hyperball training result. [rollfast 1.3.0](https://pypi.org/project/rollfast/1.3.0/) was released October 15; its [tagged Hyperball source](https://github.com/RhizomeResearch/rollfast/blob/v1.3.0/src/rollfast/optim/hyperball.py) matches the previously inspected file, while the [release notes](https://github.com/RhizomeResearch/rollfast/blob/v1.3.0/docs/releases/1.3.0.md) concern precision and checkpoint handling. The [nanochat author submission](https://github.com/karpathy/nanochat/pull/498) remains open and unmerged, and Puro's inspected optimizer file has no new change in this interval. Source/tests were reviewed without launching training. Fresh bioRxiv access still returned 403, and arXiv full-text search endpoints were unavailable; indexed searches and direct history checks therefore support a bounded audit, not absolute completeness. Generic Muon work, derivative paper summaries, molecular rendering, granular-ball methods, and graph-centrality HyperBall remain excluded.

The **October 15** incremental audit keeps **15 papers** and adds one official Stanford CS312 learning resource and its implementation/data artifact, for **26 resources / 28 artifacts**. The October 1 course release was an older omission: Lecture 3 and Assignment 2 explain Hyperball and provide 24 single-seed AdamH LR/horizon runs, but the held-out transfer task is an exercise rather than a verified new result. Unofficial derivative course notes are not counted separately.

That search covered public sources through **2026-10-15**, including October 9–15 arXiv histories, author sites/data, and Hyperball/MuonH/AdamH implementation commits and PRs. Of the fourteen indexed arXiv papers, only [The Best Optimizer Depends on Batch Size](https://arxiv.org/abs/2610.08975) had a newly posted version in that interval: v2, October 12. Appendix F.3/F.7 documents repeat/median-based coordinate-descent acceptance, while the [released companion provenance](https://github.com/dangxingyu/batch-size-blog/blob/dcef47e3f1e947e4a853870b5f2d83651f634c9e/batch-size/data/provenance.json) still describes largely single-run Hyperball endpoints and min/max ranges rather than confidence intervals. These evidence layers are not interchangeable; the near-ties and full repeat protocol were not independently reproduced. Its then-accessible [canonical author site](https://batch-size-scaling.com/) was a URL update, not another resource.

Existing artifact entries absorbed two substantive implementation updates: [Paddle #79850](https://github.com/PaddlePaddle/Paddle/pull/79850), merged October 12, separately retracts logical slices of fused weights instead of allowing their individual norms to drift under one combined constraint; [MarinSkyRL #939](https://github.com/marin-community/MarinSkyRL/pull/939), merged October 9 UTC, adds native Megatron MuonH/AdamH parameter grouping and checkpoint continuation. Source and tests were inspected, not run, and neither establishes a new model-quality advantage. Paddle's [release cherry-pick #79851](https://github.com/PaddlePaddle/Paddle/pull/79851) was closed unmerged, so release-branch availability was not asserted. Its [mixed-sharding proposal #79844](https://github.com/PaddlePaddle/Paddle/pull/79844) was still unmerged when checked, before its later October 15 merge recorded above. The older [Dion MuonH proposal #71](https://github.com/microsoft/dion/pull/71) was also closed unmerged and was not counted as upstream Dion support. No new direct paper was verified in that pass; generic Muon changes, automatic paper summaries, graph-centrality HyperBall, and derivative teaching mirrors were excluded. Previously unresolved bioRxiv/Notion/OpenReview bodies were not newly claimed as verified, and indexed retrieval does not establish absolute completeness.

The **October 9** historical audit added a direct Hyperball batch-size study, its author blog and measured-data artifact, and an earlier Marin sharded-norm failure investigation. Its inventory was **15 papers / 25 resources / 27 artifacts**; see the [historical audit in this archive](#hyperball-incremental-audit--2026-10-09).

That October 4–9 search covered arXiv version histories, author source/data, GitHub core-path histories, framework examples/PRs, and primary training-status records. All thirteen previously indexed arXiv papers retained their reviewed versions at that cutoff. The new paper's arXiv v1 was October 6; an earlier author-PDF Git commit did not establish the exact first public posting day. NeMo's merged October 9 nanoGPT example was grouped into its existing framework entry. Palingenesis's October 6 base-Muon change left the Hyperball wrapper unchanged. Marin's October 6 report remained ongoing-run evidence, with cross-expert norm grouping rather than fixed individual-expert radii; nanochat PR #498 was unmerged. The author-blog body was then reviewed through GitHub after live retrieval failed. The 2.1 Notion body returned 404, fresh bioRxiv retrieval failed, and nearby OpenReview candidates presented browser-verification challenges. FiveTechSoft/tinyllama's named MuonH clips an orthogonalized direction but lacks radius-scaled sphere retraction, so it was excluded as an implementation.

The **October 4** audit recovered a MuonH/HyperP looped-LM paper and its official code, reviewed MACRO v2, added a Rochester author-seminar record, and resolved the LR-transfer dataset's paper provenance. Its historical inventory was 14 papers / 23 resources / 26 artifacts; see the [cross-collection audit](search-audit-2026-10-04.md). The looped-LM width/horizon checks do not establish independent depth transfer, and the LR-transfer dataset is research data rather than a newly verified training implementation. Full author-hosted 2.2 source was recovered without establishing its exact publication day.

The **September 24** incremental audit retained the 13-paper / 22-resource / 24-artifact inventory and updated the Palingenesis wrapper to its zero-LR-warmup fix. The LR-transfer dataset was still unattributed at that time; the October 4 pass resolves it through the indexed paper's author link. See the [historical cross-collection audit](search-audit-2026-09-24.md).

The September 23 search covered public sources through **2026-09-23**, including arXiv version histories and full text, bioRxiv metadata, OpenReview records, author and institutional pages, GitHub source/logs/PRs, and Hugging Face releases. It followed Hyperball, AdamH, MuonH, SGDH, NorMuonH, AdEMAMixH, SOAPH, sphere-constrained Muon variants, HyperP, HyperTransfer, and the Fantastic Pretraining Optimizers II / 2.1 / 2.2 aliases. That pass verified unchanged latest versions for all 12 then-indexed arXiv papers, recovered six learning resources and nine implementation entries, and expanded existing model/data links. The bioRxiv page could not be re-read in that pass; its metadata was retained from the earlier audit.

See the [full search audit](hyperball-search-audit-2026-09-14.md), [September 19 incremental audit](hyperparameter-transfer-search-audit-2026-09-19.md), [September 20 correction audit](hyperball-search-audit-2026-09-20.md), and [September 23 source audit](hyperball-search-audit-2026-09-23.md) for inclusion decisions, version issues, and unresolved artifacts. Graph-centrality HyperBall, geometric ball packing, generic “hyperball” terminology, games, mirrors, and automatic paper summaries are outside this optimizer collection. Follow [CONTRIBUTING.md](../../CONTRIBUTING.md) when adding entries.

<a id="hyperball-source-audit"></a>

### Hyperball Source Audit — 2026-09-23

This audit extends the [September 20 inventory](hyperball-search-audit-2026-09-20.md) through **2026-09-23**, combining paper, learning-resource, and source-code checks.
The inventory contains **13 papers**, **22 learning resources / reports**, and **24 implementation / artifact families**.
No additional qualifying research paper was verified; six resources and nine implementation families were recovered, including an upstream framework integration merged on September 21.

| Collection | Previous | Added | Current |
|---|---:|---:|---:|
| Research papers, including the labeled MD Decoupling extension | 13 | 0 | 13 |
| Learning resources / reports | 16 | 6 | 22 |
| Implementation / artifact families | 15 | 9 | 24 |

All nine new implementation families belong in the main artifact index; their inclusion does not imply equivalent maturity, identical norm conventions, or demonstrated training gains.
Puro and Delphi releases/data, Segurant, and additional Track 3 submissions enrich existing families without increasing their count.
The review inspected public records and source; it did not execute third-party tests or reproduce training. Project-reported measurements remain attributed results.

#### Paper Search and Version Check

No qualifying paper first public from **September 8 through September 23** was verified in this pass.
The twelve existing arXiv records were reopened and their official **Submission history** checked; all latest versions match the previous inventory.
Mirror crawl dates and “LastUpdated” labels were not treated as manuscript revisions.

| Existing paper | Latest version verified |
|---|---|
| [HyperTransfer](https://arxiv.org/abs/2609.07017) | v1, 2026-09-07 |
| [Curvature-Conditioned Multiscale Momentum with Sphere Constraints for LLM Pretraining](https://arxiv.org/abs/2608.28442) | v1, 2026-08-28 |
| [Puro-2B](https://arxiv.org/abs/2608.27370) | v2, 2026-09-03 |
| [Effective Learning Rate Governs Loss Dynamics in Language Model Pretraining](https://arxiv.org/abs/2608.24814) | v1, 2026-08-25 |
| [Hyperball May Not Be a Free Lunch](https://arxiv.org/abs/2607.22444) | v1, 2026-07-24 |
| [On the Nonlinearity of Learning Rate Scaling for LLM Training](https://arxiv.org/abs/2606.29158) | v1, 2026-06-28 |
| [Improving Neural Network Training by Decoupling the Magnitude and Direction of Weight Vectors](https://arxiv.org/abs/2606.25971) | v2, 2026-07-17 |
| [Fantastic Pretraining Optimizers and Where to Find Them II: Hyperball Optimization](https://arxiv.org/abs/2606.16899) | v1, 2026-06-15 |
| [Redesign Mixture-of-Experts Routers with Manifold Power Iteration](https://arxiv.org/abs/2606.12397) | v1, 2026-06-10 |
| [Demystifying Manifold Constraints in LLM Pre-training](https://arxiv.org/abs/2605.04418) | v1, 2026-05-06 |
| [Rethinking Language Model Scaling under Transferable Hypersphere Optimization](https://arxiv.org/abs/2603.28743) | v2, 2026-04-05 |
| [Manifold constrained steepest descent for smooth and closed-set optimization](https://arxiv.org/abs/2601.21487) | v2, 2026-08-13 |

HyperTransfer remains September 7 v1 despite a mirror displaying September 9 as an update date.
The [bioRxiv DNA paper](https://www.biorxiv.org/content/10.64898/2026.07.17.739267v1) could not be freshly read, so its version status was **not** independently reverified.
Its July 22 posting date is retained from the earlier audit; the July 17 date embedded in the DOI is not substituted for the posting date.

#### Six Added Learning Resources / Reports

##### FAI-Seminar author talk — 2026-07-24

The [official FAI schedule](https://www.fai-seminar.ac.cn/FAI/previous.html) lists Kaiyue Wen's Chinese-language **Fantastic Pretraining Optimizers and Where to Find Them II: Hyperball Optimization** talk.
The row links the paper, announcement, and official Bilibili recording. It is a separate event from the existing Yao Class, Princeton, and IOS materials.
The recording was not watched; Bilibili/WeChat targets failed extraction, so the verified schedule is the stable entry point.

##### Optimization 1 — Norm reparametrization — 2026-01-23

[Ziming Liu's tutorial](https://kindxiaoming.github.io/blog/2026/optimization-1/) explicitly starts from Hyperball's fixed-radius motivation and explores learned magnitude through a two-dimensional Adam/MSE example and Colab.
It is a related norm-direction extension, not an AdamH/MuonH LLM benchmark or a general counterexample for scale-invariant architectures.

##### Marin expert-group norm investigation — 2026-08-24

[Issue #8621](https://github.com/marin-community/marin/issues/8621) documents cross-expert Frobenius normalization for stacked `[layers, experts, in, out]` tensors while Newton–Schulz acts on individual expert matrices.
An [August 25 comparison](https://github.com/marin-community/marin/issues/8621#issuecomment-5406386634) reports d768 Paloma macro loss **3.015 with grouped experts versus 3.018 per expert**, described by the maintainer as within noise.
The implementation and interpretation correction is detailed below; the issue is primary engineering evidence rather than a formal paper.

##### Agnes 2B pretraining protocol — 2026-09-03

The Agnes Foundation Model Team's [report](https://github.com/AgnesAI-Labs/Agnes-2B-Pretraining/blob/main/REPORT.md) specifies selected-matrix MuonH with AdamW fallbacks, FP8, and a two-stage curriculum for a proposed 2.032B dense model.
The date follows the [first publication commit](https://github.com/AgnesAI-Labs/Agnes-2B-Pretraining/commit/43a36a9179c02d0025f0aad3d9a3a43201c3b1cc); the document labels itself Technical Report V1.1, September 2026.
It explicitly describes a falsifiable protocol: training time, capability targets, and release floors are projections, reference-system measurements, or acceptance criteria, **not completed Agnes training results**.
The repository supplies PDF/Markdown; no runnable full training release was independently verified.

##### Blog: Survey of Optimizers — 2026-08-28

[Ruoran Xu's arXiv survey](https://arxiv.org/abs/2608.28557) remains v1. [Section 5.2](https://arxiv.org/html/2608.28557v1#S5.SS2) explains Hyperball through weight norms and angular learning rates; Sections 4.3, 10.5, and 14.3 add geometry and evaluation context.
This is substantive explanatory coverage, but its Hyperball results summarize prior work; count it as a resource rather than another independent experiment or research-paper entry.

##### MACRO companion seminar at Johns Hopkins — 2026-09-14

The [institutional event page](https://publichealth.jhu.edu/events/2026/09/14/biostatistics-dept-seminar-demystifying-manifold-constraints-in-llm-pre-training) identifies Shiqian Ma's **Demystifying Manifold Constraints in LLM Pre-Training** seminar.
Its connection is the already-indexed MACRO comparison paper. The abstract discusses constraints, RMS normalization, and rotational equilibrium but does not itself name Hyperball.
No public recording or slides were verified; this entry is an author seminar record, not a claim about viewed video content.

#### Nine Added Implementation / Artifact Families

##### 1. PaddlePaddle upstream Muon / Adam Hyperball

[PR #79792](https://github.com/PaddlePaddle/Paddle/pull/79792) merged on **2026-09-21**, merge commit `8a0db499edcfa4ce6a54bc1d5d94c32f67e4da3c`.
Inspected [Muon source](https://github.com/PaddlePaddle/Paddle/blob/7c722e55099a9750186922c5158f9b78f723f168/python/paddle/optimizer/muon.py#L666) routes `use_hyperball` and `use_muon` to MuonH, AdamH, ordinary Muon, or AdamW.
`_hyperball_apply` computes FP32 radius and normalized update, steps by `lr * radius`, then reprojects; Hyperball routes omit weight decay. Trailing-two-axis norms distinguish full 2D matrices and individual 3D expert matrices.
The [sharding implementation](https://github.com/PaddlePaddle/Paddle/blob/7c722e55099a9750186922c5158f9b78f723f168/python/paddle/distributed/fleet/meta_optimizers/muon_sharding_optimizer.py#L327) keeps Muon/Hyperball tensors whole on an owner rank.
Radius is recomputed rather than saved as an initialization checkpoint; exact preservation remains subject to epsilon and rounding. A merged integration does not establish availability in every released Paddle version.

##### 2. HeavyBall HyperBallAdamW

The [public class](https://github.com/HomebrewML/HeavyBall/blob/44b4bb46483da14d81f2417c2bb39e0c9da42fb3/heavyball/__init__.py#L414) cites the Hyperball note and routes rank-two-or-higher tensors to Hyperball, with ordinary AdamW for vectors.
The [actual update](https://github.com/HomebrewML/HeavyBall/blob/44b4bb46483da14d81f2417c2bb39e0c9da42fb3/heavyball/utils.py#L3736) normalizes the direction, steps by learning rate times stored initial norm, and projects back, with precision promotion and stochastic copy-back.
Default weight decay is zero; optional decay/cautious masking changes the direction before normalization. Higher-rank tensors use whole-tensor norms, not automatic per-expert constraints.

##### 3. rollfast Hyperball transforms

[Pinned source](https://github.com/RhizomeResearch/rollfast/blob/c232e58bdc8d0caeeadc2413e9f1080e4fd41ecb/src/rollfast/optim/hyperball.py) stores initial L2 norms in `HyperballState`, uses FP32/named-axis reductions, and applies normalized direction, radius-scaled step, and fixed-radius projection as a terminal Optax transform.
The [README](https://github.com/RhizomeResearch/rollfast/blob/c232e58bdc8d0caeeadc2413e9f1080e4fd41ecb/README.md) and [tests](https://github.com/RhizomeResearch/rollfast/blob/c232e58bdc8d0caeeadc2413e9f1080e4fd41ecb/tests/test_hyperball.py) expose AdamW, Muon, PRISM, RMNP, Kron, Aurora, and Riemannian-Aurora compositions, masks, and separate fallback learning rates.
[PyPI](https://pypi.org/project/rollfast/) supplies a distribution entry. Optional nonzero decay/caution changes the underlying direction; no LLM superiority benchmark was verified.

##### 4. MarinSkyRL PyTorch port

[PR #249](https://github.com/marin-community/MarinSkyRL/pull/249) merged on **2026-08-03**; [source at the PR head](https://github.com/marin-community/MarinSkyRL/blob/339039e5468b9c0f1dab00882cd70a95f22de3ae/skyrl-train/skyrl_train/distributed/grug_muonh.py) implements MuonH hidden/expert matrices, AdamH output head, and ordinary Adam auxiliary groups.
MuonH/AdamH share a learning-rate track; ordinary Adam has another. `_hyperball_delta` uses trailing matrix axes and DTensor-aware materialization, with FP32 state and BF16 compute.
The port rejects expert parallelism above one, nonzero weight decay, and unsupported options. The PR reports JAX-oracle, FSDP2/checkpoint, and four-H100 lifecycle checks, not an RL-quality comparison.
This is Marin's SkyRL fork; it is not evidence of upstream SkyRL support.

##### 5. Author nanochat MuonH submission

[dangxingyu's PR #498](https://github.com/karpathy/nanochat/pull/498) is **unmerged**. [Pinned optimizer code](https://github.com/karpathy/nanochat/blob/330fa1188c4dfd41345307cb90b71c06b4b37dcd/nanochat/optim.py) supplies cached initial norms and `hyperball_step_fused` around a NorMuon direction.
The submission links [FP8](https://wandb.ai/xingyu20/nanochat/runs/uocq5uxw) and [BF16](https://wandb.ai/xingyu20/nanochat/runs/5f40sch5) runs plus [schedule discussion #499](https://github.com/karpathy/nanochat/discussions/499).
It also changes parameterized RMSNorm, zero-initialized vector output multipliers, matrix learning-rate depth scaling, and separate cooldowns: this is a recipe comparison, not an isolated optimizer swap.
Reported d24/8-H100 results include 167.91 minutes and CORE 0.2645; these were not reproduced. Keep it distinct from the already-indexed January 29 d12 negative experiment.

##### 6. ANCORA / ancora-cutile

The [Hyperball kernel and NumPy oracle](https://github.com/MythosAd/ancora-cutile/blob/835ff0da33ec22c82ea08ace30cc4c23453164ba/ancora/optim/hyperball.py), [AdamH head](https://github.com/MythosAd/ancora-cutile/blob/835ff0da33ec22c82ea08ace30cc4c23453164ba/ancora/optim/adamh.py), and [MuonH wiring](https://github.com/MythosAd/ancora-cutile/blob/835ff0da33ec22c82ea08ace30cc4c23453164ba/ancora/optim/muon.py) establish the implementation beyond a README claim.
It uses CUDA Tile/device-resident updates, FP32 master/norm arithmetic, BF16 views, per-expert constraints, MuonH hidden/expert matrices, AdamH untied head, and ordinary Adam auxiliary groups.
The [README](https://github.com/MythosAd/ancora-cutile/blob/835ff0da33ec22c82ea08ace30cc4c23453164ba/README.md) describes a systems candidate with single-GPU Windows 11/CUDA 13.3/sm_120a validation, no stable package/API, and limited performance measurements rather than matched multi-seed quality evidence.

##### 7. Tiny Shakespeare Hyperball sandbox

[Optimizer source](https://github.com/JiHa-Kim/tinyshakespeare-gpt/blob/f5c3fc7dc4a6163a4b8c787da70c8032b60affc3/scionh/optim/scion.py) stores RMS radii and converts them to Frobenius radii with `sqrt(numel)` for normalized stepping and retraction.
Its configurable ULMO directions default to hidden Gram Newton–Schulz. Default `retract` follows the wrapper geometry for that direction; optional `slerp` tangent-projects and uses an exponential-map update, a different variant.
The [parameterization note](https://github.com/JiHa-Kim/tinyshakespeare-gpt/blob/f5c3fc7dc4a6163a4b8c787da70c8032b60affc3/docs/hyperball_parametrization.md) supports its role as a community learning/experimentation artifact, not an official reproduction.

##### 8. Palingenesis fine-tuning wrapper

[Current source](https://github.com/mii-llm/palingenesis/blob/8cccefc0c3ab0eb3b5b8f98a74c18f9b1d50ed7d/src/palingenesis/optim.py#L483) snapshots parameter buckets, disables decay, obtains a direction from the base optimizer displacement, normalizes it, and applies an angular step plus initial-radius projection.
[Tests](https://github.com/mii-llm/palingenesis/blob/8cccefc0c3ab0eb3b5b8f98a74c18f9b1d50ed7d/tests/test_optim_and_health.py) accompany the implementation. Positive `angular_lr` selects a common rate; zero calibrates a separate rate for each matrix from its first base update.
Some prose still describes projection alone, so cite source for semantics. Repeated 20–30% claims derive from the original pretraining paper, not a verified fine-tuning gain.

##### 9. Chess-engine-4 application and mixed-result report

[Pinned source](https://github.com/maxencefrenette/chess-engine-4/blob/60b898c664e56834d3b30eeb2f0df9d736a377f1/src/chess_engine_4/training/optimizers.py) implements AdamHyperball with stored FP32 initial radii, radius-scaled updates, retraction, and zero-update handling; other parameter families use Adam.
The [experiment report](https://github.com/maxencefrenette/chess-engine-4/blob/60b898c664e56834d3b30eeb2f0df9d736a377f1/experiments/2026-08-11.03-hyperball/README.md) includes per-arm runs, controls, paired seeds, and width/token-budget studies.
Its initial d128 noninferiority gate failed (+0.01383/+0.01136 loss versus the best light AdamW); later d256/d512 arms favored AdamH, but [learning rates remained width-indexed](https://github.com/maxencefrenette/chess-engine-4/blob/60b898c664e56834d3b30eeb2f0df9d736a377f1/experiments/2026-08-11.04-adamh-16d-learning-rates/README.md).
Larger-width MXFP8 runs had spikes and MoE validation remained outstanding. This is useful community application evidence, not unchanged-learning-rate transfer or a formal paper.

#### Existing Families Expanded Without Double-Counting

- **Dragon / Segurant:** [Segurant AdamH](https://github.com/OVHai-LLM/Segurant/blob/3f07101cc3e80592b4d38851b395fcb37dfab57a/optimizers/adamh.py) and [AdEMAMixH](https://github.com/OVHai-LLM/Segurant/blob/3f07101cc3e80592b4d38851b395fcb37dfab57a/optimizers/ademamixh.py) implement normalized updates and retraction, with 2D or per-first-axis-slice norms; group these training sources with the existing Dragon lineage.
- **Track 3:** the [pinned ledger](https://github.com/KellerJordan/modded-nanogpt/blob/bc3a0c2d640d0d73dedaef87eae26148d2e32afb/records/track_3_optimization/README.md) additionally links [#277 NorMuonH outer updates](https://github.com/KellerJordan/modded-nanogpt/pull/277), [#293 KL-SOAP-H](https://github.com/KellerJordan/modded-nanogpt/pull/293), [#302 SOAP-H](https://github.com/KellerJordan/modded-nanogpt/pull/302), [#316 PSGD + Hyperball](https://github.com/KellerJordan/modded-nanogpt/pull/316), and [#324 MuonH auxiliary tuning](https://github.com/KellerJordan/modded-nanogpt/pull/324).
- The latter three report 3125 steps/n=6, 3375/n=5, and 3250/n=10 respectively. #324 preserves matrix LR but retunes auxiliary parameters; these remain recipe-level records within one collection. No later Hyperball row than #324 was found in the inspected ledger.
- **Delphi:** [public blog data](https://huggingface.co/datasets/marin-community/delphi-blog-data) exposes six configurations and 4,117 rows, including 102 `delphi-ladder` and 3,737 `hparam-scaling` rows, fits, held-out validation, and per-row W&B links. The [447M / 122B-token model card](https://huggingface.co/marin-community/delphi-3e20-447Mparams-122Btokens) identifies AdamH and the Complete(d)-inspired recipe; both enrich the existing recipe entry.
- **Puro:** the [author collection](https://huggingface.co/collections/thu-pacman/puro-2b) supplies base/phase checkpoints and curriculum/averaging inputs, alongside [materialized training data](https://huggingface.co/datasets/thu-pacman/Puro-2B). A root-review follow-up read the [base-model card using a query-parameter URL](https://huggingface.co/thu-pacman/Puro-2B-Base?hardware=rtx-5080) after the clean URL failed extraction; it identifies random initialization and MuonH. These releases enrich the Puro-Megatron entry without another artifact count.
- **Marin / Levanter:** the [standalone AdamH source](https://github.com/marin-community/levanter/blob/982cef7f1d8d1a642b825fcd30ab1b44a912f478/src/levanter/optim/adamh.py) is the same implementation lineage. Individual agent experiment issues are not separate papers or implementation families.

#### Cross-Expert Norm Correction and Live-Run Status

Marin's 535B stack must be described as using **recipe-specific cross-expert norm groups**, not as independently fixing every expert matrix to its own initial Frobenius norm.
The [maintainer's acknowledgment](https://github.com/marin-community/marin/issues/8621#issuecomment-5399971477) says this grouping was unintended but retained after earlier comparisons; the [author response](https://github.com/marin-community/marin/issues/8621#issuecomment-5404682748) accepts retaining it while monitoring individual expert norms.
The 3.015-versus-3.018 d768 result does not establish a robust advantage. An [August 31 stability update](https://github.com/marin-community/marin/issues/8621#issuecomment-5482266030) is an intermediate observation.
The [hero tracker](https://github.com/marin-community/marin/issues/8435) remained open, last updated 2026-09-22 22:33 UTC; nothing inspected establishes completed 18T-token training by the cutoff.
The [agent digest](https://marin.readthedocs.io/en/latest/reports/agent-moe-experiments/) explicitly summarizes 80 experiments as of August 20, so it must not be presented as covering all September issues.

#### Hyperball Exclusions and Unresolved Candidates

- **Author repository name is insufficient:** `dangxingyu/Megatron-LM-Hyperball` had only `main`, with a complete non-truncated tree at `622a06af26348c999848531c2fa231507245e809`; code search found no Hyperball. Its [Muon source](https://github.com/dangxingyu/Megatron-LM-Hyperball/blob/622a06af26348c999848531c2fa231507245e809/megatron/core/optimizer/muon.py) is ordinary tensor-parallel Muon. The verified author addition is nanochat PR #498.
- **PyTorchOptimizer:** scoped search found no Hyperball source; generic Muon support does not establish AdamH/MuonH support. Existing [NeMo MuonHyperball](https://github.com/NVIDIA-NeMo/Emerging-Optimizers/blob/6117b1bfb1e196643c57ddd39d22f499fc871436/emerging_optimizers/orthogonalized_optimizers/muon_hyperball.py) remains valid but requires an explicit matching nonzero radius.
- **Different geometry:** [Odyssey](https://github.com/HomericIntelligence/Odyssey/blob/a8e1038001fa086a7df30874634b16a4dce7c977/src/odyssey/training/optimizers/muon_hyperball.mojo) uses one-sided ball clipping; [koochak](https://github.com/jamaliki/koochak/blob/2d69bae139c59be113ca361b3e663cb98f739b5f/koochak/optim/muon.py#L106) projects after an ordinary optimizer step without the normalized radius-scaled direction. Neither is the exact original wrapper.
- **Configurable toolkit:** [MODULUS](https://github.com/grandchallenge/MODULUS/blob/9fc42eb5f29d5fff396f13e1a6c972af8fe64b35/modulus/optim/hyperball.py) offers sphere/ball/tangent/target-angle variants and scalar/row/column/leaf choices; its defaults do not automatically reproduce initialization-radius whole-matrix Hyperball.
- **Code-only candidate:** [optimstep matrix-step source](https://github.com/optimstep/optimizer-experiments/blob/d35a48a44a7c07addc43f497e603ee51e8e43b3f/experiments/train_gradcache_qwen3.py#L600) normalizes a base optimizer displacement and reprojects; base decay/options and a possible subsequent anchor-decay branch matter. No published result was verified, so retain it as supplementary rather than a counted benchmark family.
- **AdamH name collision:** [FRAMES-VQA](https://github.com/chengyuehuang511/FRAMES-VQA/blob/4bc58c6dcacd14dd068b6ec5ed5a2ed579b5515d/optimizer/adamh.py), also copied into `vlm_robustness`, uses Adam with conditional decay toward initialization, not fixed-radius projection.
- **Copied records:** optimizer-fingerprints, nanogpt-optimizer-benchmark, and many nanochat/modded-nanogpt forks expose copied benchmark files; copies are not independent implementation evidence.
- **Adjacent theory:** [Weight-norm Criticality](https://arxiv.org/html/2607.21005v1) discusses norm shrinkage and loss spikes but has no Hyperball/Muon text match or established direct comparison. Existing nGPT, SSO, Mano, Nora, PC Layer, AngularMuown, and OmniOpt classifications were not upgraded without new direct evidence.
- **Unconfirmed paper:** [OpenReview 39sAm5aTYZ](https://openreview.net/pdf?id=39sAm5aTYZ) appears in search with a Hyperball bibliography entry, but browser challenges/API failures prevented title and substantive-body verification; a citation alone is insufficient.
- **Other collisions:** graph-centrality HyperBall/HyperANF, hyperbolic packing, conceptual-space balls, Deep SVDD boundaries, cryptographic sampling, games/paintball, Mien `muonh`, and unrelated SGDH/ADAMH/AdamHD abbreviations are excluded. No distinct official SGDH release was established.
- Automatic summaries, translations, social mirrors, and speculative “MuonH Stiefel ERM” descriptions without an identified primary paper do not supply independent evidence. Nearly constant RMS in released weights, including Muse Glimmer observations, cannot establish the optimizer used.

#### Access Limits and Uncounted Resource Leads

- **Tencent ELR:** [official page](https://hy.tencent.ai/research/elr) and its English variant failed extraction; direct access returned HTTP 567. A [public bookmark](https://github.com/AkihikoWatanabe/paper_notes/issues/6289), translations, and HyperTransfer's bibliography corroborate existence, not a fresh primary-body review. The reported August 5 date remains unconfirmed; no translation is counted separately.
- **Author material:** the Hyperball 2.1 redirect reached a Notion URL that returned 404 here, and 2.2 yielded no extractable body. IOS Google Slides export was inaccessible; its prior content description is inherited, not freshly reviewed. These failures do not establish withdrawal.
- **Princeton recording:** the official event linked [Xingyu Dang — PLI Lunch Series 20260205](https://drive.google.com/file/d/1vqllSlJMhrabWm57sSnvJLhvttm8RvQd/view?usp=sharing); add it to the existing entry, but the recording was not watched.
- **Unreleased/future talks:** Kaiyue Wen's [homepage](https://whenwen.github.io/) records an April 2 Kimi talk without verified public media. The [September 30 Rochester MACRO event](https://events.rochester.edu/event/demystifying-manifold-constraints-in-llm-pre-training) is future-dated at this cutoff. Neither increases completed accessible resources.
- **Snowball application:** the [August 16 SFT report](https://storage.googleapis.com/marin-public/benjaminfeuer/standing-up-a-cold-start-sft-pipeline-for-marin-models/2026.08.16/index.html) does not identify its optimizer in the inspected body; related implementation routing does not isolate Hyperball's contribution to its reported gains.

#### Search Coverage and Interpretation

Paper discovery combined exact Hyperball/AdamH/MuonH/HyperP/HyperTransfer queries, September date restrictions, arXiv/OpenReview searches, and full-text follow-up on sphere constraints, effective learning rate, and weight norms.
All twelve arXiv version histories were checked directly; new candidate inclusion required primary metadata and a substantive section, experiment, or implementation connection.
Resource searches covered English/Chinese author names and combinations with seminar, lecture, slides, video, Bilibili, tutorial, Tencent ELR, Open Athena, Marin, and Snowball.
Existing author pages, institutional schedules, blogs, issue discussions, and the Marin digest/hero tracker were revisited, including recent September issue searches.
GitHub discovery used Hyperball, MuonH, AdamH, NorMuonH, AdEMAMixH, and SOAPH, followed by repository-scoped code, PR metadata, branches, commit histories, benchmark ledgers, and Hugging Face collections.
Global AdamH/SOAPH queries were noisy, making source-level scoped follow-up essential. Parameter routing, norm axes, initialization radius, normalized direction, projection, and optional decay determined implementation classification.
This is a reproducible account of inspected public evidence, **not a guarantee of exhaustive coverage** of private repositories, unindexed forks, inaccessible pages, or newly published material.

### Hyperball Incremental Audit — 2026-09-24

This incremental audit found no additional verified Hyperball paper or independent implementation family beyond the September 23 inventory. The collection therefore remains at **13 papers, 22 learning resources/reports, and 24 implementation/artifact entries**; the substantive change is a corrected source pin and implementation warning for an existing artifact. These are collection counts, not evidence that every relevant publication has been discovered. See the [collection and counting conventions](../../docs/hyperball.md) and [preceding audit](hyperball-search-audit-2026-09-23.md).

**Paper-version refresh.** All twelve indexed arXiv records were reopened; their latest versions remained unchanged: [HyperTransfer, v1](https://arxiv.org/abs/2609.07017v1); [MuonM, v1](https://arxiv.org/abs/2608.28442v1); [Puro-2B, v2, September 3](https://arxiv.org/abs/2608.27370v2); [Effective Learning Rate, v1](https://arxiv.org/abs/2608.24814v1); [Free Lunch, v1](https://arxiv.org/abs/2607.22444v1); [Nonlinear LR Scaling, v1](https://arxiv.org/abs/2606.29158v1); [Magnitude–Direction Decoupling, v2, July 17](https://arxiv.org/abs/2606.25971v2); [original Hyperball paper, v1](https://arxiv.org/abs/2606.16899v1); [MPI Routers, v1](https://arxiv.org/abs/2606.12397v1); [MACRO, v1](https://arxiv.org/abs/2605.04418v1); [HyperP, v2, April 5](https://arxiv.org/abs/2603.28743v2); and [MCSD, v2, August 13](https://arxiv.org/abs/2601.21487v2). The thirteenth paper, the [regulatory-DNA comparison](https://doi.org/10.64898/2026.07.17.739267), could not be re-read because the publisher returned HTTP 403; no fresh version claim is made for it.

**Palingenesis correction.** Commit [`ea1c0a8`](https://github.com/mii-llm/palingenesis/commit/ea1c0a89a2898ce5a062a0dac7fdf05702c7f99d), timestamped **2026-09-23 22:51:05 UTC** (September 24 in Beijing), fixes automatic angular-rate calibration during zero-LR warmup. Previously, `angular_lr=0` could calibrate a matrix's effective rate from a zero displacement and subsequently freeze its constrained updates. The [inspected implementation](https://github.com/mii-llm/palingenesis/blob/ea1c0a89a2898ce5a062a0dac7fdf05702c7f99d/src/palingenesis/optim.py#L594) now postpones calibration until both the scheduled multiplier and proposed displacement are nonzero. Thereafter it normalizes the base-optimizer displacement, scales the step relative to the fixed radius, and projects back to that radius. This is a correction to an already indexed community wrapper, not a new algorithm or benchmark result. The [regression test](https://github.com/mii-llm/palingenesis/blob/ea1c0a89a2898ce5a062a0dac7fdf05702c7f99d/tests/test_checkpoint_optimizer.py#L115) checks parameter movement and radius preservation following warmup; its source was inspected, **but the test was not executed**. No independent fine-tuning gain or end-to-end checkpoint-restoration guarantee was established.

**Unresolved dataset lead.** The [LR-Transfer-Trajectory dataset card](https://huggingface.co/datasets/neurips2026lrtransfer/LR-Transfer-Trajectory/blob/main/README.md) describes twelve-layer GPT width/LR sweeps and loss, gradient/update/weight-norm, and alignment trajectories. Its [directory tree](https://huggingface.co/datasets/neurips2026lrtransfer/LR-Transfer-Trajectory/tree/main/LRTransferData) includes Adam, AdamW, and AdamH runs under WS/WSD schedules. However, the inspected card does not identify an associated paper or attributable training-code release, nor establish that its `AdamH` label implements the specific Hyperball wrapper. It remains a discovery lead outside the verified counts. The approximately 3.84-GB archive was not downloaded, and trajectory contents were not independently reproduced.

**Exclusions and limits.** The similarly named [`arclabs561/hyperball`](https://github.com/arclabs561/hyperball) provides Rust Poincare-ball/Lorentz geometry primitives; it is not the fixed-Frobenius-norm optimizer wrapper and is excluded. Related optimizer-memory work belongs in the scale-aware HPT collection when it derives transfer rules, rather than becoming a Hyperball paper solely through a related-work citation; [Optimizer Memory Schedules](https://arxiv.org/html/2609.04577) is one such distinction. Search results, repository names, and optimizer labels alone were not treated as implementation verification.

### Hyperball Incremental Audit — 2026-10-04

The October 4 audit searched September 24–October 4 updates and checked backward omissions against the 13-paper / 22-resource / 24-artifact baseline. It verified no new qualifying Hyperball paper first posted in that interval, but recovered [How Much Is One Recurrence Worth? Iso-Depth Scaling Laws for Looped Language Models](https://arxiv.org/abs/2604.21106), first posted April 22 and reviewed at v3, May 7. The paper uses matrix MuonH with μP/HyperP rules and AdamW auxiliary groups. Appendix D checks width transfer at dimensions 512/640/896, with a two-LR sanity check at 1152, and token-horizon transfer over T/P = 10/20/40. These are fixed-effective-depth experiments, not an independent depth-transfer result or isolated optimizer comparison. Its [official code](https://github.com/kschwethelm/looped-lm-scaling/blob/ad7d8f3055501e4c84ced7797f772dce2f8e2107/nanochat/optim.py) normalizes the Muon direction, scales it by cached initial matrix norms, and reprojects after the update; implementation tests were inspected, not executed. The paper and code also qualify for the μP collection.

[MACRO](https://arxiv.org/abs/2605.04418) changed to v2 on September 28. The main comparison table now reports Qwen3 models at 240M/600M/1.2B, 40 tokens per parameter, and five seeds, comparing Frobenius/spectral MuonH variants with MACRO and other constrained optimizers. Older appendix captions retain inconsistent model sizes, so the catalog follows the main table. Its new BF16 analysis compares MACRO with Muon and should not be attributed to MuonH. The other eleven indexed arXiv papers retained their previously reviewed versions. The [Rochester September 30 author-seminar record](https://events.rochester.edu/event/demystifying-manifold-constraints-in-llm-pre-training) adds one companion learning resource; its scheduled date has passed, but no recording or slides were verified.

The formerly unresolved [LR-Transfer-Trajectory dataset](https://huggingface.co/datasets/neurips2026lrtransfer/LR-Transfer-Trajectory) is now attributable: [On the Nonlinearity of Learning Rate Scaling](https://arxiv.org/html/2606.29158v1) author-links it in the Introduction, and Appendix B specifies normalized, radius-scaled AdamH with fixed-radius retraction. The release contains Adam/AdamW/AdamH width–LR trajectories and norm/alignment statistics. It adds a paper-associated data artifact, not a newly verified training implementation; the 3.84GB archive and inaccessible anonymous code were not reproduced.

The original author-note lineage was rechecked. The [2.1 URL](https://whenwen.github.io/wd_blog/public/hyperball-part-1.html) still redirects to Notion, whose live body could not be freshly read. The full [2.2 theory tutorial source](https://github.com/WhenWen/WhenWen.github.io/blob/1f21d2d782f25fbe68f40aec743231e9b68f258b/wd_blog/public/weight-decay-part-2.html) and historical combined note were recovered; no interval update or exact 2.2 publication date was established. Targeted Paddle/NeMo/HeavyBall/rollfast/Marin optimizer-path checks found no interval changes. [Palingenesis's September 25 release](https://github.com/mii-llm/palingenesis/commit/8875f32726c893c78a400a0b9d13b1d7a9452a3e) reformatted its wrapper without replacing the already-indexed warmup fix. [Marin's 535B report](https://github.com/marin-community/marin/issues/8435#issuecomment-5922369256) remains ongoing, and [nanochat PR #498](https://github.com/karpathy/nanochat/pull/498) remains unmerged. BioRxiv fresh access still failed. [Graph-autoencoding “hyperball cloud” work](https://arxiv.org/abs/2609.35527) was excluded as a name collision. Current totals are 14 papers / 23 resources / 26 artifacts; indexed search and these access limits do not establish absolute completeness.

### Hyperball Incremental Audit — 2026-10-09

The October 4–9 public-source pass adds [The Best Optimizer Depends on Batch Size](https://arxiv.org/abs/2610.08975), its companion author blog, and its measured-data/interactive artifact, and recovers an earlier Marin implementation-failure investigation. The current collection contains **15 papers, 25 resources, and 27 artifact entries**. The original author note, living 2.1/2.2 notes, and formal Hyperball paper remain one research lineage. Shared HyperP, MACRO, and looped-LM entries overlap with the μP collection; the new batch-size study is cross-linked from complementary HPT rather than duplicated there.

The new paper's Section 4/Figure 3 and Appendix F directly test AdamH, LionH, MuonH, KL-SOAP-H, and ShampooH at 128K–2M tokens per update under an approximately 1.7B-token budget. Each configuration is retuned at each batch. KL-SOAP-H has the lowest reported loss at smaller batches, while ShampooH wins at 2M; their 1M difference is only about 0.0003 nats. Most Hyperball endpoints are single-seed. Thus this is evidence of batch-dependent optimizer rankings, not a universal successful batch-transfer rule or a claim that every crossover is statistically resolved. The author artifact preserves measured endpoints, rule grids, provenance, and explanatory simulations; no full training-code release was independently verified. ArXiv v1 was submitted October 6; an earlier author-PDF Git commit does not establish its exact public posting day.

All thirteen previously indexed arXiv version histories were rechecked, with no interval revisions. MACRO remains September 28 v2, HyperP April 5 v2, the looped-LM study May 7 v3, and HyperTransfer September 7 v1. The looped-LM width/horizon checks remain conditional on effective depth and do not establish independent depth transfer. Fresh bioRxiv retrieval failed. The 2.1 route still redirects to a Notion page whose body returned 404; the author-hosted 2.2 source remains available at its earlier inspected pin, with no verified exact publication day.

NeMo's [October 9 nanoGPT example](https://github.com/NVIDIA-NeMo/Emerging-Optimizers/pull/308) adds a MuonHyperball-capable harness with hidden-matrix radius initialization, AdamW auxiliary groups, checkpoint resume, and DDP/mixed precision. It does not change the inspected core wrapper or establish benchmark gains. Paddle's [per-group sharding prototype](https://github.com/PaddlePaddle/Paddle/pull/79844) remains unmerged. Palingenesis's October 6 change replaces its base Muon implementation without changing the Hyperball wrapper; the zero-LR-warmup repair remains the relevant fixed pin. Targeted Marin, Paddle, NeMo, HeavyBall, rollfast, and author-note core-path histories otherwise show no interval changes. Nanochat PR #498 remains unmerged.

Marin's [October 6 status](https://github.com/marin-community/marin/issues/8506#issuecomment-6025364160) reports resumed 535B training at step 215756 after an infrastructure retry, not completion of the planned 18T-token budget. Its cross-expert Frobenius grouping remains distinct from fixed individual-expert radii. The recovered [August norm-reduction incident](https://github.com/marin-community/marin/issues/8073#issuecomment-5230192126) reports a sharded 2D overcounting repair and eleven stable LR-sweep completions; this is maintainer-posted, agent-assisted evidence, not a local distributed reproduction.

Excluded leads include FiveTechSoft/tinyllama's purported MuonH, whose inspected C update clips an orthogonalized direction but omits radius-scaled sphere retraction; generic spherical-cautious/AdamP experiments; unrelated graph-centrality HyperBall, packing, games, mirrors, and automatic summaries. OpenReview challenges prevent fresh full-body verification of some nearby candidates. These search and access boundaries preclude an absolute-completeness claim.
