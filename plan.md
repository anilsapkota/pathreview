## Solution plan

**Issue:** StructuralChunker silently drops content from documents with no markdown headings
https://github.com/anilsapkota/pathreview/issues/149

### Understand
<<<<<<< HEAD
The root cause is in `_extract_sections` in `structural_chunker.py`. Content lines 
are only collected when `heading_stack` is non-empty, meaning any document without 
markdown headings (lines starting with `#`) never accumulates any content. The final 
save guard also requires `heading_stack` to be non-empty, so nothing is ever returned.

Expected: A document with no headings should return at least one chunk containing 
=======
The root cause is in `_extract_sections` in `structural_chunker.py`. Content lines
are only collected when `heading_stack` is non-empty, meaning any document without
markdown headings (lines starting with `#`) never accumulates any content. The final
save guard also requires `heading_stack` to be non-empty, so nothing is ever returned.

Expected: A document with no headings should return at least one chunk containing
>>>>>>> 675e7a0b2cf8191d26943fdd2e6ebbd043976158
the full text.
Actual: `chunk()` returns an empty list `[]`, silently dropping all content.

### Map
Files involved:
<<<<<<< HEAD
- `ingestion/chunking/structural_chunker.py` — where the bug lives, specifically 
  the `_extract_sections` method (~line 111 and ~line 115)
- `tests/unit/test_structural_chunker.py` — existing failing test 
=======
- `ingestion/chunking/structural_chunker.py` — where the bug lives, specifically
  the `_extract_sections` method (~line 111 and ~line 115)
- `tests/unit/test_structural_chunker.py` — existing failing test
>>>>>>> 675e7a0b2cf8191d26943fdd2e6ebbd043976158
  `test_document_with_no_headings` that will confirm the fix

### Plan
1. Open `structural_chunker.py` and locate `_extract_sections`
2. Fix the line collection guard to collect lines even when `heading_stack` is empty
<<<<<<< HEAD
3. Fix the final save guard to save content even when there are no headings, 
=======
3. Fix the final save guard to save content even when there are no headings,
>>>>>>> 675e7a0b2cf8191d26943fdd2e6ebbd043976158
   using an empty path `[]`
4. Run the failing test to confirm it passes
5. Run the full unit test suite to check for regressions

### Inputs & outputs
Input: Plain text string with no markdown headings, plus a metadata dict
<<<<<<< HEAD
Output: A list containing at least one chunk with the full text content and 
an empty heading path `[]`

### Risks & unknowns
- Unsure whether headingless content should be split by token limit or always 
  returned as a single chunk
- Need to verify the full test suite still passes after the fix — changing 
=======
Output: A list containing at least one chunk with the full text content and
an empty heading path `[]`

### Risks & unknowns
- Unsure whether headingless content should fall back to a single chunk
  or be split by token limit
- Need to verify the full test suite still passes after the fix — changing
>>>>>>> 675e7a0b2cf8191d26943fdd2e6ebbd043976158
  `_extract_sections` could affect other chunking behavior

### Edge cases
- Completely empty string input
- Document with only whitespace and no headings
- Document with headings in some sections but plain text before the first heading
- Very long headingless documents that exceed token limits