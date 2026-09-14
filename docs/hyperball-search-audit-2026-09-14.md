# Hyperball Search Audit — 2026-09-14

## Scope and Result

This audit implements the request for a dedicated Hyperball section in Awesome-Initialization. It covers the optimization method introduced by Wen, Dang, Lyu, Ma, and Liang, including AdamH/MuonH, substantive analyses, applications, implementations, and an explicitly labeled related extension. It does not broaden the separate μP collection to arbitrary optimizers.

The [guide](hyperball.md) and root [Hyperball section](../README.md#hyperball) contain **12 papers**, **16 learning resources / reports**, and **15 implementation / artifact entries**. Eleven papers directly develop, analyze, compare, or apply Hyperball; MD Decoupling is a substantive related extension with a different update-scaling convention. HyperP was already in the μP index. MACRO's independently verified μP experiment was also added there, taking that collection from 160 to 161 papers. The two collections share those two papers and therefore contain **171 unique papers** in total.

## Search Method

Search terms included `Hyperball optimization`, `Hyperball optimizer`, `MuonH`, `AdamH`, `SGDH`, `HyperP`, `HyperTransfer`, `Fantastic Pretraining Optimizers II`, `2.1`, `2.2`, `effective learning rate`, and combinations with paper, arXiv, OpenReview, pretraining, MoE, DNA, and 2026. Searches followed references and application leads through September 14, including early author materials that predate the formal paper.

Metadata and claims were checked against arXiv records and full texts, bioRxiv metadata, OpenReview manuscripts, author posts, institutional talk pages, and source repositories. The GitHub connector was used to inspect author HTML, implementation files, and public experiment records. Search snippets, mirrors, and generated summaries served as discovery leads, not as evidence of a new paper's claims.

This is a best-effort public-literature search. It cannot prove completeness for unindexed manuscripts, private code, deleted notes, or unpublished experiments. No future-dated item was included; the newest verified paper is HyperTransfer, first submitted September 7, 2026.

## Paper Evidence

| Paper | Primary evidence checked | Inclusion decision |
|---|---|---|
| [HyperTransfer](https://arxiv.org/html/2609.07017v1) | §3, appendices C/E/H; arXiv first submission September 7. | Optimizer mapping and conditional trajectory equivalence; non-invariant extension requires care about rescaled representatives. |
| [Puro-2B](https://arxiv.org/html/2608.27370v2) | §3.3 equation 1, §3.5, §4.3.1; v1 August 27, v2 September 3. | Actual MuonH pretraining/SFT application. Preserve the paper title; its title cost is not the cost of every later checkpoint. |
| [Effective Learning Rate Governs Loss Dynamics](https://arxiv.org/html/2608.24814v1) | §5, §6, appendix D; first submission August 25. | MuonH/MuonW interventions and held-out Hyperball prediction; approximate loss alignment is not exact parameter equivalence. |
| [Hyperball May Not Be a Free Lunch](https://arxiv.org/html/2607.22444v1) | §§3–5; first submission July 24. | Direct angular-step analysis and controlled schedule comparisons; finite dense-LM evidence. |
| [Regulatory DNA Transformers](https://www.biorxiv.org/content/10.64898/2026.07.17.739267v1) | BioRxiv metadata; same-DOI manuscript §§4–5, §8.2, appendix D; [author companion experiments](https://origin.bio/blogs/muon/). | AdamH/MuonH comparison with negative boundary evidence. July 22 is the posting date; July 17 in the DOI is not. |
| [Nonlinearity of LR Scaling](https://arxiv.org/html/2606.29158v1) | §6 / figure 7 / appendix B; first submission June 28. | Actual AdamH horizon-scaling experiment; μP discussion alone would not qualify it for the separate μP index. |
| [MD Decoupling](https://arxiv.org/html/2606.25971v2) | §§4.1.1–4.1.3 and §5; v1 June 24, v2 July 17. | Explicitly related extension: learnable magnitudes with constrained direction norms; not identical to the original wrapper. |
| [Original Hyperball paper](https://arxiv.org/html/2606.16899v1) | Algorithm 1, §§2–4; first submission June 15. | Canonical formal entry for the research lineage. Its independent LR-transfer experiments do not automatically make it a strict μP-method entry. |
| [MPI Routers](https://arxiv.org/html/2606.12397v1) | §§4.1–4.2, §5.2.2, appendix B.2; first submission June 10. | Actual AdamH/MuonH experiments and MuonH-based MoE scaling, distinct from its router-specific row retraction. |
| [MACRO](https://arxiv.org/html/2605.04418v1) | §5 / table 4, appendix B.2 and D.2; first submission May 6. | Direct MuonH comparisons; explicit μP-compatible initialization/radii and width sweeps also qualify it for the μP collection. |
| [HyperP](https://arxiv.org/html/2603.28743v2) | §2 equation 2, §§3.3–3.4, §4.2; v1 March 30, v2 April 5. | Direct MuonH/AdamH extension with transfer rules and limits; reuses the existing citation key. |
| [MCSD](https://arxiv.org/html/2601.21487v2) | §1 and example 3.1; v1 January 29, relevant inspected v2 August 13. | Direct theoretical discussion of projected Hyperball-style directions; the counterexample does not establish failure on real LLMs. |

BioRxiv's full-text/PDF endpoints were unavailable during this search. The same July 22 manuscript was inspected through its [ResearchGate-hosted copy](https://www.researchgate.net/publication/410722507_Muon_Reduces_the_Training_Cost_of_Regulatory_DNA_Transformers), with the DOI and version checked against bioRxiv and the method corroborated by the author's own post. The index links the canonical DOI.

## Version and Resource Decisions

- The [original combined author note source](https://github.com/WhenWen/WhenWen.github.io/blob/master/wd_blog/public/index.html) supplies a November 30, 2025 citation. The [2.1 redirect](https://whenwen.github.io/wd_blog/public/hyperball-part-1.html), legacy note, and formal paper are one lineage. The direct Notion body could not be retrieved; the author HTML and formal paper establish the relationship. The 2.2 page copies a 2.1 citation, so that field was not used to date 2.2.
- The [Princeton event](https://pli.princeton.edu/events/2026/hyperball-optimizer) includes a recording link; only event metadata was verified. The [IOS deck](https://docs.google.com/presentation/d/1t5TSjK0CzVuDUQKB196XqwUSh29Qy2z1/htmlpresent) was readable. Another author talk without a distinct public artifact was not counted again.
- [Marin 535B](https://openathena.ai/blog/marin-535b-launch-note/) is an ongoing application. The public run specification and pinned optimizer establish MuonH/AdamH usage; a planned 18T-token budget is not a completed evaluation.
- Community benchmark variants and Marin agent experiments are grouped into source collections, not promoted into separate formal papers. Translations, reposts, repository copies, redirect aliases, and duplicate benchmark links do not create new paper entries.

## Implementation Checks and Limits

- [NeMo's inspected source](https://github.com/NVIDIA-NeMo/Emerging-Optimizers/blob/36f70336da89dc80481c07dfb0af2c7333b9e5b3/emerging_optimizers/orthogonalized_optimizers/muon_hyperball.py) requires a nonzero explicit radius and validates matrix norms. Older generated API documentation exposes a different interface; the index describes the pinned source.
- The [author toy](https://github.com/WhenWen/WhenWen.github.io/blob/78142c6a673ca37fdde2cb9bab138298ed0eddd1/wd_blog/scripts/hyperball.py) omits the paper's explicit radius multiplier in the proposed step. It remains useful as a historical example, with its LR convention stated.
- The Free Lunch repository's current README describes released CSVs and training/plotting code, and explicitly excludes manuscript/LaTeX. Its stale repository description does not override the current contents.
- No separate official SGDH paper or implementation was verified. SGDH appears in Zou's analytical essay. The name alone was not used to invent an artifact.
- Source inspection establishes what an implementation contains, not successful reproduction. No GPU training, scientific benchmarks, or runtime-performance claims were independently rerun.

## Adjacent and Unresolved Candidates

| Candidate | Decision |
|---|---|
| [Summer-22B](https://arxiv.org/html/2603.00173), [Complete(d)P](https://arxiv.org/html/2512.22382) | Contextual reading; row-wise projected optimization or transfer theory is not itself a Hyperball application. Existing μP records remain. |
| [Mano](https://arxiv.org/html/2601.23000v1), [Nora](https://arxiv.org/html/2605.03769v1), [PC Layer](https://arxiv.org/html/2606.06470v1) | Hyperball is related-work context, without verified direct AdamH/MuonH evaluation. Excluded from the Hyperball paper count. |
| [Spherical Cautious Optimizers](https://openreview.net/forum?id=OyT2CJ4fh7) | Related tangent-space masking method. Inspected OpenReview text does not establish an AdamH/MuonH experiment; kept as contextual reading. The current forum/PDF route returned a browser challenge. |
| [Muown Implicitly Performs Angular Step-Size Decay](https://arxiv.org/html/2606.23637v1) | Related row-norm/angular-step method; no direct Hyperball/MuonH discussion or comparison found in the inspected version. |
| [OmniOpt](https://arxiv.org/html/2607.04033v1) | Broad optimizer survey; inspected text has no Hyperball, AdamH, or MuonH result. |
| [From LR to ELR](https://hy.tencent.com/research/elr) | Related author-report lead returned an empty JavaScript shell; not counted as a verified resource with independent claims. The formal ELR paper is included. |
| [dangxingyu/Megatron-LM-Hyperball](https://github.com/dangxingyu/Megatron-LM-Hyperball) | Author-owned repository candidate, but inspected default README was inherited and method code was not established. Not labeled a verified official release. |
| Graph HyperBall, hyperbolic packing, granular-ball methods, samplers, and games | Name collisions unrelated to this optimization method. |

## Repository Validation

Paper, resource, and artifact rows are mirrored between the root Hyperball section and its guide. The twelve paper records have matching bibliography entries; MACRO and HyperP reuse identical metadata across both bibliographies. The μP table mirrors, local document links, table column counts, reverse-date ordering, duplicate keys/identifiers, and whitespace were checked before publication. Counts refer to rows within each collection, not to every hyperlink embedded in a row.
