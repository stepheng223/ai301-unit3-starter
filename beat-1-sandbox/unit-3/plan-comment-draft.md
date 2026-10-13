I plan to fix issue #64 by changing only `test_query_with_partial_overlap` in `tests/unit/test_relevance_scorer.py`.

My reproduction: https://github.com/codepath/pathreview-ai301-fa26-howard/issues/64#issuecomment-6090767203. Bypassing the strict xfail with `--runxfail` showed score 1.0 and the failing `0.3 < score < 0.9` assertion. The current chunk contains all four query terms, so the scorer is computing the correct full-overlap score.

I'll keep the query and assertion band, replace the chunk with "Python is a web framework for rapid development" (three of four query terms; expected score 0.75), and remove this test's strict xfail marker in the same change, following the contributing guide. I won't change the scorer or unrelated tests.

I'll re-run the full relevance-scorer test file and the targeted `--runxfail -k partial_overlap` check before and after. Success means the targeted test passes and all 19 tests pass without xfail/XPASS, including the existing perfect-match and zero-overlap controls. I'll also compare the real scorer's old/new fixture outputs, expected 1.0/0.75. This validates the relevant Python code path; I haven't checked other platforms or the full application.

AI assistance: I used Codex to help draft the plan and prepare the bounded change; the commands and results will be recorded from actual runs.
