# Plan: issue #64 partial-overlap fixture

Issue: https://github.com/codepath/pathreview-ai301-fa26-howard/issues/64
Reproduction: https://github.com/codepath/pathreview-ai301-fa26-howard/issues/64#issuecomment-6090767203
Branch: `fix/64-partial-overlap-fixture`

## Diagnosis and evidence
At commit `99673c7f53aa3c4666f99d38641a6ef0512a22e6`, the query "Python Django web framework" and the chunk "Django is a Python web framework for rapid development" share all four query tokens. The posted reproduction reports score 1.0 and the failure `assert 1.0 < 0.9` when `--runxfail` bypasses the strict marker. The scorer computes intersection size divided by query-token count, so this fixture is full overlap. The fixture needs correction.

## Bounded change and implementation
Change only `tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap`.
1. Re-run the two posted pytest commands before editing and save their outputs.
2. Replace the chunk with "Python is a web framework for rapid development". It shares Python, web and framework, but excludes Django: 3/4 coverage, expected score 0.75.
3. Remove this test's strict xfail decorator in the same edit, as contributing docs require when a seeded bug is fixed.
4. Keep the query and the existing assertion band `0.3 < score < 0.9`.

No scorer algorithm changes, assertion relaxation, unrelated fixtures or cleanup. No plan document will be committed on the fix branch; this note stays in the course submission.

## Validation
From the build checkout, create `.venv` and install pytest and structlog (the relevant scorer path imports only structlog; this avoids unrelated application dependencies).
Before and after the edit run:
```
.venv/bin/python -m pytest tests/unit/test_relevance_scorer.py -q -rxX
.venv/bin/python -m pytest tests/unit/test_relevance_scorer.py -q --runxfail -k partial_overlap
```
Before: expect 18 passed/1 xfailed and the forced assertion failure with score 1.0. After: expect 19 passed with no xfail/XPASS and the targeted partial-overlap test passing. The full file includes the perfect-match and zero-overlap controls. Also call the real RelevanceScorer directly with old/new chunks to verify 1.0 and 0.75, respectively.

## Risks and unknowns
Validation covers the installed Python version and the real scorer/test-file path, not the full application or other platforms. The replacement must omit Django under the actual lowercase/split tokenizer; inspect the token intersection before accepting the change. Repository-wide PR/CI validation belongs to the following PR unit.

## Deviations
No deviations. The plan held through validation: the full relevance-scorer suite moved from 18 passed / 1 xfailed to 19 passed with no XPASS, the targeted `--runxfail -k partial_overlap` run went from the forced `assert 1.0 < 0.9` failure to pass, and a direct call to `RelevanceScorer.score` returned 1.0 for the old chunk and 0.75 for the replacement, matching the predicted intersection sizes. The chunk edit and the strict xfail removal were the only edits; the query, assertion band and surrounding tests were not touched; no scorer code was changed. Before/after captures are in `build-evidence-before.txt` and `build-evidence-after.txt`.
