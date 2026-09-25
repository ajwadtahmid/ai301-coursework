I dug into the root cause: BM25Okapi divides by corpus_size (0 when empty) during initialization, causing the ZeroDivisionError. The `search()` method already has a defensive guard for empty indexes (returns []), so the fix is to add the same pattern to `index()`.

Plan: Add an empty-check guard to `KeywordSearcher.index()` that returns early if chunks is empty, matching the defensive behavior of `search()`. Remove the xfail marker from the existing test. I'll verify all tests pass including the empty-index case.
