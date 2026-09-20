# Hyperball Search Correction Audit — 2026-09-20

This incremental audit corrects the September 14 Hyperball inventory and extends the public-source check through **2026-09-20**. The collection now contains **13 papers**, **16 learning resources / reports**, and **15 implementation / artifact entries**. Twelve papers directly develop, analyze, compare, or materially apply Hyperball; MD Decoupling remains one explicitly labeled related extension. HyperP and MACRO also appear in the μP collection, so the μP and Hyperball collections contain **172 unique papers** together.

## Recovered Direct Paper

| Record | Inclusion evidence | Classification |
|---|---|---|
| [Curvature-Conditioned Multiscale Momentum with Sphere Constraints for LLM Pretraining](https://arxiv.org/abs/2608.28442) | First public 2026-08-28. Section 5.2 normalizes weights and updates on a Frobenius sphere, adds learnable radii, and parallel-transports momentum. Section 6.2 and Figure 6 directly compare tuned MuonH, SSO, MuonS, and Muon on a 0.12B dense model; Appendix B.2 reports a MuonH learning-rate grid search. The proposed MuonM method is evaluated across 0.12B–2.3B dense and MoE models. | Direct Hyperball comparison and related sphere-constraint extension; not labeled as the original fixed-radius Hyperball algorithm. |

The record was missed because neither its title nor abstract names Hyperball, AdamH, or MuonH. Full-text citation following from sphere-constrained optimizer work recovered the direct comparison. The manuscript's cover is dated August 31, while the repository uses the arXiv v1 submission date, August 28, as the first-public date.

## Artifact Check

The arXiv record and manuscript do not link an author code repository. Search results exposing third-party “request code” or reproduction pages are not treated as official implementations, so the artifact count remains unchanged.

## Incremental Search Result

The follow-up searched exact and full-text combinations of `Hyperball`, `AdamH`, `MuonH`, `Frobenius sphere`, `sphere constraint`, `learnable radius`, `effective learning rate`, `MuonS`, `MuonM`, `HyperP`, and `HyperTransfer`, then screened citation chains and first-public dates. No qualifying paper first public after HyperTransfer on 2026-09-07 was verified through September 20. Graph-centrality HyperBall, geometric balls, generic sphere optimization without a direct Hyperball experiment, and automatic summaries remain excluded.

## Files Corrected

- `README.md` and `docs/hyperball.md`: paper count, snapshot, direct-paper row, and audit links.
- `papers/hyperball.bib`: one matching BibTeX record.
- `docs/hyperparameter-transfer-search-audit-2026-09-19.md`: a dated correction explaining the earlier false negative.

The correction preserves the existing rule that table rows must be identical between the root README and Hyperball guide and must map one-to-one to the paper-only bibliography.
