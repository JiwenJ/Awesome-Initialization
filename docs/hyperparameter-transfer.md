# Scale-Aware Hyperparameter Transfer

> A companion collection on transferring optimization hyperparameters from affordable proxy runs to larger or otherwise more expensive target runs.

Snapshot: **2026-09-19**. This page indexes **17 complementary direct papers**, **10 learning resources**, and **14 implementation / artifact entries**. Direct papers already indexed by the strict μP or Hyperball collections are cross-linked instead of counted again; resource and artifact tables intentionally overlap where one tool supports several approaches.

The scope is **scale-aware transfer within a model or training family**: width, depth, parameter count, model shape, token horizon, batch size, optimizer schedule, sparsity, expert configuration, adaptation rank, or post-training scale. Cross-dataset AutoML transfer, ordinary transfer learning, neural architecture search, and scaling laws that never estimate or preserve an optimization hyperparameter are outside the direct-paper count.

## Contents

- [How the approaches relate](#how-the-approaches-relate)
- [Complementary direct papers](#complementary-direct-papers)
- [Key papers in the μP and Hyperball collections](#key-papers-in-the-μp-and-hyperball-collections)
- [Learning resources](#learning-resources)
- [Implementations and artifacts](#implementations-and-artifacts)
- [Practical transfer protocol](#practical-transfer-protocol)
- [Common distinctions](#common-distinctions)
- [Evidence limits and maintenance](#evidence-limits-and-maintenance)

## How the Approaches Relate

| Approach | Core idea | Typical transfer axes | Representative work |
|---|---|---|---|
| Parameterization invariance | Scale initialization, parameter groups, and optimizer steps so the same hyperparameters induce comparable feature dynamics. | width; with extensions, depth, modules, optimizer families, MoE | μP, Depth-μP, CompleteP, νGPT, HyperP |
| Geometry or norm control | Normalize updates in an architecture-aware norm or constrain selected weights to a sphere. | width, depth, optimizer geometry | Modular norm, Hyperball, HyperP |
| Function-space matching | Measure how much each tensor update changes the network function on a proxy and match that change on the target. | width, depth, initialization scale, LoRA rank | FLeRM |
| Empirical hyperparameter scaling laws | Fit optimal learning rate, batch size, or other settings as functions of model, data, compute, or sparsity. | parameters, tokens, compute, batch, MoE activation ratio | DeepSeek Law, Step Law, OpenEuroLLM, MoE sparsity laws |
| Theory-driven horizon laws | Derive how schedules, momentum, or batch size should change with training horizon or token budget. | steps, tokens, schedule, batch, momentum | schedule-transfer theory, random-feature theory, modern optimization bounds |
| Active experiment selection | Choose proxy configurations that reduce uncertainty in the entire scaling law per unit cost. | model and data scale | PLES |

No single approach covers every axis. μP primarily addresses width; Hyperball controls selected matrix norms and update directions; HyperP re-derives scale rules for hypersphere optimization; empirical laws remain necessary when token budget, batch size, sparsity, or data distribution changes.

## Complementary Direct Papers

These papers directly derive, estimate, validate, or falsify a proxy-to-target hyperparameter rule and are not already counted in the strict μP or Hyperball paper tables. BibTeX: [hyperparameter-transfer.bib](../papers/hyperparameter-transfer.bib).

| Date | Paper | Main contribution | Transfer axes |
|---|---|---|---|
| 2026-09-08 | [Hyperparameter Scaling Laws Across MoE Sparsity](https://arxiv.org/abs/2609.08690) | Fits learning-rate and batch-size laws that explicitly include MoE activation ratio, then validates joint scale-and-sparsity extrapolation on a held-out 12B-total-parameter model with 1/64 activation. | compute, tokens, MoE sparsity, expert granularity |
| 2026-09-01 | [Efficiently Estimating Optimal Hyperparameter Scaling Laws through Power-Law Entropy Search](https://arxiv.org/abs/2609.01431) | Introduces PLES, a cost-aware multi-fidelity acquisition rule that selects proxy runs to reduce uncertainty in an entire power-law hyperparameter fit; reported experiments need less than one tenth of grid-search compute. | model scale, data scale, experiment budget |
| 2026-08-28 | [Deriving Scaling Laws for OpenEuroLLM Models: Learning Rate, Batch Size and Loss](https://arxiv.org/abs/2608.28308) | Jointly models optimal learning rate and batch size over model/data scale and tests whether settings transfer between the stable and decay phases of WSD schedules; releases the underlying pretraining-run collection. | model size, data, batch, WSD phase |
| 2026-07-08 | [Optimal Learning Rate Scaling Depends on Data in Deep Scalar Linear Networks](https://arxiv.org/abs/2607.07884) | Gives an exact failure case for data-agnostic depth rules and derives a data-dependent correction whose dynamics are nearly depth independent in the analyzed scalar networks. | depth, data distribution |
| 2026-07-01 | [How to Allocate Your Tokens? Scaling Laws with Training Steps and Batch Size](https://arxiv.org/abs/2607.01487) | Splits data budget into batch size and training steps in a three-term loss law, recovering optimal and suboptimal batch-size scaling from runs that need not all use an optimal batch. | model size, steps, batch, token allocation |
| 2026-06-04 | [Predictable Scaling Laws of Optimal Hyperparameters for LLM Continued Pre-training](https://arxiv.org/abs/2606.05610) | Learns proxy laws from compute budget to optimal learning rate and batch size, estimates a checkpoint's equivalent pretraining compute, and reports up to 90% lower search overhead for continued pretraining. | continued-pretraining state, compute, batch |
| 2026-06 | [Post-Training Science for Supervised Fine-Tuning](https://labs.baseten.co/articles/post-training-science-for-supervised-fine-tuning) | Measures whether learning-rate and batch-size choices transfer across Qwen3 and Llama, dense and MoE models, LoRA and full fine-tuning, datasets, and a model ladder reaching 235B parameters; recommendations include uncertainty estimates. | post-training scale, family, data, LoRA/full SFT |
| 2026-03-22 | [On the Role of Batch Size in Stochastic Conditional Gradient Methods](https://arxiv.org/abs/2603.21191) | Derives regime-dependent batch-size and step-size rules under fixed token budgets for momentum conditional-gradient methods, proposes an adaptive batch/sequence strategy, and checks the predicted regimes in NanoGPT. | batch, step size, token budget, sequence length |
| 2026-03-16 | [Deriving Hyperparameter Scaling Laws via Modern Optimization Theory](https://arxiv.org/abs/2603.15958) | Derives learning-rate, momentum, and batch-size power laws from optimization bounds for LMO-based methods including normalized SGD, signSGD, and Muon; the model size is held fixed. | iterations, tokens, batch, momentum |
| 2026-02-06 | [Convex Dominance in Deep Learning I: A Scaling Law of Loss and Learning Rate](https://arxiv.org/abs/2602.07145) | Uses a weak-convexity-inspired loss bound to fit learning-rate laws and reports extrapolation up to 80× in training horizon and 70× in model size. | model size, training horizon, schedule |
| 2026-02-04 | [Theory of Optimal Learning Rate Schedules and Scaling Laws for a Random Feature Model](https://arxiv.org/abs/2602.04774) | Derives horizon-dependent optimal schedules, batch ramps, and momentum behavior in a solvable model, then shows that horizon transfer differs between easy and hard regimes in simple vision and language experiments. | horizon, schedule shape, batch, momentum |
| 2025-03-06 | [Predictable Scale: Part I, Step Law -- Optimal Hyperparameter Scaling Law in Large Language Model Pretraining](https://arxiv.org/abs/2503.04715) | Fits optimal learning rate as a function of model and data scale and optimal batch size primarily as a function of data, using 3,700 runs across dense/MoE shapes and data recipes; releases code, data, and checkpoints. | parameters, data, batch, model shape, dense/MoE |
| 2025-02-24 | [Function-Space Learning Rates](https://proceedings.mlr.press/v267/milsom25a.html) | Introduces FLeRM: record layerwise function-space update scales on a cheap model, then adjust target parameter-space learning rates to match them across width, depth, initialization scale, and LoRA rank. | width, depth, initialization, LoRA rank |
| 2025-02-07 | [Joint MoE Scaling Laws: Mixture of Experts Can Be Memory Efficient](https://proceedings.mlr.press/v267/ludziejewski25a.html) | Alongside its loss/compute study, derives and tests an optimal-learning-rate law using active non-embedding parameters and expert count, including expert-count interpolation and extrapolation. | active parameters, expert count, MoE scale |
| 2025-01-31 | [The Surprising Agreement Between Convex Optimization Theory and Learning-Rate Scheduling for Large Model Training](https://proceedings.mlr.press/v267/schaipp25a.html) | Uses a convex-optimization proxy to transfer an optimal learning rate across schedule extensions and continued-training horizons in 124M and 210M Llama-style models. | schedule length, continued training |
| 2024-05-23 | [Scalable Optimization in the Modular Norm](https://proceedings.neurips.cc/paper_files/paper/2024/hash/8629b0fff229b8a27efb1422e990605f-Abstract-Conference.html) | Recursively defines an architecture-level modular norm and normalizes any base optimizer's updates so one learning rate transfers across width and block count/depth in Transformers, ResMLPs, and ResNets. | width, depth, architecture, base optimizer |
| 2024-01-05 | [DeepSeek LLM: Scaling Open-Source Language Models with Longtermism](https://arxiv.org/abs/2401.02954) | Section 3.1 fits optimal learning rate and batch size as power laws of training compute on proxy runs, validates at a larger held-out compute budget, and uses the laws in the 7B/67B scaling recipe. | compute, learning rate, batch size |

## Key Papers in the μP and Hyperball Collections

The following direct HPT papers remain in their original collection and are not duplicated in the 17-paper count above.

| Transfer problem | Key indexed work | Where to read it |
|---|---|---|
| Width-invariant proxy tuning | [Tensor Programs V](https://arxiv.org/abs/2203.03466), [An Empirical Study of μP Learning Rate Transfer](https://arxiv.org/abs/2404.05728), [Scaling Exponents Across Parameterizations and Optimizers](https://arxiv.org/abs/2407.05872) | [μP guide](mup-transfer.md) |
| Joint width and depth | [Depthwise Hyperparameter Transfer](https://arxiv.org/abs/2309.16620), [Tensor Programs VI](https://arxiv.org/abs/2310.02244), [CompleteP](https://arxiv.org/abs/2505.01618) | [μP guide](mup-transfer.md) |
| Architecture and module rules | [Principled Architecture-aware Scaling](https://arxiv.org/abs/2402.17440), [Completed Hyperparameter Transfer](https://arxiv.org/abs/2512.22382), [νGPT](https://arxiv.org/abs/2604.27077) | [μP guide](mup-transfer.md) |
| Token horizon and schedules | [Power Scheduler](https://arxiv.org/abs/2408.13359), [Scaling Optimal LR Across Token Horizons](https://arxiv.org/abs/2409.19913), [Time Transfer](https://arxiv.org/abs/2410.05838) | [μP guide](mup-transfer.md) |
| Batch size and weight decay | [Power Lines](https://arxiv.org/abs/2505.13738), [How to set AdamW's weight decay as you scale](https://arxiv.org/abs/2405.13698), [Completed Hyperparameter Transfer](https://arxiv.org/abs/2512.22382) | [μP guide](mup-transfer.md) |
| Matrix-preconditioned optimizers | [Hyperparameter Transfer Enables Consistent Gains](https://arxiv.org/abs/2512.05620), [How to Set the Learning Rate for Large-Scale Pre-training?](https://arxiv.org/abs/2601.05049) | [μP guide](mup-transfer.md) |
| Hypersphere optimization | [HyperP](https://arxiv.org/abs/2603.28743), [Hyperball](https://arxiv.org/abs/2606.16899), [HyperTransfer](https://arxiv.org/abs/2609.07017) | [Hyperball guide](hyperball.md) |
| MoE width, experts, and duration | [Hyperparameter Transfer with MoE Layers](https://arxiv.org/abs/2601.20205), [Complete-muE](https://arxiv.org/abs/2605.23893), [Let's Scale Step by Step](https://arxiv.org/abs/2608.20061) | [μP guide](mup-transfer.md) |

## Learning Resources

| Resource | Type | Why it matters |
|---|---|---|
| [μTransfer: A technique for hyperparameter tuning of enormous neural networks](https://www.microsoft.com/en-us/research/blog/%C2%B5transfer-a-technique-for-hyperparameter-tuning-of-enormous-neural-networks/) | Microsoft Research explainer | Practical introduction to base shapes, proxy sweeps, and zero-shot width transfer with μP. |
| [Greg Yang's Tensor Programs reading guide](https://thegregyang.com/) | Author-maintained guide | Organizes the Tensor Programs lineage and links talks, papers, and code behind μP. |
| [Quickstart Guide: Hyperparameter selection](https://learningmechanics.org/quickstart/hyperparameter-selection) | Learning Mechanics tutorial | Connects width/depth parameterization choices to transfer experiments and concrete diagnostics. |
| [Step Law project](https://step-law.github.io/) | Official project and calculator | Interactive entry point for the empirical model/data learning-rate and batch-size laws, with released data and checkpoints. |
| [The Modula Docs](https://docs.modula.systems/) | Official documentation | Explains modular norms, architecture composition, optimizer wrapping, and the implementation used for modular-norm transfer. |
| [Fantastic Pretraining Optimizers 2.1: Hyperball Optimization](https://whenwen.github.io/wd_blog/public/hyperball-part-1.html) | Living author note | Original Hyperball research lineage and geometric motivation for fixed-radius optimizer wrappers. |
| [The Hitchhiker's Guide to the Weight Norm Theory](https://whenwen.github.io/wd_blog/public/weight-decay-part-2.html) | Living author tutorial | Develops weight-norm and angular-step interpretations needed to reason about Hyperball schedules. |
| [On the Hypersphere: μP Scaling of Optimizers with the Hyperball Mechanism](https://jiaxuanzou0714.github.io/en/blog/2026/spherical-hyperball/) | Technical essay | Works through assumptions connecting SGDH, AdamH, MuonH, feature-space scaling, and μP. |
| [Scaling Laws That Extrapolate 300× Past the Fit](https://openathena.ai/blog/delphi/) | Primary technical report | Documents a practical Complete(d)P/AdamH scaling workflow, failed initial assumptions, held-out checks, and a hyperparameter calculator. |
| [Hyperparameter Optimization in Machine Learning](https://arxiv.org/abs/2410.22854) | Survey | Broad HPO reference useful for separating model-scale hyperparameter transfer from cross-task AutoML transfer and ordinary search methods. |

## Implementations and Artifacts

| Artifact | Framework / method | What it provides |
|---|---|---|
| [microsoft/mup](https://github.com/microsoft/mup) | PyTorch; μP / μTransfer | Reference base-shape tooling, μP layers, optimizer parameter groups, coordinate checks, and examples. |
| [modula-systems/modula](https://github.com/modula-systems/modula) | JAX; modular norm | Official package for recursively composing modules and normalizing base-optimizer updates for width/depth learning-rate transfer. |
| [function-space-learning-rates-paper](https://github.com/edwardmilsom/function-space-learning-rates-paper) | PyTorch; FLeRM | Official experiments and measurement code for matching layerwise function-space learning rates across scales. |
| [step-law/steplaw](https://github.com/step-law/steplaw) | LLM pretraining; Step Law | Official training code, loss measurements, checkpoints, and optimal-hyperparameter estimator. |
| [OpenEuroLLM dense English scaling laws](https://github.com/OpenEuroLLM/dense_english_scaling_laws) | LLM pretraining; empirical laws | Official scripts and records for the OpenEuroLLM learning-rate, batch-size, loss, and WSD phase study. |
| [OpenEuroLLM scaling-law releases](https://huggingface.co/openeurollm/dense_english_scaling_laws) | Data and models | Training measurements and model artifacts accompanying the OpenEuroLLM fits. |
| [microsoft/ArchScale](https://github.com/microsoft/ArchScale) | PyTorch / LitGPT; HyperP | Official HyperP, MuonH, SqrtGate, and width/depth/MoE scaling experiments. |
| [VITA-Group/principled_scaling_lr_init](https://github.com/VITA-Group/principled_scaling_lr_init) | Architecture-aware HPT | Official code for topology-aware initialization and maximal-learning-rate scaling across computation graphs. |
| [S-Lab-System-Group/Hydro](https://github.com/S-Lab-System-Group/Hydro) | Distributed HPO; μP proxies | Uses small μP surrogates to preserve multi-hyperparameter rankings and reduce target-scale HPO cost. |
| [fabian-sp/lr-scheduling](https://github.com/fabian-sp/lr-scheduling) | PyTorch; schedule transfer | Official experiments for convex-proxy learning-rate scheduling and transfer across schedule extensions. |
| [Joint MoE scaling-law releases](https://huggingface.co/maciek-pioro/joint-moe-scaling-laws) | Models and inference | Author-released MoE checkpoints and inference code accompanying the expert-count and active-parameter scaling study. |
| [deepseek-ai/DeepSeek-LLM](https://github.com/deepseek-ai/DeepSeek-LLM) | Models and training utilities | Official 7B/67B release accompanying the DeepSeek scaling recipe; it is not a standalone reproduction of the hyperparameter-law sweeps. |
| [NVIDIA/ngpt](https://github.com/NVIDIA/ngpt) | PyTorch; normalized Transformer | Illustrative code for nGPT's row/vector-normalized baseline; νGPT supplies the later transfer-specific scaling rules. |
| [NVIDIA NeMo Emerging-Optimizers](https://github.com/NVIDIA-NeMo/Emerging-Optimizers) | PyTorch; Hyperball | Framework implementations of MuonHyperball and Hyperball hooks; radius and tensor-group conventions must match the intended recipe. |

## Practical Transfer Protocol

1. **Name every changing axis.** Record width, depth, model shape, parameter count, tokens, batch size, sequence length, sparsity, expert count, optimizer, schedule, data mixture, and adaptation rank. A rule validated on one axis is not automatically valid on another.
2. **Choose the mechanism before the sweep.** Use μP-like invariance when the relevant parameterization is known, FLeRM when function-space updates can be measured, HyperP for the stated hypersphere setup, or an empirical law when the optimum is expected to move.
3. **Make the proxy family faithful.** Keep tokenizer, objective, data distribution, optimizer semantics, schedule shape, normalization, parameter routing, and tensor partitioning aligned with the target unless the transfer law explicitly models the change.
4. **Span the fitted axes.** A power law fitted at one model size or one token budget cannot identify a scale exponent. Use multiple proxy scales and report the range used for fitting.
5. **Hold out at least one target.** Estimate the law without the target, then compare its prediction with a local target sweep. Report both loss regret and hyperparameter error; a broad optimum may make large parameter error operationally harmless.
6. **Carry uncertainty forward.** PLES and post-training studies make this explicit, but every fitted law should expose confidence intervals, sensitivity to data recipe, and whether the target is interpolation or extrapolation.
7. **Treat tokens as an independent axis.** Matching model-size parameterization does not make learning rate invariant to training duration. A proxy can use fewer tokens only when the method supplies and validates a horizon correction.
8. **Keep a narrow safety sweep.** When target cost permits, run a small bracket around the transferred setting and preserve failed runs as evidence about the boundary of the rule.

## Common Distinctions

- **μP and Hyperball are compatible ideas, not opposites.** μP scales parameterization and optimizer groups to preserve feature dynamics; Hyperball constrains selected matrices and normalizes update directions. HyperP derives the scaling rules needed when the two concerns meet.
- **Hyperball alone is not a complete hyperparameter-transfer law.** Its original experiments reduce some width/depth learning-rate drift, while HyperP adds explicit width, depth, token-horizon, and MoE rules.
- **Original nGPT is a normalized architecture, not a demonstrated universal HPT method.** νGPT adds alignment-based rules for width, depth, and token-horizon transfer. `Training nGPT` uses scale-dependent coefficients and leaves a full scaling law for future work.
- **Proxy and target token counts are method-dependent.** Width-only μTransfer usually keeps the training setup comparable; token-horizon methods deliberately vary duration and fit a correction. There is no universal rule requiring equal or Chinchilla-matched tokens.
- **A performance scaling law is not automatically a hyperparameter law.** Direct inclusion requires a rule for an optimization hyperparameter or a validated invariant recipe, not only a prediction of loss, compute, or model/data allocation.

## Evidence Limits and Maintenance

The public-source audit covers records available through **2026-09-19**. It searched exact hyperparameter-transfer terminology, learning-rate and batch-size scaling, model/data/token/sparsity laws, parameterization methods, normalized and hypersphere training, citations in recent papers, and official code releases. Indexed search cannot prove absolute completeness, and several 2026 items remain preprints.

See the [2026-09-19 search audit](hyperparameter-transfer-search-audit-2026-09-19.md) for queries, inclusion decisions, false positives, code checks, and the incremental μP/Hyperball review. Follow [CONTRIBUTING.md](../CONTRIBUTING.md) when adding records.
