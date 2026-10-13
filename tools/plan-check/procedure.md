# Procedure

## Read order
1. Identify eval or live mode. Eval uses only the frozen bundle and ignores scope/voice; never fetch the live source. Live reads scope first and stops for an unfilled repo or out-of-scope issue, then reads voice-guide.md.
2. Read rubric and evidence guide before grading; stop if checks, verdict rule or procedure are empty.
3. Read issue context, thread and repo rules; record the requested outcome and explicit obligations. Read repro evidence next and record trigger, observed/expected behavior and controls before the candidate diagnosis can bias the reading.
4. Read the entire candidate plan, deviations if present, then comment. Record approach, boundaries, implementation steps, validation, uncertainty and promises.

## Evidence gathering
5. Follow every evidence-guide family: pair diagnosis/intervention with repro observations; pair scope edits with requested outcome; extract targets, operations and dependency resolutions for executability; pair validation with trigger, expected outcome and regression controls; compare certainty/risks/deviations with established observations; pair each explicit thread/repo obligation with the comment response.
6. In live mode fetch issue body/comments and applicable repository docs using GitHub/web tools; locate the student's posted repro or quoted house pack. Do not fill missing draft content from unquoted local notes. Record source locations and short deciding quotes. If needed evidence is inaccessible, record that gap rather than guessing.

## Check execution
7. Execute every rubric row in table order, even after a required failure. For each row apply only its stated pass condition to the gathered facts, rereading the specific source if a contradiction needs resolution. Grade pass when the condition is supported, fail when evidence contradicts it, unclear when necessary evidence is absent or unverifiable. Do not require headings, length, exact line numbers or a finished patch.
8. Give each check one concise evidence line naming the fact or quote that decides it. Reuse gathered facts when sufficient; never substitute intuition for a rule. Flag procedure gaps in the readable summary without inventing a missing step.
9. Live only: compare the outgoing comment with each voice rule and report violations with the rule quoted. Voice notes alone do not change the verdict unless a rubric check reads the same evidence.

## Verdict assembly
10. Apply the rubric rule: accept only when all required checks pass; a required fail or unclear rejects. Preferred grades do not change the verdict. State deciding failures and their evidence in any readable summary.
11. Emit the SKILL.md fenced JSON object with item id/URL, every check's name, pass/fail/unclear grade and deciding evidence, plus binary verdict. The JSON block is last, with nothing after it.
12. After a live deviation update, repeat the whole procedure on the updated plan/comment before posting a follow-up.
