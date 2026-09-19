# Scale-Aware Hyperparameter Transfer Search Audit — 2026-09-19

This audit records how the repository expanded from strict μP and Hyperball collections to a complementary scale-aware hyperparameter-transfer collection. It is a best-effort audit of public sources available through **2026-09-19**, not a proof that every unpublished, proprietary, or newly indexed item has been found.

## Scope Used for This Audit

A direct paper must do at least one of the following in its main text:

- preserve an optimization hyperparameter across a stated proxy-to-target scale change;
- derive or fit a rule that predicts an optimal learning rate, batch size, momentum, weight decay, schedule, or closely related training hyperparameter at an unseen scale;
- introduce a measurement or experiment-selection method whose explicit purpose is scale-aware hyperparameter transfer;
- directly test a failure mode or limit of such a transfer rule.

Relevant scale axes include width, depth, parameters, model shape, data or token horizon, compute, batch size, schedule length, sparsity, expert count or activation ratio, adaptation rank, and post-training scale. A direct paper does not have to use μP or Hyperball.

The audit excludes cross-dataset/task AutoML transfer, generic Bayesian-optimization warm starts, ordinary transfer learning, neural architecture search, and performance scaling laws without an optimization-hyperparameter result. It also excludes papers that only cite μP, Hyperball, or a scaling law without deriving, implementing, testing, or materially applying the method.

## Search Routes

The review combined:

- arXiv title, abstract, and full-text searches for `hyperparameter transfer`, `hyperparameter scaling law`, `learning rate transfer`, `optimal learning rate scaling`, `batch size scaling`, `token horizon`, `model scale`, `MoE sparsity`, `proxy model`, `target model`, `μP`, `muP`, `Hyperball`, `AdamH`, `MuonH`, `HyperP`, `nGPT`, and `νGPT`;
- exact-title and citation-following checks from the newest direct papers;
- PMLR, OpenReview, ICLR, NeurIPS, institutional, author, and project pages when a formal or primary record existed;
- official GitHub and Hugging Face organizations for implementations, released measurements, models, and documentation;
- an incremental review of records first public from 2026-09-15 through 2026-09-19.

The exact arXiv phrase query for `hyperparameter transfer` returned many cross-domain, HPO, and incidental matches. Titles and abstracts were screened, then the main text was checked whenever the abstract did not establish whether the transfer was across model/training scale.

## Newly Indexed Complementary Direct Papers

| Record | Inclusion evidence |
|---|---|
| [Hyperparameter Scaling Laws Across MoE Sparsity](https://arxiv.org/abs/2609.08690) | Direct LR/batch extrapolation over compute, tokens, activation ratio, expert granularity, and a held-out 12B/1:64 MoE. |
| [Power-Law Entropy Search](https://arxiv.org/abs/2609.01431) | Directly optimizes which proxy runs reduce uncertainty in the full hyperparameter scaling law per unit cost. |
| [Post-Training Science for Supervised Fine-Tuning](https://labs.baseten.co/articles/post-training-science-for-supervised-fine-tuning) | The June Base Labs report directly asks whether LR/batch selection transfers across scale, model family, dense/MoE, data, and LoRA/full SFT; arXiv:2609.01244 followed in September. |
| [OpenEuroLLM learning-rate, batch-size and loss laws](https://arxiv.org/abs/2608.28308) | Direct joint LR/batch scaling study, including stable-to-decay phase transfer and released runs. |
| [Optimal Learning Rate Scaling Depends on Data](https://arxiv.org/abs/2607.07884) | Exact counterexample and correction for depth-wise LR transfer; the narrow scalar setting is stated in the description. |
| [How to Allocate Your Tokens?](https://arxiv.org/abs/2607.01487) | Derives optimal/suboptimal batch laws after splitting tokens into batch and steps. |
| [Optimal Hyperparameters for LLM Continued Pre-training](https://arxiv.org/abs/2606.05610) | Proxy-derived compute laws plus checkpoint-state correction predict target LR/batch and reduce reported search cost. |
| [On the Role of Batch Size in Stochastic Conditional Gradient Methods](https://arxiv.org/abs/2603.21191) | Derives scale-dependent batch/step rules and an adaptive batch/sequence strategy under fixed token budgets, with NanoGPT checks; it is labeled as a narrow optimizer-theory result. |
| [Modern Optimization Theory for Hyperparameter Scaling](https://arxiv.org/abs/2603.15958) | Derives LR/momentum/batch rules over iteration or token budget; model size remains fixed, so the row does not claim size transfer. |
| [Convex Dominance in Deep Learning I](https://arxiv.org/abs/2602.07145) | Fits and extrapolates optimal LR over both horizon and model size. |
| [Optimal LR Schedules for a Random Feature Model](https://arxiv.org/abs/2602.04774) | Derives horizon-dependent schedule regimes and tests the implied transfer distinction in simple pretraining settings. |
| [Step Law](https://arxiv.org/abs/2503.04715) | Large direct empirical study of LR/model/data and batch/data laws, with held-out evaluation and public artifacts. |
| [Function-Space Learning Rates](https://proceedings.mlr.press/v267/milsom25a.html) | FLeRM explicitly transfers layerwise LR through matched function-space updates across width, depth, initialization, and LoRA rank. |
| [Joint MoE Scaling Laws](https://arxiv.org/abs/2502.05172) | Section 5.1.2 contains a direct optimal-LR law in active parameters and expert count; the main paper has broader loss/memory aims. |
| [Convex Theory and Learning-Rate Scheduling](https://arxiv.org/abs/2501.18965) | Transfers an optimal LR across schedule extensions and continued-training horizons. |
| [Scalable Optimization in the Modular Norm](https://proceedings.neurips.cc/paper_files/paper/2024/hash/8629b0fff229b8a27efb1422e990605f-Abstract-Conference.html) | Directly normalizes arbitrary base-optimizer updates and verifies LR transfer across network width and block count/depth. |
| [DeepSeek LLM](https://arxiv.org/abs/2401.02954) | Section 3.1 fits compute-dependent optimal LR/batch laws on proxies, validates at higher compute, and uses them in the target recipe. |

## Direct Work Already Covered Elsewhere

The strict [μP bibliography](../papers/mup-transfer.bib) already contains the primary parameterization lineage and many multi-axis extensions, including Tensor Programs V, Depthwise HPT, Tensor Programs VI, architecture-aware scaling, Scaling Exponents, Power Scheduler, token-horizon scaling, Time Transfer, CompleteP, Power Lines, Complete(d)P, matrix-preconditioned optimizer transfer, νGPT, and several MoE rules and applications.

The [Hyperball bibliography](../papers/hyperball.bib) already contains the formal Hyperball paper, HyperP, effective-learning-rate and schedule analyses, HyperTransfer, criticism, and substantive applications. Those records remain in their original bibliographies and are cross-linked by the new guide instead of duplicated.

The 2026-09-15 to 2026-09-19 incremental search found no new qualifying direct μP or Hyperball paper after the prior September 14 audits. HyperTransfer, first submitted 2026-09-07, remains the latest verified direct Hyperball paper. The newest complementary scale-aware HPT paper found is the 2026-09-08 MoE-sparsity law.

## Contextual Papers Kept Outside the Direct Count

| Record | Why it is contextual |
|---|---|
| [An Empirical Model of Large-Batch Training](https://arxiv.org/abs/1812.06162) | Gradient-noise scale predicts useful batch size across tasks and training, but it is a foundation for batch selection rather than a same-family proxy-to-target HPT study. |
| [Scaling Laws for Neural Language Models](https://arxiv.org/abs/2001.08361) | Includes early empirical learning-rate scaling, while its main result is loss/compute scaling and the stated LR rule has limited large-scale validity. |
| [Automatic Gradient Descent](https://arxiv.org/abs/2304.05187) | Removes manual LR tuning through an automatic rule; it is an alternative to transfer, not proxy-to-target HPT. |
| [Fast Catch-Up, Late Switching](https://arxiv.org/abs/2602.14208) | Derives and tests batch scheduling through functional scaling laws, but its main claim is schedule design rather than transferring an optimum to an unseen target scale. |
| [Training nGPT](https://arxiv.org/abs/2608.01284) | Uses scale-dependent coefficients in its normalized MoE ladder and explicitly leaves a full hyperparameter scaling law for future work. |
| [Spend Less, Fit Better](https://arxiv.org/abs/2604.22753) | Active experiment selection for performance scaling laws is methodologically relevant, but it does not implement PLES or target optimization-hyperparameter laws. |

## Representative Exclusions and Name Collisions

| Category | Decision |
|---|---|
| Cross-dataset HPO transfer, including quantile/copula BO and meta-learning response surfaces | Excluded from the scale-aware collection; these transfer search information across tasks or datasets rather than model/training scale within a recipe. |
| `Scaling Laws for Hyperparameter Optimization` | Excluded from the direct timeline because it models multi-fidelity HPO performance over search budget/epochs, not proxy-to-target model-scale hyperparameters. |
| Generic optimizer comparisons, Muon papers, or scheduler papers | Excluded unless they derive or validate how an optimum moves or remains invariant over a stated scale axis. |
| Scaling-law papers that only predict loss, compute-optimal model/data allocation, memory, or downstream quality | Excluded unless the main text also contains a substantive optimization-hyperparameter law. |
| GPT-4, MM1, or other reports that only cite μP | Excluded without public implementation or transfer evidence. |
| Graph-centrality HyperBall, geometric hyperballs, embeddings, games, and ball packing | Name collisions; unrelated to the optimizer wrapper. |
| `Training nGPT` and original nGPT | Kept as normalized-architecture context. νGPT is the paper that directly demonstrates width/depth/token-horizon LR transfer. |

## Artifact Checks

- Official code or author artifacts were verified for μP, Modula, FLeRM, Step Law, OpenEuroLLM, Joint MoE scaling, DeepSeek LLM, HyperP/ArchScale, architecture-aware scaling, Hydro, schedule transfer, nGPT, and NeMo Hyperball.
- No author-declared public implementation was verified for PLES, the MoE-sparsity law, the continued-pretraining law, Convex Dominance, the scalar depth counterexample, the token-allocation law, or the modern-optimization-theory paper as of the snapshot date.
- The Step Law author list follows arXiv v7. PLES includes Bryan Kian Hsiang Low. FLeRM uses its ICML 2025 / PMLR 267 publication record.
- OpenEuroLLM code and Hugging Face artifacts are listed separately because one provides experiment code and the other provides released measurements/models.
- Community code is not labeled official unless the linked paper or author project identifies it as such.

## Maintenance Notes

- Order direct papers by first public date, then mention later versions or venues in descriptions when material.
- Keep the direct paper, resource, and artifact tables identical between `README.md` and `docs/hyperparameter-transfer.md`.
- Keep the direct paper table synchronized with `papers/hyperparameter-transfer.bib`.
- Do not duplicate a paper into this bibliography when it is already a direct μP or Hyperball record; cross-link it in the guide.
- State the tested interpolation/extrapolation range and avoid turning fitted exponents into universal constants.
- Recheck source version, author list, and official-code claims before each snapshot update.
