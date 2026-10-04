# μP, Scale-Aware HPT, and Hyperball Audit — 2026-10-04

This public-source update follows [CONTRIBUTING.md](../CONTRIBUTING.md), extending the September 24 snapshot through **October 4, 2026** and checking older omissions. It adds 14 distinct papers, keeps manuscript revisions within existing records, and separates demonstrated transfer from heuristic recipes and unverified implementations. It does not claim exhaustive coverage of private, unindexed, or inaccessible work.

| Collection | Papers before → after | Resources before → after | Artifacts before → after |
|---|---:|---:|---:|
| μP / μTransfer | 162 → 165 | 55 → 56 | 97 → 99 |
| Complementary scale-aware HPT | 27 → 38 | 11 → 13 | 19 → 21 |
| Hyperball | 13 → 14 | 22 → 23 | 24 → 26 |

HyperP, MACRO, and the recovered looped-language-model study belong to both strict collections. The bibliographies therefore contain **214 distinct paper records**, not 217. Resource and artifact totals also overlap; a shared author-code repository is not two independent implementations.

## Search and Inclusion Method

Searches combined μP / muP / μTransfer, maximal-update spelling variants, Tensor Programs, coordinate checks, proxy-to-target hyperparameter transfer, batch-size / optimal-learning-rate scaling, Hyperball, AdamH, MuonH, HyperP, and HyperTransfer. The recent window was September 24–October 4; backward checks followed cited batch-scaling literature, author publication pages, the community μP index, and source-code references.

Primary manuscript methods and appendices were checked before inclusion. First-public dates are distinguished from revisions and venue years. Where only a month is documented, no day is invented. Generic scaling laws, optimizer papers, and related-work citations remain outside the strict μP and Hyperball tables; direct scale-dependent optimization rules can qualify for the complementary HPT table at their stated theoretical or experimental scope.

## Newly Included Papers

| Collection | Paper | Primary evidence and limitation |
|---|---|---|
| μP | [Learning Rate Transfer for Hybrid Transformer-SSM Architectures](https://arxiv.org/html/2610.01172v1) | Original μP with AdamW transfers the base learning rate in hybrid width sweeps despite failed standard coordinate checks. The 1.24B long run is not independently swept for its long-horizon optimum; Nemotron-H experiments reproduce smaller architectures rather than train the production checkpoint. First public October 1. |
| μP | [Fast Learning Rate Transfer in Shallow Linear Networks at Growing Training Horizons](https://arxiv.org/html/2609.35029v1) | Studies a μP-initialized shallow linear model with one trained hidden matrix, frozen input/readout, full-batch GD, and fixed-data spectral assumptions. Its growing-horizon theorem compares widths at a common horizon; it is not transfer between different durations. First public September 28; distinct from the existing HiLD sketched-regression paper. |
| μP and Hyperball | [How Much Is One Recurrence Worth?](https://arxiv.org/html/2604.21106v3) | Section 4.1 uses MuonH with auxiliary AdamW. Appendix D.2 independently checks HyperP width transfer and a fourfold token-horizon change for looped and non-looped models, with reported regret below 0.005 nats. Recovered historical omission, first public April 22, reviewed May 7 v3. |
| HPT | [Faynt](https://arxiv.org/html/2610.02144v1) | Sections 3.1/3.4 and Appendix C fit a shared learning-rate/weight-decay multiplier from 5M/20M/50M policy sweeps and initialize 10M/75M searches. Targets still need learning-rate adjustments; this is empirical search initialization, not zero-shot μP invariance. First public October 1. |
| HPT | [Optimizer-dependent training dynamics](https://arxiv.org/html/2609.37745v1) | Section 3, Figure 3, and Appendices E/H derive and measure optimizer-dependent optimal learning-rate laws over batch and online sample budget. Included for this explicit optimum in a single-layer teacher–student model, not as an established LLM scaling law. First public September 29; code is promised, not publicly verified. |
| HPT | [ScAn-Bench](https://arxiv.org/html/2609.35707v1) | Sections 4.1–4.3 evaluate extrapolation of full configurations, including optimization hyperparameters, from proxy Pareto points to 20× compute. Results use LLM/CLIP surrogates and expose acquisition-dependent failure modes, not a universal successful transfer recipe. First public September 28. |
| HPT | [Towards joint scaling laws with optimal batch size schedules](https://arxiv.org/html/2607.27731v1) | Section 3/Theorem 3 derives a batch schedule for a prescribed learning-rate shape and budget; Section 4/Theorem 5 couples learning rate and weight decay to training iterations. Empirical tests cover Llama3/Qwen3-MoE families. μP is background only, so this is not a strict μP addition. |
| HPT | [How to Set the Batch Size for Large-Scale Pre-training?](https://arxiv.org/html/2601.05034v2) | Sections 3–4 derive and fit WSD stable-phase batch prescriptions on 122M–1B models, then test increasing-batch training on larger Qwen3 dense/MoE models. Its fixed-learning-rate fits and batch schedule do not imply a universal square-root learning-rate rule. |
| HPT | [Scaling Law for Language Models Training Considering Batch Size](https://arxiv.org/html/2412.01505v1) | Sections 4.2.3–4.5 fit optimal batch allocations from 125M–2.6B runs and check larger targets, while considering learning-rate adjustments. Table 3 reports approximately 4.49B/6.80B, whereas the abstract rounds them differently. |
| HPT | [Adaptive Methods through the Lens of SDEs](https://arxiv.org/html/2411.15958v2) | Lemma 3.13 and Appendix F.8 derive coordinated AdamW/RMSpropW batch rules and test a 160M language model. The criterion concerns specified asymptotic-loss/speed properties, not exact trajectory invariance. An earlier May 2024 version is identified by arXiv; its exact public day remains unverified. |
| HPT | [Surge Phenomenon in Optimal Learning Rate and Batch Size Scaling](https://arxiv.org/html/2405.14578v5) | Derives a rising-then-falling optimum under a sign-gradient/Gaussian-noise approximation and tests Adam-style workloads. It is not an exact theorem for arbitrary Adam training; the very-large-batch regime has a nonzero limiting optimum. |
| HPT | [MM1](https://arxiv.org/pdf/2403.09611) | The model-scaling subsection fits peak learning rate from 9M–1.2B proxy sweeps using downstream few-shot performance, then applies it to 30B multimodal training. No independent target-optimum sweep is shown. The earlier strict μP exclusion still stands; this embedded empirical rule qualifies for HPT. |
| HPT | [Learning Rates as a Function of Batch Size](https://jmlr.org/papers/v23/20-1258.html) | The random-matrix analysis and Sections 10–11 study largest-stable learning rates and batch transfer in image models. Linear SGD and square-root adaptive rules are regime-dependent, not universal prescriptions. First public June 16, 2020; JMLR publication 2022. |
| HPT | [An Empirical Model of Large-Batch Training](https://arxiv.org/html/1812.06162v1) | Section 2.2/Equation 2.6 explicitly derives an optimal SGD step size depending on batch noise scale; Section 3.2 checks its saturating form. This local quadratic optimum satisfies the current HPT scope, although the paper was previously excluded from the stricter μP collection. |

## Metadata and Resource Checks

The existing HiLD paper [Fast Learning Rate Transfer for Gradient Descent in Sketched Linear Regression](https://nikhilgsh.github.io/publications/) now has public authors: Garrett Wen, Alberto Bietti, Nikhil Ghosh, Theodor Misiakiewicz, and Denny Wu. The obsolete anonymous note is replaced while preserving its citation key. ScAn-Bench's manuscript and author-code citation list Steven Adriaensen, who is absent from arXiv's abstract-page author list; the BibTeX follows the eight-author manuscript.

The update adds a clearly labeled [one-step μP teaching/reproduction note](https://hamidreza-hashempoor.github.io/blog/aistats2026_batch/papers/lr_transfer_mup.html), [Malladi's SDE scaling tutorial](https://sadhikamalladi.github.io/blog/2024/01/22/SDEs-ScalingRules/), [OpenAI's large-batch explainer](https://openai.com/index/how-ai-training-scales/), and the Rochester MACRO seminar. The one-step note contains inconsistent reported percentages and its notebook could not be retrieved; it is not counted as an independently validated implementation. These resources do not add paper records.

New artifacts include the [looped-LM author repository](https://github.com/kschwethelm/looped-lm-scaling), a [community MLX μP demonstration](https://gist.github.com/yberreby/4d117a2ff4571af60628c9f88c2a3988), [Adaptive-SDE](https://github.com/abhishekpanigrahi1996/Adaptive-SDE), [ScAn-Bench](https://github.com/automl/scan_bench), and the author-linked LR trajectory dataset described below. Source inspection does not constitute rerunning training or reproducing benchmarks. MLX changes residual scaling between its SP and μP comparisons, so it does not isolate the causal effect of width parameterization alone.

## Hyperball Update

The complete current Hyperball audit is included in the [root README](../README.md#hyperball-incremental-audit--2026-10-04), including the recovered application, MACRO revision, seminar, dataset provenance correction, version checks, and unresolved leads. The September 23/24 audits remain historical snapshots rather than being rewritten to imply knowledge unavailable at their cutoffs.

## Exclusions and Access Limits

- [Su Jianlin's μP tutorial](https://kexue.fm/archives/10770), [Diego Martinez Taboada's research notes](https://dmartinezt.github.io/research_notes/nn_parametrization/tensor-programs-iv-v.html), and [Trisham Patil's tutorial](https://trishampatil.com/ai%20engineering/2026/09/06/mup-maximal-update-parameterization-hyperparameter-transfer/) remain leads: indexed discovery text was available, but direct body retrieval failed. They were not promoted into the verified resource table.
- The earlier SDE OpenReview manuscript and several unresolved OpenReview candidates returned browser-verification challenges. This limits date/source verification; it is not evidence that the works are irrelevant or unavailable to human readers.
- Faynt, general batch laws, and the joint batch-schedule paper are not strict μP papers without substantive μP evidence. The one-third data-scaling paper only cites hypersphere work; no direct Hyperball experiment was identified.
- Generic geometric HyperBall, ordinary transfer learning, unrelated program-equivalence “Tensor Programs,” automatic paper summaries, and evaluation-only repositories are not added as optimization-transfer evidence.

## Validation

The update checks root/companion table equality, bibliography counts and balanced records, duplicate citation keys and canonical URLs, shared-paper metadata, reverse chronological dates, local file links, and whitespace. No expensive training sweeps or third-party notebooks were executed. The user's unrelated untracked proof note is preserved and excluded from the commit.
