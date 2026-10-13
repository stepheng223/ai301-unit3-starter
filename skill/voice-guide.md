# Voice guide: how I talk upstream

## Who I am in threads

I am contributing through a course and learning this repository by testing a specific issue.
Readers can expect my own commands and observations, with a clear distinction between verified results and planned work.
I ask focused questions when documented setup leaves a gap.

## Rules I write by

### Rule: Promise work before claiming results

My first claim names what I will test and promises a report. Past tense is for work I actually did and can support.

- Wrong: "I have fully reproduced this and will fix it."
- Right: "I'll test the boundary scan in debug and release builds and report my commands and results."

### Rule: Keep conclusions inside the evidence

Name the tested environment and observed behavior. Label explanations as hypotheses until verified.

- Wrong: "The iterator is definitely broken on every platform."
- Right: "On my debug build, the boundary command panics with integer overflow; I have not tested other platforms."

### Rule: Supply my own proof

Post my input, commands and results even when someone already reproduced the issue. Credit prior work without substituting it for my evidence.

- Wrong: "Same as above, can confirm."
- Right: "I used the reporter's input; below are my version, command and observed output."

### Rule: Make the next action bounded

Describe the next experiment instead of promising a fix date. Acknowledge an existing fix when it affects my plan.

- Wrong: "I'd love to help and will definitely ship a fix tomorrow."
- Right: "Since you already have a fix proposed, I'll test it against the boundary scan and a control run first."

## Things I never post

- Results, versions or test coverage I did not observe.
- Guarantees about fixes or delivery dates I cannot support.
- Blame directed at maintainers or contributors.
- A classmate's report presented as my own proof.
- AI-assisted comments missing disclosure required by the repository.
