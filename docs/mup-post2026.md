# Post-2026 μP / μTransfer Synthesis (Engineering + Community Layer)

This document aggregates the **missing layer beyond papers** in the μP ecosystem (2024–2026):

- GitHub engineering signals
- blog-level practitioner knowledge
- optimizer / architecture failures
- post-μP frameworks
- community consensus shifts

It is intended as a companion to:
- `docs/mup-transfer.md` (theory + papers)
- `docs/mup-community.md` (already added)

---

# 1. Key Insight: μP is no longer a complete system

Across 2024–2026 usage, the consistent conclusion is:

> μP provides a **first-order scaling coordinate system**, not a complete training recipe.

Real systems require corrections along:

- optimizer geometry
- embedding scaling
- architecture-specific constraints
- sparsity / MoE routing
- schedule coupling

---

# 2. Engineering Reality (GitHub + Training Systems)

## 2.1 Common failure patterns

Observed in large-scale training stacks (Megatron, muP, unit-scaling, ArchScale):

### (1) Width transfer works — but only partially
- loss curves remain stable
- but optimal LR drifts slightly with scale

### (2) Embedding layer dominates instability
- large vocab breaks μP symmetry
- embedding LR becomes the main tuning knob

### (3) Attention variants break scaling
- GQA / MQA introduce non-uniform scaling axes
- naive μP fails without per-head correction

### (4) MoE routing breaks assumptions
- router logits scale differently from experts
- requires MSSP-like corrections

---

## 2.2 Optimizer dependence is now central

Across 2025–2026 systems:

- AdamW: baseline for μP validity
- Muon: introduces geometric constraint mismatch
- Shampoo / Sophia: curvature scaling changes transfer
- LAMB: batch scaling interacts with μP

Key conclusion:
> μP is optimizer-sensitive, not optimizer-invariant

---

# 3. Blog / Practitioner Insights

## 3.1 Cerebras & practitioner guides

Core message:

- μP must be validated via coordinate checks
- LR transfer is reliable only after **diagnostic alignment**
- embedding + output layers must be treated separately

---

## 3.2 Microsoft μTransfer workflow

Standard pipeline:

1. define μP parameterization
2. run small proxy model
3. sweep LR + WD
4. transfer to large model

Breaks when:
- architecture changes
- optimizer changes
- sequence length shifts

---

## 3.3 unit-scaling / u-μP insights

Key idea:

> normalize everything to unit scale

Benefits:
- reduces sensitivity to init
- improves FP8 robustness
- reduces variance in LR transfer

---

# 4. Post-μP Frameworks (2025–2026 evolution)

## 4.1 u-μP

- unit-scale normalization
- simplifies μP tuning surface
- improves numerical stability

---

## 4.2 CompleteP

- extends μP beyond width
- includes depth + module interactions
- prevents “lazy learning” collapse in deep nets

---

## 4.3 SμPar

- extends μP to sparsity axis
- shows transfer survives extreme sparsity (~99%)

---

## 4.4 HyperP (Muon-based systems)

- replaces parameter-space scaling with geometry constraint
- optimization on hypersphere
- claims joint transfer over multiple axes

---

# 5. Community Consensus Shift (2026 view)

The field has converged to a 3-layer view:

## Layer 1 — μP (baseline law)
- width scaling
- initialization consistency
- first-order LR transfer

## Layer 2 — corrections
- embedding LR
- weight decay
- optimizer geometry

## Layer 3 — architecture-specific laws
- GQA / MoE / LoRA
- diffusion transformers
- sparse models

---

# 6. What actually matters in practice

Empirical ranking of importance:

1. embedding LR scaling (critical)
2. weight decay tuning
3. optimizer choice
4. μP parameterization correctness
5. architecture-specific corrections

---

# 7. Key takeaway

> μP is necessary but not sufficient.

Modern large-scale training requires a **stack of scaling laws**, not a single rule.

---

# 8. How this connects to repo structure

This document complements:

- `docs/mup-transfer.md` → formal theory + paper map
- `docs/mup-community.md` → blogs + engineering signals

Together they form a **3-layer μP system map**:

- Theory layer
- Engineering layer
- Post-theory layer
