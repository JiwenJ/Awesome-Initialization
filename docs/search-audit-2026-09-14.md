# μP / μTransfer search audit — 2026-09-14

This update follows the substantive μP scope in [CONTRIBUTING.md](../CONTRIBUTING.md). It extends the September 5 snapshot through September 14, with backward searches for omissions. The collection now contains **160 papers**, **54 learning / technical resources**, and **96 implementation / artifact links**: additions of **2**, **14**, and **8**, respectively. The two Markdown indexes have identical paper and resource tables; each paper has a BibTeX entry.

This is a best-effort search of publicly accessible sources, not proof of exhaustive coverage. Unindexed work, newly posted records, inaccessible submissions, and unpublished material can still be missing. The date is the search cutoff, not a claim that every source was published on that date. General hyperparameter transfer was searched for discovery, but this update retains the repository's existing μP inclusion boundary.

## Search method

- Searched arXiv metadata and full texts, OpenReview, PMLR, NeurIPS and PoS proceedings, CERN records and talks, author pages, institutional teaching sites, and paper-linked GitHub repositories.
- Used spelling variants of `maximal update parametrization`, `maximal update parameterization`, `muP`, `μP`, `μTransfer`, and `hyperparameter transfer`, plus width, depth, batch, token horizon, embedding, optimizer, MoE, Mamba, diffusion, finetuning, and application terms. Recent searches covered August–September 2026; backward searches covered the historical lineage and venue-only records.
- Compared the current [community μP index](https://github.com/francesco-innocenti/mup-papers) against the existing collection by title, arXiv ID, and URL. Alternate OpenReview/arXiv records and title changes were treated as one paper.
- Used search snippets and third-party summaries only to discover candidates. Accepted additions were checked against primary text, slides, or source code. A related-work citation or a generic scaling analogy was insufficient.

One reproducible recent arXiv API query was:

```text
(all:"maximal update" OR all:"hyperparameter transfer" OR all:"muTransfer" OR all:"CompleteP")
AND submittedDate:[202608010000 TO 202609102359]
sortBy=submittedDate&sortOrder=descending&max_results=100
```

The September 10 response contained five records: `2609.08690`, `2609.05239`, `2609.00449`, `2608.20061`, and `2608.07118`. Only `2608.20061` qualified for the μP main list, where it was already present. This query misses some TeX spellings and full-text-only applications, so it supplements the other searches rather than defining the collection.

## Final recent-record checks

The September 11 follow-up screened the September 10–11 cs.LG/stat.ML listings, a bounded arXiv query, and specific optimizer/initialization candidates. It did not verify another qualifying μP addition. A September 14 follow-up checked date-filtered web results and the [latest cs.LG](https://arxiv.org/list/cs.LG/recent) and [stat.ML](https://arxiv.org/list/stat.ML/recent) listings; no additional qualifying record was verified from those checks. The September 14 arXiv API request timed out, so it is not counted as a successful empty search. These were bounded supplementary checks, not a reread of every new full text.

## Added papers and evidence

| Paper | First public record | Inclusion evidence and limits |
|---|---|---|
| [Shaping capabilities with token-level data filtering](https://arxiv.org/abs/2601.21571) — Neil Rathi and Alec Radford | 2026-01-29 | §3.2 and Appendix A.2 explicitly use μP with depth-matched width-512 proxies to select AdamW learning rate and weight decay; Table 2 documents the target model family up to 1.816B parameters. This is a reported transfer application, without an independent target-optimum comparison. The paper links its [PyTorch implementation](https://github.com/neilrathi/token-filtering). |
| [Flavour Tagging with Graph Neural Network at ATLAS](https://cds.cern.ch/record/2912358) — Maxence Draguet, on behalf of ATLAS | 2024-10-04 | §3 and Figures 5–6 validate layer-scale behavior and peak-LR transfer across GN2 embedding widths 64, 128, and 256. The CERN record supplies the first-public date; the [PoS version](https://pos.sissa.it/476/1002/) was prepublished 2025-01-15 and published 2025-04-29. The experiments establish width transfer, not depth transfer. |

The ATLAS BibTeX entry cites the 2024 CERN report and records the 2025 proceedings DOI. The earlier conference talk is a companion resource, not another paper. The new citation keys are `rathi2026shaping` and `draguet2024flavour`; existing keys are unchanged.

## Added teaching and technical resources

| Resource | Primary evidence checked |
|---|---|
| [Chenyu Zheng's Chinese μP tutorial slides](https://chen-yu-zheng.github.io/assets/pdf/slides/2026_4_9_muP_tutorial.pdf) | April 9, 2026 author-linked tutorial; spectral conditions, Adam/Muon, DiT/PixArt transfer, and width-depth extensions. |
| [ICLR Delta Workshop width-depth slides](https://chen-yu-zheng.github.io/assets/pdf/slides/2026_4_27_ICLRW.pdf) | April 27, 2026 author talk; one- versus multi-layer residual blocks and Depth-μP / CompleteP conditions. Companion to an existing paper. |
| [muP 漫游 introduction](https://chen-yu-zheng.github.io/blog/2026/muP-01/) | Author's Chinese introduction, displayed June 16, 2026; derives the motivation in a two-layer model. Only the available first installment is counted. |
| [MarinDNA](https://openathena.ai/blog/marin-dna/) | August 3, 2026 report, “Hyperparameter transfer” section and Figures 8–11: proxy-to-target LR checks under an adapted Complete(d) / AdamH recipe. Its token exponent differs from the original prescription. |
| [Delphi: Scaling Laws That Extrapolate 300× Past the Fit](https://openathena.ai/blog/delphi/) | May 11, 2026 report: failed initial transfer recipe followed by 24 width/batch/horizon checks of an adapted Complete(d)P / AdamH recipe. Counted as an engineering report. |
| [Stanford CS336 Lecture 11](https://github.com/stanford-cs336/spring2025-lectures/blob/00191bba00d6d64621dc46ccaed9122681413a24/nonexecutable/2025%20Lecture%2011%20-%20Scaling%20details.pdf) | Official 2025 course slides: pp.6–13 discuss Cerebras-GPT/MiniCPM; pp.37–54 derive and assess μP. Relevant text was extracted and representative derivation/limitation slides visually checked. |
| [Dive into Deep Learning: Scaling Up](https://d2l.smola.org/chapter_optimization/scaling.html) | Author-hosted chapter, accessed September 2026; §9.11.2 contains width-scaled Adam rules and executable coordinate/transfer checks. No unverified original-publication date is assigned. |
| [Hyperball μP scaling essay](https://jiaxuanzou0714.github.io/en/blog/2026/spherical-hyperball/) | Jiaxuan Zou's 2026 derivation explicitly states spherical-dynamics assumptions and derives optimizer-specific rules; no independent empirical transfer claim added. |
| [Mamba / Mamba-2 μP implementation note](https://github.com/alxndrTL/mamba.py/pull/50) | July 2024 author PR and comments document WikiText LR sweeps and coordinate checks, with empirical design choices and short-run caveats. No general feature-learning guarantee for structured SSMs is inferred. |
| [ATLAS GN2 implementation talk](https://indico.cern.ch/event/1297159/contributions/5729198/) | January 30, 2024 original talk and slides 7–12, including width-64 versus width-256 checks and timing. Later slides repeating its plots were not added separately. |
| [Online KL Shampoo](https://blog.tilderesearch.com/blog/online-kl-shampoo) | July 28, 2026 author report: §4 derives μP shape scaling under a whitening approximation; §5.3 / Figure 12 tests LR transfer across model sizes. No separate public paper was verified. |
| [Nikhil Ghosh's dissertation](https://escholarship.org/uc/item/6nb8v2rj) | Summer 2024; Chapter 6 develops μFT-Transfer by subsampling/rescaling pretrained μP networks and tests finetuning transfer on CIFAR-10. No large-LLM validation is implied. |
| [Learning Mechanics: Hyperparameter selection](https://learningmechanics.org/quickstart/hyperparameter-selection) | September 1, 2025 teaching chapter on μP transfer, feature-learning strength, and depth scaling; an explanatory resource, not a new result. |
| [Go small then go home — HEP transfer](https://indico.cern.ch/event/1496673/contributions/6637963/) | September 4, 2025 FastML slides: tracking MLP, CICADA, and particle-Transformer experiments, including too-small-proxy failures and imperfect batch transfer. |

## Implementation provenance

Eight artifact rows were added. The [token-filtering repository](https://github.com/neilrathi/token-filtering), [MarinDNA hub](https://github.com/Open-Athena/marin-dna), shared [Complete(d)/AdamH recipe](https://github.com/marin-community/marin/blob/a638849fa837f924aaac66ff3d0c1f581dfdd49e/experiments/scaling_law_sweeps/completed_adamh.py), and [Online KL Shampoo release](https://github.com/tilde-research/online-kl-shampoo-release) are linked from the corresponding primary reports. The Marin configuration is commit-pinned and explicitly uses a modified token-horizon exponent.

[Mamba.py](https://github.com/alxndrTL/mamba.py) provides the code and checks behind its author PR. The [EPFL optimizer benchmark](https://github.com/epfml/llm-optimizer-benchmark) has μP GPT/Llama modules added after its original paper; including this implementation does not retrospectively classify that paper as a μP experiment. The FastML slides link [tracking-MLP experiment files](https://github.com/livaage/mup_transfer_gnn_mlp) and the [TensorFlow CICADA implementation](https://github.com/livaage/cicada-teacher-hyperparameter-transfer). The former depends on a separate GNN Tracking installation and model setup; it is not presented as a self-contained release.

## General-HPT candidates excluded from the μP main list

These links preserve the result of broader searching. They are not additional accepted papers and are not included in the paper count or BibTeX collection.

| Candidate | Decision |
|---|---|
| [Hyperparameter Scaling Laws Across MoE Sparsity](https://arxiv.org/abs/2609.08690) | September 8, 2026 empirical LR/batch scaling study. The checked full text does not implement or analyze μP; Muon use alone does not qualify. |
| [Efficiently Estimating Optimal Hyperparameter Scaling Laws through Power-Law Entropy Search](https://arxiv.org/abs/2609.01431) | Generic scaling-law estimation and acquisition method; μP-associated work appears as background rather than a substantive experimental control. |
| [Function-Space Learning Rates](https://arxiv.org/abs/2502.17405) | FLeRM directly studies generic cross-scale HPT, but μP occurs in related work rather than an implemented or analyzed μP method. |
| [Optimal learning rate scaling depends on data in deep scalar linear networks](https://arxiv.org/abs/2607.07884) | Appendices C/D adopt residual factors consistent with Depth-μP/CompleteP, but the deterministic scalar models derive maximal-stable-LR laws without implementing or testing a full maximal-update parameterization. Kept outside the strict collection; not dismissed merely because width is fixed. |
| [Over-Alignment vs Over-Fitting](https://arxiv.org/abs/2602.00827) | Studies feature-learning-strength/output-multiplier transfer, describes its construction as analogous to μP, and does not validate a complete μP recipe. |
| [Quantitative Gaussian-Process Limits of Tensor Programs](https://arxiv.org/abs/2607.06290) | Gaussian-process finite-width convergence, rather than maximal-update feature learning or μTransfer; excluded by the repository's foundation-only restriction. |
| [Cross-Domain Tracker Adaptation Without Target-Domain Labels via Vision-Language Agents](https://arxiv.org/abs/2609.05239) | Domain adaptation of tracking-system settings, rather than neural-network maximal-update scaling. |

## Unresolved or insufficiently documented leads

- [Textbook Consistency Weighted Internet Improves Efficiency Twofold](https://openreview.net/forum?id=a6bnpOInjs): an indexed primary PDF excerpt reports μP proxy transfer, but direct forum/API/full-text retrieval failed and authors, public date, and venue status were not verified. Held for follow-up rather than assigned invented metadata.
- [Command A](https://arxiv.org/abs/2504.00698): §2.4 states fixed-depth μTransfer use, but the checked material did not expose proxy dimensions, transferred settings, or an independent transfer check. Retained outside the main list under the existing evidence threshold.
- [Arcee Trinity Large](https://arxiv.org/abs/2602.17004): configuration fields and third-party claims were insufficient to establish substantive μP evidence in the report.
- [Salvi's Maths4DL slides](https://maths4dl.ac.uk/wp-content/uploads/2024/04/Salvi.pdf): promising μP teaching excerpt, but the full resource could not be verified during this audit.

Existing AK-Momentum / DeltaMomentum, GQA-μP, spectral width-depth, Muon, and variational-learning aliases were deduplicated. A later title, venue page, or companion presentation does not count as another paper. No paper-specific public implementation was invented for records such as Chimera or AK-Momentum when one could not be verified.
