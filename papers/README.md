# Reference Files

This directory stores BibTeX files for the paper collections in this repository.

## Files

| File | Covers |
|---|---|
| [mup-transfer.bib](mup-transfer.bib) | μP, μTransfer, direct maximal-update / feature-learning theory, extensions, evaluations, and substantive applications. |
| [hyperball.bib](hyperball.bib) | Direct Hyperball papers: the original method, analyses, extensions, comparisons, and material applications. |
| [hyperparameter-transfer.bib](hyperparameter-transfer.bib) | Complementary scale-aware HPT papers not already counted in the strict μP or Hyperball bibliographies. |

## Maintenance Notes

- Keep citation keys stable after they are added.
- Prefer arXiv or official publication URLs.
- Preserve Greek-letter titles when the source title uses them.
- Keep μP papers synchronized across the root `README.md`, `docs/mup-transfer.md`, and `mup-transfer.bib`; keep its learning-resource and artifact rows mirrored between the two Markdown indexes.
- Keep Hyperball papers synchronized across the root `README.md` Hyperball section, `docs/hyperball.md`, and `hyperball.bib`; mirror its learning-resource and artifact rows between those Markdown indexes.
- Keep complementary HPT papers synchronized across the root `README.md` scale-aware section, `docs/hyperparameter-transfer.md`, and `hyperparameter-transfer.bib`; mirror its learning-resource and artifact rows between those Markdown indexes.
- Reuse citation keys and identical bibliographic metadata for papers present in both collections. Collection counts overlap and must not be summed as unique papers.
- Author notes, talks, blogs, code links, and explicitly labeled adjacent reading do not increase the direct-paper counts or receive entries in these paper-only bibliographies.
- Do not add generic initialization, optimizer, scaling-law, or hyperparameter-transfer papers to the μP collection unless μP is substantively used or analyzed. The Hyperball collection instead requires substantive Hyperball evidence. The complementary HPT collection requires a direct optimization-hyperparameter rule or invariant recipe over a stated model/training scale axis, as specified in [CONTRIBUTING.md](../CONTRIBUTING.md).
