# Learning-Rate Schedules

> A primary-source reading guide to learning-rate schedule design, from warmup and fixed-budget decay to open-ended training and effective learning rates.

This guide is linked from [Others](../README.md#others), but is not limited to μP, Hyperball, or papers that test cross-scale transfer. It cross-links papers already in the [μP](mup-transfer.md), [Hyperball](hyperball.md), and [scale-aware HPT](hyperparameter-transfer.md) collections. Foundational and comparison-only readings here do not add to the direct-paper counts.

## Contents

- [Warmup, cycles, and fixed-budget decay](#warmup-cycles-and-fixed-budget-decay)
- [Open-ended training and continuation](#open-ended-training-and-continuation)
- [Peak learning-rate and schedule transfer](#peak-learning-rate-and-schedule-transfer)
- [Online and schedule-free methods](#online-and-schedule-free-methods)
- [Effective and angular learning rates](#effective-and-angular-learning-rates)
- [Comparison checklist](#comparison-checklist)

## Warmup, Cycles, and Fixed-Budget Decay

- [Cyclical Learning Rates for Training Neural Networks](https://arxiv.org/abs/1506.01186) and [Super-Convergence: Very Fast Training of Neural Networks Using Large Learning Rates](https://arxiv.org/abs/1708.07120) study LR range tests, repeating cycles, and a single large-LR cycle. Their original evidence is primarily from vision, not LLM pretraining.
- [SGDR: Stochastic Gradient Descent with Warm Restarts](https://arxiv.org/abs/1608.03983) uses cosine decay within each cycle and restarts the learning rate without reinitializing the weights. The cosine shape can also be used as a one-shot decay without restarts.
- [Attention Is All You Need, §5.3](https://arxiv.org/html/1706.03762v7#S5.SS3) warms up linearly in optimizer steps and then decays as $t^{-1/2}$. [Why Warmup the Learning Rate? Underlying Mechanisms and Improvements](https://arxiv.org/abs/2406.09405) examines why warmup can make a higher target LR stable, rather than prescribing one warmup duration.
- [Budgeted Training: Rethinking Deep Neural Network Training Under Resource Constraints](https://arxiv.org/abs/1905.04753) treats schedule length as part of a known resource budget and finds linear decay a strong vision baseline. [Straight to Zero: Why Linearly Decaying the Learning Rate to Zero Works Best for LLMs](https://arxiv.org/abs/2502.15938) tests the decay shape and terminal LR in controlled μP / AdamW pretraining; its conclusion is tied to those conditions.
- [The Step Decay Schedule: A Near Optimal, Geometrically Decaying Learning Rate Procedure For Least Squares](https://arxiv.org/abs/1904.12838) studies the last iterate of streaming least-squares SGD. [A Second look at Exponential and Cosine Step Sizes: Simplicity, Adaptivity, and Performance](https://proceedings.mlr.press/v139/li21z.html) analyzes exponential and cosine decay under stochastic optimization assumptions. [Optimal Linear Decay Learning Rate Schedules and Further Refinements](https://arxiv.org/abs/2310.07831) derives a finite-horizon linear schedule from a convex bound and explores refinements using gradient statistics; these are different theoretical settings, not a universal ranking.

[↑ Back to Contents](#contents)

## Open-Ended Training and Continuation

- **Constant-LR trunk with a cooldown branch.** [MiniCPM, §4](https://arxiv.org/html/2404.06395#S4) describes WSD. [Scaling Laws and Compute-Optimal Training Beyond Fixed Training Durations, §5](https://arxiv.org/html/2405.18392v3#S5) measures compute savings from sharing the constant-LR prefix of a scaling ladder. Keep the pre-cooldown checkpoint, including optimizer and scheduler state, if further extension is possible.
- **Decaying trunk with a cooldown branch.** [Scaling Vision Transformers, §3.5](https://arxiv.org/pdf/2106.04560#page=6) is an early vision precedent for reciprocal-square-root decay followed by a linear cooldown. [WSqD, §1.2 and §3](https://arxiv.org/html/2607.10959v1) studies a shifted inverse-square-root trunk and reuses the same calibrated peak across tested AdamW horizons; its shift still needs calibration, and the last-iterate result is for stochastic convex optimization. [Puro-2B, §3.3.2](https://arxiv.org/html/2608.27370v2#S3.SS3.SSS2) instead uses a MuonH power-law trunk and a long linear terminal phase, with a separate peak-ELR × cooldown grid.
- **Continue after a cooldown.** [Understanding Warmup-Stable-Decay Learning Rates: A River Valley Loss Landscape Perspective, §5](https://arxiv.org/html/2410.05192v3#S5) evaluates WSD-S, which resumes at high LR after producing each cooled checkpoint. For changing data mixtures, [Simple and Scalable Strategies to Continually Pre-train Large Language Models](https://arxiv.org/abs/2403.08763) studies re-warming, re-decay, and replay, while [Beyond Cosine Decay: On the effectiveness of Infinite Learning Rate Schedule for Continual Pre-training](https://proceedings.mlr.press/v330/singh26b.html) compares an infinite schedule with repeated cosine under distribution shift. Neither protocol is the same as extending an untouched pre-cooldown trunk.

An **open-ended trunk** needs no final token budget once its parameters are fixed. A **cooldown branch** does need a length or stopping point. An **anytime output** can instead be read from an averaged model without running a separate cooldown.

For a direct check of whether branching changes the chosen hyperparameters, [Latent Feedback Is a Compute and Data Amplifier, Appendix G](https://arxiv.org/html/2611.20404v1#A7) compares reused LR/WD on WSD branches with separate grid tuning. [On Neural Scaling Laws for Weather Emulation through Continual Training](https://arxiv.org/abs/2603.25687) tests the shared-trunk protocol on Swin-based weather emulation rather than language modeling.

[↑ Back to Contents](#contents)

## Peak Learning-Rate and Schedule Transfer

- [Scaling Optimal LR Across Token Horizons](https://arxiv.org/abs/2409.19913) and [Time Transfer: On Optimal Learning Rate and Batch Size In The Infinite Data Limit](https://arxiv.org/abs/2410.05838) study how the best settings of **separate fixed-budget runs** change with the budget. A fitted peak law $\eta_{\mathrm{peak}}^*(D)$ over final token budgets $D$ is not by itself a schedule $\eta(t)$ over elapsed training $t$.
- [Power Scheduler, §3–4](https://arxiv.org/html/2408.13359v2#S4) uses a cross-budget LR fit to motivate an elapsed-token decay, then calibrates the new scheduler. [The Surprising Agreement Between Convex Optimization Theory and Learning-Rate Scheduling for Large Model Training](https://proceedings.mlr.press/v267/schaipp25a.html) transfers rates across schedule shapes and extensions using a convex-optimization proxy; see the [author's lookahead note](https://fabian-sp.github.io/posts/2025/09/horizon/) for the move from discrete stages to infinite schedules.
- [Scaling Law with Learning Rate Annealing](https://arxiv.org/abs/2408.11029) uses cumulative LR and an annealing term to predict loss under unseen schedules. [Optimal Learning Rate Schedules under Functional Scaling Laws: Power Decay and Warmup-Stable-Decay](https://arxiv.org/abs/2602.06797) derives task-dependent power-decay and WSD-like regimes in kernel regression. For a joint batch/LR change, see [Seesaw: Accelerating Training by Balancing Learning Rate and Batch Size Scheduling](https://arxiv.org/abs/2510.14717).

[↑ Back to Contents](#contents)

## Online and Schedule-Free Methods

- **State-driven adaptation.** [AutoLRS: Automatic Learning-Rate Schedule by Bayesian Optimization on the Fly](https://arxiv.org/abs/2105.10762) selects piecewise-constant rates online with short probes. [How to decay your learning rate](https://arxiv.org/abs/2103.12682) triggers decay from weight-norm behavior, a signal that is not available on a fixed-radius tensor in the same form. [SOLAR, Appendix C.5](https://arxiv.org/html/2609.34681v1#A3.SS5) combines online residual control with μP width transfer; the cosine base and fractional progress still use a planned horizon.
- **Adaptive step rules.** [DoG is SGD’s Best Friend: A Parameter-Free Dynamic Step Size Schedule](https://proceedings.mlr.press/v202/ivgi23a.html) computes step sizes from gradient norms and iterate motion, with guarantees in stochastic convex optimization rather than large-scale LLM optimality.
- **Average instead of cooling down.** [The Road Less Scheduled](https://arxiv.org/abs/2405.15682) develops Schedule-Free learning. [Anytime Pretraining: Horizon-Free Learning-Rate Schedules with Weight Averaging](https://arxiv.org/abs/2602.03702) combines open-ended schedules with online averaging. [ScheduleFree+: Scaling Learning-Rate-Free & Schedule-Free Learning to Large Language Models](https://arxiv.org/abs/2605.19095) uses a Polyak-based step rule, and [Anytime Training with Schedule-Free Spectral Optimization](https://arxiv.org/abs/2605.23061) transfers a fixed SF-NorMuon recipe across model sizes and horizons. The latter still trails horizon-aware cosine NorMuon in the tested comparisons.

[↑ Back to Contents](#contents)

## Effective and Angular Learning Rates

- [From LR to ELR: A Better Heuristic for Pretraining Dynamics](https://hy.tencent.ai/research/elr) is Tencent's primary research report on angular updates and MuonH schedule comparisons. [Hyperball May Not Be a Free Lunch, §4](https://arxiv.org/html/2607.22444v1#S4) tests angular-step alignment and shows that faster early convergence can come at a later loss cost. [Effective Learning Rate Governs Loss Dynamics in Language Model Pretraining, §4–6](https://arxiv.org/html/2608.24814v1#S4) studies conditional loss alignment by matching norm-relative trajectories, not a universal way to predict the best ELR without a reference.
- [Your LR Schedule May Not Be Principled: Effective Learning Rate Scheduling in Weight-Normalized Training, v2](https://arxiv.org/pdf/2610.40362v2) tests AIR with weight-normalized Muon, not the full Hyperball wrapper. AIR affinely rescales an inverse-square-root curve to a prescribed terminal LR over a **known** horizon. Its late-stage interventions are a reason to test the cooldown separately, not evidence that the same tail is optimal for MuonH.
- [Training nGPT, §2.2](https://arxiv.org/html/2608.01284v3#S2.SS2) studies logarithmic LR decay and angular control in normalized hybrid models. Its row/vector-wise constraints and optimizer differ from whole-matrix Hyperball.
- [Delayed Effects of Temporary Weight Decay in Muon Pretraining, §5.3](https://arxiv.org/html/2611.17876v1#S5.SS3) switches between recorded relative-step schedules at fixed norms to isolate when an early decay intervention pays off. [Muown Implicitly Performs Angular Step-size Decay](https://arxiv.org/abs/2606.23637) instead makes row-angle scheduling explicit while learning row magnitudes.

The names are easy to confuse: **inverse square root** $(1+t/\tau)^{-1/2}$ is open-ended at fixed $\tau$; **minus square root** $1-\sqrt{t/T}$ reaches zero at the chosen $T$; and **AIR** applies finite-horizon endpoint normalization to the *inverse* square root. See the [Hyperball mechanism guide](hyperball.md#mechanism-and-interpretation) before comparing ELR numbers from different definitions.

[↑ Back to Contents](#contents)

## Comparison Checklist

1. **Record the clock and endpoints.** Specify whether the scheduler counts updates or tokens; report warmup, peak LR, terminal floor, and cooldown length. Record which groups use nominal LR versus an angular or norm-relative step target.
2. **Separate reuse from quality.** On a scaling ladder, compare one reusable trunk with cooldown branches at $D$, $2D$, and $4D$ against independently tuned fixed-budget runs. Report both held-out loss and total compute, including branch and tuning work.
3. **Specify where continuation starts.** Extending a pre-cooldown checkpoint, re-warming a final checkpoint, and continuing under a changed data mixture are different tests. Keep the full optimizer/scheduler state when the aim is to preserve the training trajectory.

[↑ Back to Contents](#contents) · [Repository Contents](../README.md#contents)
