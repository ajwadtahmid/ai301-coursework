# Plan: Fix KeywordSearcher.index() ZeroDivisionError on empty corpus

## Diagnosis

The `KeywordSearcher.index()` method crashes with `ZeroDivisionError` when passed an empty chunks list because:

1. **Root cause**: BM25Okapi library divides by `corpus_size` (0 when corpus is empty) in its initialization
2. **Context**: The `search()` method already handles empty gracefully (returns [] at lines 38-40)
3. **Inconsistency**: The defensive pattern exists in search() but is missing in index()
4. **Evidence**: 
   - Repro evidence shows `ZeroDivisionError: division by zero` in BM25Okapi._initialize() at `self.avgdl = num_doc / self.corpus_size`
   - xfail test at tests/unit/test_keyword_search.py:134-143 expects index([]) to succeed

## Scope

**In scope**:
- Add empty-check guard to `KeywordSearcher.index()` (lines 17-26)
- Update the xfail test to remove the @pytest.mark.xfail decorator
- Verify all existing tests pass

**Not in scope**:
- Changes to BM25Okapi library (external dependency)
- Changes to search() method (already handles empty correctly)
- Performance optimizations
- Changes to other retriever classes

## Files to modify

- `rag/retriever/keyword_search.py` (the index method)
- `tests/unit/test_keyword_search.py` (remove xfail on test_empty_index)

## Approach

1. In `KeywordSearcher.index()` method, add an empty-check guard at the start:
   - If chunks is empty, set self.bm25 = None and self.chunks = [] (matching the init state)
   - Log a warning consistent with search()'s warning
   - Return early

2. This mirrors the defensive pattern in search() (lines 38-40) and allows subsequent search() calls to return [] gracefully

3. Remove the @pytest.mark.xfail decorator from test_empty_index()

## Test plan

1. **Unit test verification**: Run pytest on test_empty_index() - should pass after removing xfail
2. **Regression test**: Run full test suite on test_keyword_search.py - all 17 tests should pass
3. **Manual verification**: Reproduce the original bug scenario and verify it now succeeds:
   ```python
   searcher = KeywordSearcher()
   searcher.index([])  # Should not raise ZeroDivisionError
   results = searcher.search("test", top_k=10)  # Should return []
   ```

## Unknowns and risks

- **Risk**: None identified - this is a defensive guard with no side effects
- **Certainty**: High - the pattern already exists in search() and the fix is straightforward
- **Testing**: Complete - existing test already covers the scenario, just needs xfail removed

## Implementation order

1. Add empty-check guard in index() method
2. Remove @pytest.mark.xfail from test_empty_index
3. Run tests to verify
4. No other changes needed
