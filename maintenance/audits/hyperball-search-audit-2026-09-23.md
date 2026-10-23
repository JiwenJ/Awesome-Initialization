# Hyperball Search and Implementation Audit — 2026-09-23

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

## Paper Search and Version Check

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

## Six Added Learning Resources / Reports

### FAI-Seminar author talk — 2026-07-24

The [official FAI schedule](https://www.fai-seminar.ac.cn/FAI/previous.html) lists Kaiyue Wen's Chinese-language **Fantastic Pretraining Optimizers and Where to Find Them II: Hyperball Optimization** talk.
The row links the paper, announcement, and official Bilibili recording. It is a separate event from the existing Yao Class, Princeton, and IOS materials.
The recording was not watched; Bilibili/WeChat targets failed extraction, so the verified schedule is the stable entry point.

### Optimization 1 — Norm reparametrization — 2026-01-23

[Ziming Liu's tutorial](https://kindxiaoming.github.io/blog/2026/optimization-1/) explicitly starts from Hyperball's fixed-radius motivation and explores learned magnitude through a two-dimensional Adam/MSE example and Colab.
It is a related norm-direction extension, not an AdamH/MuonH LLM benchmark or a general counterexample for scale-invariant architectures.

### Marin expert-group norm investigation — 2026-08-24

[Issue #8621](https://github.com/marin-community/marin/issues/8621) documents cross-expert Frobenius normalization for stacked `[layers, experts, in, out]` tensors while Newton–Schulz acts on individual expert matrices.
An [August 25 comparison](https://github.com/marin-community/marin/issues/8621#issuecomment-5406386634) reports d768 Paloma macro loss **3.015 with grouped experts versus 3.018 per expert**, described by the maintainer as within noise.
The implementation and interpretation correction is detailed below; the issue is primary engineering evidence rather than a formal paper.

### Agnes 2B pretraining protocol — 2026-09-03

The Agnes Foundation Model Team's [report](https://github.com/AgnesAI-Labs/Agnes-2B-Pretraining/blob/main/REPORT.md) specifies selected-matrix MuonH with AdamW fallbacks, FP8, and a two-stage curriculum for a proposed 2.032B dense model.
The date follows the [first publication commit](https://github.com/AgnesAI-Labs/Agnes-2B-Pretraining/commit/43a36a9179c02d0025f0aad3d9a3a43201c3b1cc); the document labels itself Technical Report V1.1, September 2026.
It explicitly describes a falsifiable protocol: training time, capability targets, and release floors are projections, reference-system measurements, or acceptance criteria, **not completed Agnes training results**.
The repository supplies PDF/Markdown; no runnable full training release was independently verified.

### Blog: Survey of Optimizers — 2026-08-28

[Ruoran Xu's arXiv survey](https://arxiv.org/abs/2608.28557) remains v1. [Section 5.2](https://arxiv.org/html/2608.28557v1#S5.SS2) explains Hyperball through weight norms and angular learning rates; Sections 4.3, 10.5, and 14.3 add geometry and evaluation context.
This is substantive explanatory coverage, but its Hyperball results summarize prior work; count it as a resource rather than another independent experiment or research-paper entry.

### MACRO companion seminar at Johns Hopkins — 2026-09-14

The [institutional event page](https://publichealth.jhu.edu/events/2026/09/14/biostatistics-dept-seminar-demystifying-manifold-constraints-in-llm-pre-training) identifies Shiqian Ma's **Demystifying Manifold Constraints in LLM Pre-Training** seminar.
Its connection is the already-indexed MACRO comparison paper. The abstract discusses constraints, RMS normalization, and rotational equilibrium but does not itself name Hyperball.
No public recording or slides were verified; this entry is an author seminar record, not a claim about viewed video content.

## Nine Added Implementation / Artifact Families

### 1. PaddlePaddle upstream Muon / Adam Hyperball

[PR #79792](https://github.com/PaddlePaddle/Paddle/pull/79792) merged on **2026-09-21**, merge commit `8a0db499edcfa4ce6a54bc1d5d94c32f67e4da3c`.
Inspected [Muon source](https://github.com/PaddlePaddle/Paddle/blob/7c722e55099a9750186922c5158f9b78f723f168/python/paddle/optimizer/muon.py#L666) routes `use_hyperball` and `use_muon` to MuonH, AdamH, ordinary Muon, or AdamW.
`_hyperball_apply` computes FP32 radius and normalized update, steps by `lr * radius`, then reprojects; Hyperball routes omit weight decay. Trailing-two-axis norms distinguish full 2D matrices and individual 3D expert matrices.
The [sharding implementation](https://github.com/PaddlePaddle/Paddle/blob/7c722e55099a9750186922c5158f9b78f723f168/python/paddle/distributed/fleet/meta_optimizers/muon_sharding_optimizer.py#L327) keeps Muon/Hyperball tensors whole on an owner rank.
Radius is recomputed rather than saved as an initialization checkpoint; exact preservation remains subject to epsilon and rounding. A merged integration does not establish availability in every released Paddle version.

### 2. HeavyBall HyperBallAdamW

The [public class](https://github.com/HomebrewML/HeavyBall/blob/44b4bb46483da14d81f2417c2bb39e0c9da42fb3/heavyball/__init__.py#L414) cites the Hyperball note and routes rank-two-or-higher tensors to Hyperball, with ordinary AdamW for vectors.
The [actual update](https://github.com/HomebrewML/HeavyBall/blob/44b4bb46483da14d81f2417c2bb39e0c9da42fb3/heavyball/utils.py#L3736) normalizes the direction, steps by learning rate times stored initial norm, and projects back, with precision promotion and stochastic copy-back.
Default weight decay is zero; optional decay/cautious masking changes the direction before normalization. Higher-rank tensors use whole-tensor norms, not automatic per-expert constraints.

### 3. rollfast Hyperball transforms

[Pinned source](https://github.com/RhizomeResearch/rollfast/blob/c232e58bdc8d0caeeadc2413e9f1080e4fd41ecb/src/rollfast/optim/hyperball.py) stores initial L2 norms in `HyperballState`, uses FP32/named-axis reductions, and applies normalized direction, radius-scaled step, and fixed-radius projection as a terminal Optax transform.
The [README](https://github.com/RhizomeResearch/rollfast/blob/c232e58bdc8d0caeeadc2413e9f1080e4fd41ecb/README.md) and [tests](https://github.com/RhizomeResearch/rollfast/blob/c232e58bdc8d0caeeadc2413e9f1080e4fd41ecb/tests/test_hyperball.py) expose AdamW, Muon, PRISM, RMNP, Kron, Aurora, and Riemannian-Aurora compositions, masks, and separate fallback learning rates.
[PyPI](https://pypi.org/project/rollfast/) supplies a distribution entry. Optional nonzero decay/caution changes the underlying direction; no LLM superiority benchmark was verified.

### 4. MarinSkyRL PyTorch port

[PR #249](https://github.com/marin-community/MarinSkyRL/pull/249) merged on **2026-08-03**; [source at the PR head](https://github.com/marin-community/MarinSkyRL/blob/339039e5468b9c0f1dab00882cd70a95f22de3ae/skyrl-train/skyrl_train/distributed/grug_muonh.py) implements MuonH hidden/expert matrices, AdamH output head, and ordinary Adam auxiliary groups.
MuonH/AdamH share a learning-rate track; ordinary Adam has another. `_hyperball_delta` uses trailing matrix axes and DTensor-aware materialization, with FP32 state and BF16 compute.
The port rejects expert parallelism above one, nonzero weight decay, and unsupported options. The PR reports JAX-oracle, FSDP2/checkpoint, and four-H100 lifecycle checks, not an RL-quality comparison.
This is Marin's SkyRL fork; it is not evidence of upstream SkyRL support.

### 5. Author nanochat MuonH submission

[dangxingyu's PR #498](https://github.com/karpathy/nanochat/pull/498) is **unmerged**. [Pinned optimizer code](https://github.com/karpathy/nanochat/blob/330fa1188c4dfd41345307cb90b71c06b4b37dcd/nanochat/optim.py) supplies cached initial norms and `hyperball_step_fused` around a NorMuon direction.
The submission links [FP8](https://wandb.ai/xingyu20/nanochat/runs/uocq5uxw) and [BF16](https://wandb.ai/xingyu20/nanochat/runs/5f40sch5) runs plus [schedule discussion #499](https://github.com/karpathy/nanochat/discussions/499).
It also changes parameterized RMSNorm, zero-initialized vector output multipliers, matrix learning-rate depth scaling, and separate cooldowns: this is a recipe comparison, not an isolated optimizer swap.
Reported d24/8-H100 results include 167.91 minutes and CORE 0.2645; these were not reproduced. Keep it distinct from the already-indexed January 29 d12 negative experiment.

### 6. ANCORA / ancora-cutile

The [Hyperball kernel and NumPy oracle](https://github.com/MythosAd/ancora-cutile/blob/835ff0da33ec22c82ea08ace30cc4c23453164ba/ancora/optim/hyperball.py), [AdamH head](https://github.com/MythosAd/ancora-cutile/blob/835ff0da33ec22c82ea08ace30cc4c23453164ba/ancora/optim/adamh.py), and [MuonH wiring](https://github.com/MythosAd/ancora-cutile/blob/835ff0da33ec22c82ea08ace30cc4c23453164ba/ancora/optim/muon.py) establish the implementation beyond a README claim.
It uses CUDA Tile/device-resident updates, FP32 master/norm arithmetic, BF16 views, per-expert constraints, MuonH hidden/expert matrices, AdamH untied head, and ordinary Adam auxiliary groups.
The [README](https://github.com/MythosAd/ancora-cutile/blob/835ff0da33ec22c82ea08ace30cc4c23453164ba/README.md) describes a systems candidate with single-GPU Windows 11/CUDA 13.3/sm_120a validation, no stable package/API, and limited performance measurements rather than matched multi-seed quality evidence.

### 7. Tiny Shakespeare Hyperball sandbox

[Optimizer source](https://github.com/JiHa-Kim/tinyshakespeare-gpt/blob/f5c3fc7dc4a6163a4b8c787da70c8032b60affc3/scionh/optim/scion.py) stores RMS radii and converts them to Frobenius radii with `sqrt(numel)` for normalized stepping and retraction.
Its configurable ULMO directions default to hidden Gram Newton–Schulz. Default `retract` follows the wrapper geometry for that direction; optional `slerp` tangent-projects and uses an exponential-map update, a different variant.
The [parameterization note](https://github.com/JiHa-Kim/tinyshakespeare-gpt/blob/f5c3fc7dc4a6163a4b8c787da70c8032b60affc3/docs/hyperball_parametrization.md) supports its role as a community learning/experimentation artifact, not an official reproduction.

### 8. Palingenesis fine-tuning wrapper

[Current source](https://github.com/mii-llm/palingenesis/blob/8cccefc0c3ab0eb3b5b8f98a74c18f9b1d50ed7d/src/palingenesis/optim.py#L483) snapshots parameter buckets, disables decay, obtains a direction from the base optimizer displacement, normalizes it, and applies an angular step plus initial-radius projection.
[Tests](https://github.com/mii-llm/palingenesis/blob/8cccefc0c3ab0eb3b5b8f98a74c18f9b1d50ed7d/tests/test_optim_and_health.py) accompany the implementation. Positive `angular_lr` selects a common rate; zero calibrates a separate rate for each matrix from its first base update.
Some prose still describes projection alone, so cite source for semantics. Repeated 20–30% claims derive from the original pretraining paper, not a verified fine-tuning gain.

### 9. Chess-engine-4 application and mixed-result report

[Pinned source](https://github.com/maxencefrenette/chess-engine-4/blob/60b898c664e56834d3b30eeb2f0df9d736a377f1/src/chess_engine_4/training/optimizers.py) implements AdamHyperball with stored FP32 initial radii, radius-scaled updates, retraction, and zero-update handling; other parameter families use Adam.
The [experiment report](https://github.com/maxencefrenette/chess-engine-4/blob/60b898c664e56834d3b30eeb2f0df9d736a377f1/experiments/2026-08-11.03-hyperball/README.md) includes per-arm runs, controls, paired seeds, and width/token-budget studies.
Its initial d128 noninferiority gate failed (+0.01383/+0.01136 loss versus the best light AdamW); later d256/d512 arms favored AdamH, but [learning rates remained width-indexed](https://github.com/maxencefrenette/chess-engine-4/blob/60b898c664e56834d3b30eeb2f0df9d736a377f1/experiments/2026-08-11.04-adamh-16d-learning-rates/README.md).
Larger-width MXFP8 runs had spikes and MoE validation remained outstanding. This is useful community application evidence, not unchanged-learning-rate transfer or a formal paper.

## Existing Families Expanded Without Double-Counting

- **Dragon / Segurant:** [Segurant AdamH](https://github.com/OVHai-LLM/Segurant/blob/3f07101cc3e80592b4d38851b395fcb37dfab57a/optimizers/adamh.py) and [AdEMAMixH](https://github.com/OVHai-LLM/Segurant/blob/3f07101cc3e80592b4d38851b395fcb37dfab57a/optimizers/ademamixh.py) implement normalized updates and retraction, with 2D or per-first-axis-slice norms; group these training sources with the existing Dragon lineage.
- **Track 3:** the [pinned ledger](https://github.com/KellerJordan/modded-nanogpt/blob/bc3a0c2d640d0d73dedaef87eae26148d2e32afb/records/track_3_optimization/README.md) additionally links [#277 NorMuonH outer updates](https://github.com/KellerJordan/modded-nanogpt/pull/277), [#293 KL-SOAP-H](https://github.com/KellerJordan/modded-nanogpt/pull/293), [#302 SOAP-H](https://github.com/KellerJordan/modded-nanogpt/pull/302), [#316 PSGD + Hyperball](https://github.com/KellerJordan/modded-nanogpt/pull/316), and [#324 MuonH auxiliary tuning](https://github.com/KellerJordan/modded-nanogpt/pull/324).
- The latter three report 3125 steps/n=6, 3375/n=5, and 3250/n=10 respectively. #324 preserves matrix LR but retunes auxiliary parameters; these remain recipe-level records within one collection. No later Hyperball row than #324 was found in the inspected ledger.
- **Delphi:** [public blog data](https://huggingface.co/datasets/marin-community/delphi-blog-data) exposes six configurations and 4,117 rows, including 102 `delphi-ladder` and 3,737 `hparam-scaling` rows, fits, held-out validation, and per-row W&B links. The [447M / 122B-token model card](https://huggingface.co/marin-community/delphi-3e20-447Mparams-122Btokens) identifies AdamH and the Complete(d)-inspired recipe; both enrich the existing recipe entry.
- **Puro:** the [author collection](https://huggingface.co/collections/thu-pacman/puro-2b) supplies base/phase checkpoints and curriculum/averaging inputs, alongside [materialized training data](https://huggingface.co/datasets/thu-pacman/Puro-2B). A root-review follow-up read the [base-model card using a query-parameter URL](https://huggingface.co/thu-pacman/Puro-2B-Base?hardware=rtx-5080) after the clean URL failed extraction; it identifies random initialization and MuonH. These releases enrich the Puro-Megatron entry without another artifact count.
- **Marin / Levanter:** the [standalone AdamH source](https://github.com/marin-community/levanter/blob/982cef7f1d8d1a642b825fcd30ab1b44a912f478/src/levanter/optim/adamh.py) is the same implementation lineage. Individual agent experiment issues are not separate papers or implementation families.

## Cross-Expert Norm Correction and Live-Run Status

Marin's 535B stack must be described as using **recipe-specific cross-expert norm groups**, not as independently fixing every expert matrix to its own initial Frobenius norm.
The [maintainer's acknowledgment](https://github.com/marin-community/marin/issues/8621#issuecomment-5399971477) says this grouping was unintended but retained after earlier comparisons; the [author response](https://github.com/marin-community/marin/issues/8621#issuecomment-5404682748) accepts retaining it while monitoring individual expert norms.
The 3.015-versus-3.018 d768 result does not establish a robust advantage. An [August 31 stability update](https://github.com/marin-community/marin/issues/8621#issuecomment-5482266030) is an intermediate observation.
The [hero tracker](https://github.com/marin-community/marin/issues/8435) remained open, last updated 2026-09-22 22:33 UTC; nothing inspected establishes completed 18T-token training by the cutoff.
The [agent digest](https://marin.readthedocs.io/en/latest/reports/agent-moe-experiments/) explicitly summarizes 80 experiments as of August 20, so it must not be presented as covering all September issues.

## Exclusions and Unresolved Candidates

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

## Access Limits and Uncounted Resource Leads

- **Tencent ELR:** [official page](https://hy.tencent.ai/research/elr) and its English variant failed extraction; direct access returned HTTP 567. A [public bookmark](https://github.com/AkihikoWatanabe/paper_notes/issues/6289), translations, and HyperTransfer's bibliography corroborate existence, not a fresh primary-body review. The reported August 5 date remains unconfirmed; no translation is counted separately.
- **Author material:** the Hyperball 2.1 redirect reached a Notion URL that returned 404 here, and 2.2 yielded no extractable body. IOS Google Slides export was inaccessible; its prior content description is inherited, not freshly reviewed. These failures do not establish withdrawal.
- **Princeton recording:** the official event linked [Xingyu Dang — PLI Lunch Series 20260205](https://drive.google.com/file/d/1vqllSlJMhrabWm57sSnvJLhvttm8RvQd/view?usp=sharing); add it to the existing entry, but the recording was not watched.
- **Unreleased/future talks:** Kaiyue Wen's [homepage](https://whenwen.github.io/) records an April 2 Kimi talk without verified public media. The [September 30 Rochester MACRO event](https://events.rochester.edu/event/demystifying-manifold-constraints-in-llm-pre-training) is future-dated at this cutoff. Neither increases completed accessible resources.
- **Snowball application:** the [August 16 SFT report](https://storage.googleapis.com/marin-public/benjaminfeuer/standing-up-a-cold-start-sft-pipeline-for-marin-models/2026.08.16/index.html) does not identify its optimizer in the inspected body; related implementation routing does not isolate Hyperball's contribution to its reported gains.

## Search Coverage and Interpretation

Paper discovery combined exact Hyperball/AdamH/MuonH/HyperP/HyperTransfer queries, September date restrictions, arXiv/OpenReview searches, and full-text follow-up on sphere constraints, effective learning rate, and weight norms.
All twelve arXiv version histories were checked directly; new candidate inclusion required primary metadata and a substantive section, experiment, or implementation connection.
Resource searches covered English/Chinese author names and combinations with seminar, lecture, slides, video, Bilibili, tutorial, Tencent ELR, Open Athena, Marin, and Snowball.
Existing author pages, institutional schedules, blogs, issue discussions, and the Marin digest/hero tracker were revisited, including recent September issue searches.
GitHub discovery used Hyperball, MuonH, AdamH, NorMuonH, AdEMAMixH, and SOAPH, followed by repository-scoped code, PR metadata, branches, commit histories, benchmark ledgers, and Hugging Face collections.
Global AdamH/SOAPH queries were noisy, making source-level scoped follow-up essential. Parameter routing, norm axes, initialization radius, normalized direction, projection, and optional decay determined implementation classification.
This is a reproducible account of inspected public evidence, **not a guarantee of exhaustive coverage** of private repositories, unindexed forks, inaccessible pages, or newly published material.
