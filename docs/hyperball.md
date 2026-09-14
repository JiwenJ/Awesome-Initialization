# Hyperball Optimization

> A companion collection on Hyperball, AdamH, MuonH, their analyses, implementations, and applications. Snapshot: **2026-09-14**.

Hyperball controls selected weight matrices and optimizer updates through Frobenius normalization. This collection covers the original method, substantive comparisons and criticism, applications, and explicitly identified extensions of its fixed-sphere idea. It follows the repository's primary-source, deduplication, and factual-description rules; an independent μP result is not required here. The [μP collection](mup-transfer.md) retains its original scope.

The collection contains **12 papers**, **16 learning resources / reports**, and **15 implementation / artifact entries**. Two papers (HyperP and MACRO) are also in the μP collection; collection totals overlap. MD Decoupling is explicitly labeled a related extension, and contextual readings below are outside the paper count.

## Contents

- [Start here](#start-here)
- [Mechanism and interpretation](#mechanism-and-interpretation)
- [Hyperball papers](#hyperball-papers)
- [Hyperball learning resources and reports](#hyperball-learning-resources-and-reports)
- [Hyperball implementations and artifacts](#hyperball-implementations-and-artifacts)
- [Related foundations and distinctions](#related-foundations-and-distinctions)
- [Reading the evidence](#reading-the-evidence)
- [Search coverage and maintenance](#search-coverage-and-maintenance)

## Start Here

1. Read the [original paper](https://arxiv.org/abs/2606.16899) and its [author note](https://whenwen.github.io/wd_blog/public/hyperball-part-1.html) for the wrapper and motivation.
2. Read [HyperP](https://arxiv.org/abs/2603.28743) for scale-transfer rules and limits, then [Hyperball May Not Be a Free Lunch](https://arxiv.org/abs/2607.22444) for scheduling evidence.
3. Compare [Effective Learning Rate Governs Loss Dynamics](https://arxiv.org/abs/2608.24814) with [HyperTransfer](https://arxiv.org/abs/2609.07017): approximate loss alignment and conditional optimizer-trajectory equivalence are different claims.
4. Inspect a versioned implementation below before adapting the recipe to new tensor groups or distributed layouts.

## Mechanism and Interpretation

For a constrained matrix, let $R>0$ be its fixed radius, $u_t$ the base optimizer's proposed update, and $N(X)=X/\lVert X\rVert_F$. The original wrapper is

$$
W_{t+1}=R\,N\!\left(W_t-\eta_t R\,N(u_t)\right).
$$

The paper sets $R=\lVert W_0\rVert_F$ and applies the wrapper to attention/MLP matrices. Adam and Muon supply different directions, giving AdamH and MuonH. The proposed step has norm $\eta_tR$; the final displacement after projection need not. Implementations need defined behavior for zero updates and cannot infer a positive radius from a zero-initialized matrix. See [Algorithm 1 and §2](https://arxiv.org/html/2606.16899v1).

Fixed matrix norms do not imply identical feature dynamics across arbitrary architectures. Actual angular motion also depends on the parameter–update angle; parameter routing, normalization gains, radius conventions, and schedules remain part of the recipe. [HyperP](https://arxiv.org/html/2603.28743v2) and [Free Lunch](https://arxiv.org/html/2607.22444v1) investigate these boundaries.

## Hyperball Papers

Dates are first public manuscript dates, in reverse order. Later versions are identified where relevant. The original 2025 author note and its living versions are resources in the same lineage as the 2026 formal paper, not additional papers. BibTeX: [hyperball.bib](../papers/hyperball.bib).

| Date | Paper | Main contribution | Tags |
|---|---|---|---|
| 2026-09-07 | [HyperTransfer: Understanding the Equivalence between Base Optimizer and Hyperball](https://arxiv.org/abs/2609.07017) | Maps base and Hyperball optimizers through an online proxy norm, gradient/state rescaling, and induced LR schedules; proves conditional scale-invariant trajectory equivalence and studies a non-invariant extension. | theory, optimizer equivalence, effective LR, state mapping |
| 2026-08-27 | [Puro-2B: Poor Lab's Qwen2-1.5B Trained on RTX 5090 within $5090](https://arxiv.org/abs/2608.27370) | Uses MuonH for attention/MLP matrices, studies effective-LR matching and schedules, and retains MuonH during supervised fine-tuning; reviewed v2, September 3. | application, MuonH, pretraining, SFT, scheduling |
| 2026-08-25 | [Effective Learning Rate Governs Loss Dynamics in Language Model Pretraining](https://arxiv.org/abs/2608.24814) | Tests MuonH/MuonW loss-trajectory alignment through effective-LR interventions and predicts held-out Hyperball runs without refitting its scaling law; accuracy depends on normalization and slowly varying dynamics. | empirical analysis, effective LR, loss dynamics, scaling laws |
| 2026-07-24 | [Hyperball May Not Be a Free Lunch](https://arxiv.org/abs/2607.22444) | Analyzes angular effective LR and radial/tangential updates; controlled MuonWD/MuonH schedule matching suggests effective-step evolution explains much of the difference, while faster early convergence can impair later performance. | criticism, scheduling, angular dynamics, MuonH |
| 2026-07-22 | [Muon Reduces the Training Cost of Regulatory DNA Transformers](https://doi.org/10.64898/2026.07.17.739267) | Compares AdamW, AdamH, MuonW, and MuonH on 26M–420M regulatory-DNA Transformers; independent weight decay works better with Muon in this setting, with relative-step and spectral diagnostics. | application, DNA, optimizer comparison, negative boundary evidence |
| 2026-06-28 | [On the Nonlinearity of Learning Rate Scaling for LLM Training](https://arxiv.org/abs/2606.29158) | Uses AdamH to test a weight-norm explanation of nonlinear LR scaling; 64M-model AdamH experiments make data-horizon extrapolation more nearly log-linear and reduce extrapolation cost relative to AdamW. | AdamH, effective LR, token horizon, scaling analysis |
| 2026-06-24 | [Improving Neural Network Training by Decoupling the Magnitude and Direction of Weight Vectors](https://arxiv.org/abs/2606.25971) | Extends the fixed-sphere idea with learnable row/column magnitude gains and ablates sphere axes and gains; uses a different update-scaling convention from exact AdamH/MuonH. Reviewed v2, July 17. | related extension, MD Decoupling, magnitude gains, width transfer |
| 2026-06-15 | [Fantastic Pretraining Optimizers and Where to Find Them II: Hyperball Optimization](https://arxiv.org/abs/2606.16899) | Introduces the AdamH/MuonH wrapper and its weight-decay motivation; reports 20–30% token-equivalent gains against an AdamW scaling-law baseline on Qwen3-style models up to 1.2B and reduced optimal-LR drift in width/depth sweeps. | original method, Hyperball, AdamH, MuonH, transfer |
| 2026-06-10 | [Redesign Mixture-of-Experts Routers with Manifold Power Iteration](https://arxiv.org/abs/2606.12397) | Evaluates MPI routers with AdamH and MuonH, adopts MuonH for 3B/11B MoE pretraining, and transfers a router-scale constant from smaller sweeps using Hyperball norm control. | application, MoE, MPI routers, MuonH |
| 2026-05-06 | [Demystifying Manifold Constraints in LLM Pre-training](https://arxiv.org/abs/2605.04418) | Compares MACRO with Frobenius and spectral MuonH variants on 120M–1B Qwen3-like models, isolates tangent-projection and weight-decay effects, and tests μP-compatible width transfer. | comparison, MACRO, MuonH, manifold constraints, μP |
| 2026-03-30 | [Rethinking Language Model Scaling under Transferable Hypersphere Optimization](https://arxiv.org/abs/2603.28743) | Builds HyperP around MuonH/AdamH, derives width/depth rules, fits token-horizon scaling, and adds SqrtGate for MoE granularity; finds Hyperball alone insufficient for depth transfer. | extension, HyperP, μP, depth, token horizon, MoE |
| 2026-01-29 | [Manifold constrained steepest descent for smooth and closed-set optimization](https://arxiv.org/abs/2601.21487) | The August 13 v2 explicitly formulates Hyperball and explains why projecting an ambient steepest direction need not guarantee constrained descent; its counterexample is geometric, not an LLM benchmark. | theoretical boundary, MCSD, projection, stationarity, v2 evidence |

## Hyperball Learning Resources and Reports

Author resources, experimental reports, talks, and tutorials are labeled separately from papers. Live reports are snapshots, not promises of completed runs. Translations and redirect aliases are grouped with their originals.

| Resource | Type | Why it matters |
|---|---|---|
| [Marin 535B-A23B launch note](https://openathena.ai/blog/marin-535b-launch-note/) | Ongoing training report, 2026-09-03 | The [public run specification](https://github.com/marin-community/marin/issues/8435#issuecomment-5335872267) uses MuonH matrices and AdamH readout updates; 18T tokens is the planned budget, not a completed result. |
| [Hyperball, effective lr, and the shape of peak-then-decay](https://jiaxuanzou0714.github.io/en/blog/2026/hyperball-implicit-lr-schedule/) | Jiaxuan Zou technical essay, 2026-08-25 | Synthesizes effective-LR replay, Free Lunch, and scheduling interpretations; explanatory analysis, not an independent benchmark. [Chinese version](https://jiaxuanzou0714.github.io/blog/2026/hyperball-implicit-lr-schedule/). |
| [MarinDNA: A 1B standard Transformer rivals Evo 2 40B on variant effect prediction](https://openathena.ai/blog/marin-dna/) | Primary application report, 2026-08-03 | Applies a Complete(d)-inspired AdamH recipe to genomic model scaling, with proxy sweeps and target checks across size, batch, and token horizon. Also indexed in the μP collection. |
| [Magnitude–Direction Decoupling](https://haeggee.github.io/posts/magnitude-direction-decoupling) | Companion author post, 2026-06-15 | Explains the fixed-norm direction and learned-magnitude extension, with ablations; its update scaling differs from the original Hyperball wrapper. |
| [Improving our LLM Pretraining Efficiency](https://openathena.ai/blog/pretraining-speedup/) | Larry Dial / Open Athena report, 2026-06-03 | Reports AdamH MoE scaling and MuonH ablations across four compute scales; distinguishes theoretical compute gains from kernel/runtime effects and combined-recipe improvements. |
| [Scaling Laws That Extrapolate 300× Past the Fit](https://openathena.ai/blog/delphi/) | Will Held / Delphi report, 2026-05-11 | Combines AdamH with calibrated token-horizon scaling and width-sweep checks; the empirically chosen horizon exponent is not a universal Hyperball law. Also indexed in the μP collection. |
| [A Genealogy of Optimizers](https://itzsid.github.io/publications/optimizer-ladder.html) | Siddharth Choudhary, with Claude; tutorial, 2026-05 | Section 13 introduces MuonH through weight-norm control; interactive toy examples are illustrative rather than an optimizer benchmark. |
| [Fantastic Pretraining Optimizers — IOS slides](https://docs.google.com/presentation/d/1t5TSjK0CzVuDUQKB196XqwUSh29Qy2z1/htmlpresent) | Kaiyue Wen author talk, 2026-03-21 | The author-linked deck introduces AdamH/MuonH and transfer experiments in slides 16–19. |
| [On the Hypersphere: μP Scaling of Optimizers with the Hyperball Mechanism](https://jiaxuanzou0714.github.io/en/blog/2026/spherical-hyperball/) | Jiaxuan Zou technical essay, 2026-03-07 | Derives SGDH, AdamH, and MuonH feature-space scaling under stated statistical/geometric assumptions; no independent transfer benchmark. Also indexed in the μP collection. |
| [Does Muon improve regulatory DNA learning? Part 1.](https://origin.bio/blogs/muon/) | Viraj Doshi author experiments, 2026-03-05 | Explains Hyperball geometry with AdamH/MuonH LR sweeps and cases favoring weight decay; companion to the later regulatory-DNA preprint. |
| [Hyperball Optimizer — Princeton PLI](https://pli.princeton.edu/events/2026/hyperball-optimizer) | Xingyu Dang author talk, 2026-02-05 | Institutional talk page with a linked recording, covering norm control, rotational equilibrium, and transfer; recording content was not independently reviewed. |
| [Nanochat: Hyperball/MuonH Experiments (Negative Result)](https://github.com/karpathy/nanochat/blob/master/dev/LOG.md#2026-01-29-hyperballmuonh-experiments-negative-result) | Upstream development log, 2026-01-29 | Records unsuccessful d12 MuonH/AdamH integration attempts, including LR sweeps, zero-initialized projection and readout-scale issues; a setting-specific result. |
| [Yao Class Seminar 86: Fantastic Pretraining Optimizers I & II](https://group.iiis.tsinghua.edu.cn/~stu/seminar/event/2025/seminar-86/) | Kaiyue Wen author seminar, 2025-12-13 | Official early Hyperball announcement linking the original note; no separate public recording was verified. |
| [Fantastic Pretraining Optimizers 2.1: Hyperball Optimization](https://whenwen.github.io/wd_blog/public/hyperball-part-1.html) | Living author note / original research lineage | Redirects to the paper-linked Notion note. The [legacy combined note](https://whenwen.github.io/wd_blog/public/index.html) has an author-supplied 2025-11-30 citation; both belong to the formal paper's lineage. |
| [Fantastic Pretraining Optimizers 2.2: The Hitchhiker's Guide to the Weight Norm Theory](https://whenwen.github.io/wd_blog/public/weight-decay-part-2.html) | Living author theory tutorial | Develops noise-model explanations of weight norms and angular step sizes, with interactive simulations; exact publication date was not established. |
| [Marin Agent MoE Experiment Digest](https://marin.readthedocs.io/en/latest/reports/agent-moe-experiments/) | Living project experiment digest | Groups AdamH/MuonH, gradient-aware Hyperball, and optimizer ablations; maintainer-hosted, agent-assisted experiment records rather than peer-reviewed evidence. |

## Hyperball Implementations and Artifacts

Links below were inspected for method support and provenance, not executed as reproductions. Pinned files identify the inspected implementation; upstream APIs may differ.

| Artifact | Framework / method | What it provides |
|---|---|---|
| [Marin / Levanter Hyperball optimizers](https://github.com/marin-community/marin/tree/de412954e99488cf004b54104a7eeb4b670d10a2/lib/levanter/src/levanter/optim) | JAX / Optax / Haliax; author-associated stack | Versioned [AdamH](https://github.com/marin-community/marin/blob/de412954e99488cf004b54104a7eeb4b670d10a2/lib/levanter/src/levanter/optim/adamh.py) and [MuonH](https://github.com/marin-community/marin/blob/de412954e99488cf004b54104a7eeb4b670d10a2/lib/levanter/src/levanter/optim/muonh.py), with matrix normalization, reprojection, and explicit parameter routing. |
| [Marin Grug MoE optimizers](https://github.com/marin-community/marin/tree/de412954e99488cf004b54104a7eeb4b670d10a2/experiments/grug/moe) | JAX / Optax; project implementation | Expert-aware Hyperball grouping and [535B run optimizer snapshot](https://github.com/marin-community/marin/blob/12d8b6f09f96ad4f0277445765c1a00dbc81d5be/experiments/grug/moe_hero_ep/optimizer.py); distributed layouts and auxiliary parameter treatment are recipe-specific. |
| [NVIDIA NeMo Emerging-Optimizers](https://github.com/NVIDIA-NeMo/Emerging-Optimizers) | PyTorch; framework integration | [MuonHyperball](https://github.com/NVIDIA-NeMo/Emerging-Optimizers/blob/36f70336da89dc80481c07dfb0af2c7333b9e5b3/emerging_optimizers/orthogonalized_optimizers/muon_hyperball.py) and [HyperballHook](https://github.com/NVIDIA-NeMo/Emerging-Optimizers/blob/36f70336da89dc80481c07dfb0af2c7333b9e5b3/emerging_optimizers/weight_update_hooks/hyperball.py); this snapshot requires an explicit nonzero radius and validates initial parameter norms. |
| [modded-nanogpt Track 3](https://github.com/KellerJordan/modded-nanogpt/tree/master/records/track_3_optimization) | PyTorch; public optimization benchmark | Source/log collection with author submissions for [AdamH](https://github.com/KellerJordan/modded-nanogpt/pull/272), [MuonH](https://github.com/KellerJordan/modded-nanogpt/pull/267), [NorMuonH](https://github.com/KellerJordan/modded-nanogpt/pull/273), and [KL-SOAP-H](https://github.com/KellerJordan/modded-nanogpt/pull/290), plus later variants and schedules. |
| [Hyperball May Not Be a Free Lunch — experiments](https://github.com/mangocrazz/hyperball-may-not-be-a-free-lunch) | PyTorch; official paper artifact | Training programs, released scalar CSVs, and plotting code for effective-LR decomposition, alignment, and schedules; figure recreation can use CSVs without GPUs. |
| [Puro-Megatron](https://github.com/thu-pacman/Puro-Megatron) | PyTorch / Megatron; official Puro-2B code | [Versioned documentation](https://github.com/thu-pacman/Puro-Megatron/blob/a7b80e873a0b5e1820ae425b9abd1b9ec578dc5c/docs/puro-megatron.md) describes MuonHyperball, logical QKV/SwiGLU grouping, tensor-parallel radii, and effective-LR diagnostics. |
| [microsoft/ArchScale](https://github.com/microsoft/ArchScale) | PyTorch / LitGPT; official HyperP code | HyperP, MuonH, and SqrtGate, with sharded global Frobenius-norm handling and hybrid parameter groups. Also indexed in the μP collection. |
| [Router with Manifold Power Iteration](https://github.com/ericshwu/Router-with-Manifold-Power-Iteration) | PyTorch / TorchTitan; official MPI-router code | Router implementation and advanced optimizer support for the paper's AdamH/MuonH comparisons; repository documents FSDP/expert-layout limits. |
| [MD Decoupling — dense models](https://github.com/haeggee/Megatron-LM/tree/gainz) | PyTorch / Megatron; official related-extension code | Fixed-norm direction and learned-magnitude optimizer, called `master` in research code; [MoE branch](https://github.com/haeggee/Megatron-LM/tree/feat/scaling-sweeps) accompanies the same paper. This is not the exact original Hyperball wrapper. |
| [Open-Athena/marin-dna](https://github.com/Open-Athena/marin-dna) | JAX / Marin; primary application hub | Genomic models, experiment pointers, and AdamH-based training context. Also indexed in the μP collection. |
| [Marin Complete(d)-inspired AdamH recipe](https://github.com/marin-community/marin/blob/a638849fa837f924aaac66ff3d0c1f581dfdd49e/experiments/scaling_law_sweeps/completed_adamh.py) | JAX / Levanter; application scaling recipe | Combines AdamH with batch/token-dependent settings and an empirically selected token exponent; shared by Delphi and MarinDNA. Also indexed in the μP collection. |
| [Author HyperballAdam toy](https://github.com/WhenWen/WhenWen.github.io/blob/78142c6a673ca37fdde2cb9bab138298ed0eddd1/wd_blog/scripts/hyperball.py) | PyTorch; historical author example | Normalized MLP example with fixed-radius reprojection; its step omits the paper's explicit radius multiplier, so its LR convention is radius-dependent. |
| [Dragon AdamH / AdEMAMixH](https://huggingface.co/alexandretl/dragon/commit/940f63368210b3712cd1b4c1d1506256c85c8e65) | PyTorch; community implementation | Source commit introducing Hyperball-normalized Adam and AdEMAMix updates; experimental code, without an independently verified transfer benchmark. |
| [TitanPrecond](https://github.com/ErstinAn/titanprecond) | PyTorch / TorchTitan; community implementation | Experimental manifold optimizer with a `muonh` option and Frobenius/spectral constraints; update-alignment conventions change the LR scale. |
| [CMU 18660 Hyperball Project](https://github.com/aramesh10/18660-Optimization-Hyperball-Project) | PyTorch; coursework implementation | MLP/NanoGPT hMuon comparisons; a community learning artifact, not an official paper reproduction or a separate formal paper. |

## Related Foundations and Distinctions

These contextual links do not increase the Hyperball paper count and are not asserted to implement Hyperball.

| Reading | Connection and distinction |
|---|---|
| [Spherical Motion Dynamics](https://arxiv.org/abs/2006.08419) | Earlier analysis of normalization, SGD, weight decay, and motion on a sphere. |
| [Training Scale-Invariant Neural Networks on the Sphere Can Happen in Three Regimes](https://arxiv.org/abs/2209.03695) | Earlier fixed-sphere learning-dynamics analysis; useful context for effective-LR regimes. |
| [Rotational Equilibrium: How Weight Decay Balances Learning Across Neural Networks](https://proceedings.mlr.press/v235/kosson24a.html) | Analyzes norm/angular-update equilibria and explicit rotation control before Hyperball. |
| [nGPT: Normalized Transformer with Representation Learning on the Hypersphere](https://arxiv.org/abs/2410.01131) | Architectural/vector normalization differs from a matrix-wise Frobenius optimizer wrapper. |
| [Controlled LLM Training on Spectral Sphere](https://arxiv.org/abs/2601.08393) | Spectral-sphere constraints and SSO/Muon Sphere differ from Hyperball's Frobenius sphere. |
| [Completed Hyperparameter Transfer across Modules, Width, Depth, Batch and Duration](https://arxiv.org/abs/2512.22382) | Transfer theory adopted by later AdamH recipes; the original paper does not introduce Hyperball. |
| [Summer-22B](https://arxiv.org/abs/2603.00173) | Uses row-wise tangent-projected Adam and unit-row retraction; retained in the μP index, not labeled a Hyperball application. |
| [Mano](https://arxiv.org/abs/2601.23000) and [Spherical Cautious Optimizers](https://openreview.net/forum?id=OyT2CJ4fh7) | Related tangent-space/oblique or cautious-update methods; checked texts do not establish direct AdamH/MuonH evaluation. |

## Reading the Evidence

- **Transfer is conditional.** The original paper's finite width/depth sweeps are not a theorem for every architecture. HyperP supplies additional depth and MoE rules; its token-horizon exponent is fitted. [Original experiments](https://arxiv.org/html/2606.16899v1), [HyperP](https://arxiv.org/html/2603.28743v2).
- **Scheduling remains consequential.** Free Lunch reports heuristic bidirectional schedule matching; ELR reports conditional loss alignment, including an initial transient. HyperTransfer additionally maps optimizer states under explicit assumptions. Its non-scale-invariant extension compares a rescaled representative, not necessarily the raw fixed-radius network's loss. [Free Lunch](https://arxiv.org/html/2607.22444v1), [ELR](https://arxiv.org/html/2608.24814v1), [HyperTransfer](https://arxiv.org/html/2609.07017v1).
- **Separate optimizer evidence from full-recipe gains.** Puro combines hardware, precision, curriculum, and optimizer changes. The DNA study's family-level efficiency gains are not evidence that MuonH beats MuonW. [Puro](https://arxiv.org/html/2608.27370v2), [DNA companion experiments](https://origin.bio/blogs/muon/).
- **Inspect norm conventions.** Whole-matrix, row-wise, expert-wise, and global sharded norms define different algorithms. NeMo's inspected source requires an explicit radius; older online API documentation differs. Parameter-group exceptions are part of each linked implementation.

## Search Coverage and Maintenance

The search covered public sources through **2026-09-14**, including arXiv version histories and full text, bioRxiv metadata, OpenReview records, author and institutional pages, and GitHub code/logs. It followed Hyperball, AdamH, MuonH, SGDH, HyperP, HyperTransfer, and the Fantastic Pretraining Optimizers II / 2.1 / 2.2 aliases. The latest directly verified paper is HyperTransfer, first submitted September 7. Indexed search cannot establish absolute completeness.

See the [search audit](hyperball-search-audit-2026-09-14.md) for inclusion decisions, version issues, and unresolved artifacts. Graph-centrality HyperBall, geometric ball packing, generic “hyperball” terminology, games, mirrors, and automatic paper summaries are outside this optimizer collection. Follow [CONTRIBUTING.md](../CONTRIBUTING.md) when adding entries.
