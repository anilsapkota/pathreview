Week 7 — Issue selection
Issue link: https://github.com/ascherj/pathreview/issues/149

Issue title: Structural chunker silently drops documents that contain no headings

Tier: [x] Tier 1  [ ] Tier 2  [ ] Tier 3

Problem summary:
The StructuralChunker class responsible for processing incoming documents fails to generate chunks if a file completely lacks markdown headers. Instead of falling back to a secondary splitting method or indexing the document as a single block, the pipeline returns an empty list and excludes the text entirely from the RAG index. A successful fix will modify the document chunking logic inside the ingestion/ module to handle headerless files gracefully so that valid content is no longer silently dropped.

Branch name: fix/149-structural-chunker-empty-headings

Setup confirmation: [x] App runs locally at localhost:5173

Cohort ledger: [x] Issue added to cohort ledger