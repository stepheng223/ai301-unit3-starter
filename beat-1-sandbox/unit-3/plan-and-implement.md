# Unit 7 / Assignment 3: Plan and Implement

## Links and identifiers

- GitHub username: stepheng223
- Issue: https://github.com/codepath/pathreview-ai301-fa26-howard/issues/64
- Reproduction comment (Unit 6): https://github.com/codepath/pathreview-ai301-fa26-howard/issues/64#issuecomment-6090767203
- Posted plan comment: https://github.com/codepath/pathreview-ai301-fa26-howard/issues/64#issuecomment-6138056972
- Branch: `fix/64-partial-overlap-fixture`

## Plan comment (pasted body)

> I plan to fix issue #64 by changing only `test_query_with_partial_overlap` in `tests/unit/test_relevance_scorer.py`.
>
> My reproduction: https://github.com/codepath/pathreview-ai301-fa26-howard/issues/64#issuecomment-6090767203. Bypassing the strict xfail with `--runxfail` showed score 1.0 and the failing `0.3 < score < 0.9` assertion. The current chunk contains all four query terms, so the scorer is computing the correct full-overlap score.
>
> I'll keep the query and assertion band, replace the chunk with "Python is a web framework for rapid development" (three of four query terms; expected score 0.75), and remove this test's strict xfail marker in the same change, following the contributing guide. I won't change the scorer or unrelated tests.
>
> I'll re-run the full relevance-scorer test file and the targeted `--runxfail -k partial_overlap` check before and after. Success means the targeted test passes and all 19 tests pass without xfail/XPASS, including the existing perfect-match and zero-overlap controls. I'll also compare the real scorer's old/new fixture outputs, expected 1.0/0.75. This validates the relevant Python code path; I haven't checked other platforms or the full application.
>
> AI assistance: I used Codex to help draft the plan and prepare the bounded change; the commands and results will be recorded from actual runs.

## Test evidence (before / after)

### Before

```
OS: macOS-26.5.1-arm64-arm-64bit
Python 3.12.3
commit: 99673c7f53aa3c4666f99d38641a6ef0512a22e6

$ .venv/bin/python -m pytest tests/unit/test_relevance_scorer.py -q -rxX
..x................                                                      [100%]
=========================== short test summary info ============================
XFAIL tests/unit/test_relevance_scorer.py::TestRelevanceScorer::test_query_with_partial_overlap - issue #64: relevance scorer 'partial overlap' fixture actually has full overlap
18 passed, 1 xfailed in 0.10s

$ .venv/bin/python -m pytest tests/unit/test_relevance_scorer.py -q --runxfail -k partial_overlap
F                                                                        [100%]
E       assert 1.0 < 0.9
tests/unit/test_relevance_scorer.py:58: AssertionError
1 failed, 18 deselected in 0.04s
```

### After

```
$ .venv/bin/python -m pytest tests/unit/test_relevance_scorer.py -q -rxX
...................                                                      [100%]
19 passed in 0.09s

$ .venv/bin/python -m pytest tests/unit/test_relevance_scorer.py -q --runxfail -k partial_overlap
.                                                                        [100%]
1 passed, 18 deselected in 0.03s

$ .venv/bin/python -c "from rag.evaluator.relevance_scorer import RelevanceScorer; s=RelevanceScorer(); q='Python Django web framework'; print('old:', s.score(q,[{'text':'Django is a Python web framework for rapid development'}])); print('new:', s.score(q,[{'text':'Python is a web framework for rapid development'}]))"
old: 1.0
new: 0.75
```

Full captures in `build-evidence-before.txt` and `build-evidence-after.txt`.

## Eval iteration

### Run history

I committed one run of 20 scored packages, model `sonnet`, written by the harness to `eval-run.txt`. Order of grading as the harness recorded it:

1. First run (and the one submitted): rubric sha `c68cf67177cc3d72`, evidence-guide sha `5eb2a09c3661f46d`, procedure sha `93d64f30bab3fe89`, SKILL sha `4688d0d0f4cf6cb3`. Agreement 18/20 (meets the 18 bar).
   - Clear-accept: 6/7 (pkg-14 over-rejected)
   - Scope-creep: 4/4
   - Thread-convention: 1/2 (pkg-20 over-accepted)
   - Unbuildable: 3/3
   - Wrong-cause: 4/4

No partial `--only` or `--limit` runs were used in place of this run; a prior `--only pkg-20` sanity check was taken before this full confirming run and did not replace it. The 20/20 ceiling was not pursued because the two disagreements were, respectively, a defensible fail on honest-claims (pkg-14) and a thread-convention miss that revealed a real gap in how the rubric reads AI-policy disclosure (pkg-20), which the write-up below treats as the live-case category floor miss the assignment flags.

### Package analysis

I examined **pkg-20** (ghostty-org/ghostty#11261, category `thread-convention`). My rubric graded it **accept**; the gold label is **reject**. All six checks passed in my run, including `thread-conventions` with the evidence line "Comment follows mitchellh's recompute-on-capacity-change direction, gives own mechanism and validation; no AI use shown in bundle, so no disclosure obligation triggered."

My rubric read it that way because `thread-conventions`'s pass condition is "Judge obligations actually stated in the evidence; invent no disclosure or reservation requirement." Nothing in the plan or comment text showed overt AI use, so my rubric treated AI disclosure as not triggered. The gold is reject because the ghostty repo-facts block captures a hard contribution rule: "All AI usage in any form must be disclosed, stating the tool used and the extent of the assistance; the human in the loop must fully understand the work; AI-assisted issues and comments must be reviewed and edited by a human before submission." The thread highlights also show mitchellh referring to "the AI-proposed solution," which marks the plan lineage as AI-assisted. A reasonable grader there demands a disclosure line in the candidate comment, and its absence is the convention miss. My rubric read "no AI use shown" as "disclosure not required," when the house rule the repo-facts block records is the opposite: in this repo the burden is on the plan to disclose, not on the thread to show use.

### Check rationale

The relevant check as it reads in `tools/plan-check/rubric.md`:

> | thread-conventions | Candidate comment compared with maintainer requests, thread highlights and repo contribution/AI policy. | The comment communicates its own diagnosis, bounded approach and validation, respects applicable maintainer directions and contribution requirements, and supplies required disclosure. It does not substitute a generic promise or another person's plan for its own. Judge obligations actually stated in the evidence; invent no disclosure or reservation requirement. | required |

This check already names "repo contribution/AI policy" in the evidence column and "required disclosure" in the pass condition, so the ghostty AI_POLICY block should have driven a fail. What the pkg-20 miss shows is a procedure gap, not a rubric-text gap: the check reads the evidence column as "look for stated obligations," and my procedure for gathering the evidence did not escalate the repo-facts contribution-policy block to the same weight as thread highlights. Tightening `references/evidence-guide.md` to list "repo-facts: contribution and AI policy, verbatim" as a first-class evidence source for `thread-conventions` is the cheapest fix; the pass condition itself does not need loosening or tightening.

### Trade-offs

Loosening `thread-conventions` to always require an explicit AI-disclosure sentence would flip pkg-20 to reject, but would risk false rejects on packages where no AI use is in the thread or repo policy (several `clear-accept` packages have no AI-policy text at all; a blanket requirement would reject them). Tightening the evidence guide instead keeps the pass condition conservative while raising the signal my grader reads from the repo-facts block, which is where the ghostty AI_POLICY lives. The canary move the assignment describes applies here: before spending a confirming full run on an evidence-guide change, I would re-grade pkg-20 plus one `clear-accept` from each small category with `--only` to catch regressions, then run the full 20 once I believed the component set.

## Plan deviation

See `plan.md` for the full plan and its Deviations section. The plan held through validation: before/after test runs matched the predicted 18 passed/1 xfailed → 19 passed transition, the targeted `--runxfail` check went red → green, and the direct scorer call returned the predicted 1.0 → 0.75 scores. No scope expansion, no assertion band changes, no unrelated edits. The Deviations section records this as a held plan rather than leaving it blank.
