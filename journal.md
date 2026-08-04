## Week 7 — Issue selection & environment setup

**Issue selected:** StructuralChunker silently drops content from documents with no markdown headings
https://github.com/anilsapkota/pathreview/issues/149

**Problem summary:**
The StructuralChunker class responsible for processing incoming documents fails to generate chunks
if a file completely lacks markdown headers. Instead of falling back to a secondary splitting
method or indexing the document as a single block, the pipeline returns an empty list and excludes
the text entirely from the RAG index. A successful fix will modify the document chunking logic
inside the ingestion/ module to handle headerless files gracefully so that valid content is no
longer silently dropped.

Branch name: fix/149-structural-chunker-empty-headings

Setup confirmation: [x] App runs locally at localhost:5173

Cohort ledger: [x] Issue added to cohort ledger

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** https://github.com/anilsapkota/pathreview/commit/f665c9b

**Reproduction summary:**
Ran the existing test `test_document_with_no_headings` using pytest and confirmed
it fails with `assert 0 >= 1` — the chunker returns an empty list when given plain
text with no markdown headings, silently dropping all content.

**PLAN.md link:** https://github.com/anilsapkota/pathreview/blob/fix/149-structural-chunker-empty-headings/PLAN.md

**Walkthrough video (recommended):** N/A

**Blockers or open questions:**
Need to confirm whether headingless content should fall back to a single chunk
or be split by token limit. Also need to run the full unit test suite after
the fix to ensure no regressions.