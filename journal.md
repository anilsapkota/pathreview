Week 7 — Issue selection
Issue link: https://github.com/ascherj/pathreview/issues/149

Issue title: Structural chunker silently drops documents that contain no headings

Tier: [x] Tier 1  [ ] Tier 2  [ ] Tier 3

Problem summary:
The StructuralChunker class responsible for processing incoming documents fails to generate chunks if a file completely lacks markdown headers. Instead of falling back to a secondary splitting method or indexing the document as a single block, the pipeline returns an empty list and excludes the text entirely from the RAG index. A successful fix will modify the document chunking logic inside the ingestion/ module to handle headerless files gracefully so that valid content is no longer silently dropped.

Branch name: fix/149-structural-chunker-empty-headings

Setup confirmation: [x] App runs locally at localhost:5173

Cohort ledger: [x] Issue added to cohort ledger



## Week 8 — Reproduction & solution planning

**Reproduction commit link:** [link to commit documenting the reproduced issue]
https://github.com/anilsapkota/pathreview/commit/f665c9b


**Reproduction summary:**
[1–2 sentences: How did you reproduce the issue? What did you observe?]
Ran the existing test `test_structural_chunker` using pytest and confirmed it fails with `assert 0>=1` the chunker returns an empty list when given plain text with no markdown headings, silently dropping all content.

**PLAN.md link:** [link to PLAN.md in your fork]

**Walkthrough video (recommended):** [link to your Loom video, ≤2 min — recommended, not graded]

**Blockers or open questions:**
[Anything you're still uncertain about going into Week 9, or leave blank]