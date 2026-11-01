# Contribution: CI: reproducible benchmark harness with tracked results

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Andrew Jorge
**GitHub Username:** ajorge2
**Issue:** https://github.com/usemoss/moss/issues/435
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: I turned my issue grader into an evaluation-focused GitHub scout**

I built an issue-scout with a production-AI fit profile, five tested GitHub
queries, result-based ranking filters, and a rubric that grades five candidates
per scan. My first scan exposed that the rubric could accept repositories whose
code required private access and issues blocked by unfinished prerequisites,
while the ranking elevated frontend-only work that did not fit my profile. I
added required newcomer-access and prerequisite checks, documented their
evidence, and changed the ranking to demote frontend-only issues and prefer
profile-fit titles before comment count.

**Unit 6: I guarded upstream comments and cached Moss's benchmark context**

I consolidated my voice rules into one canonical guide and installed a
PreToolUse guard that checks every GitHub issue or pull-request body posted
through Claude Code. The first wiring attempt could not reach the hook because
my Claude login had expired; after signing in again, the required body-less
comment test was stopped and my repro-check-approved Moss claim passed. I also
forked Moss and wrote an excluded `CLAUDE.local.md` that imports the repository's
agent guidance and distills the benchmark files, commands, missing tests, and
credential-dependent CI risk into 60 lines for future sessions.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I found Moss #435 on the course Issue Selection site while looking for Python
and production-AI evaluation work. My scout regraded it alongside cmux #16159
and OpenTelemetry #7114; all three passed, but it ranked Moss first by fit and
called it ambitious because an earlier 12-file implementation in PR #446
closed without merging. I chose it because the requested latency percentiles,
recall measurements, reproducible dataset, and CI regression gate match the
kind of measurable AI reliability work I want to do. I hope to learn how to
separate hardware-sensitive performance baselines from retrieval-quality
signals and make both trustworthy in CI.

The scout was useful for separating an interesting issue from one that was
actually available, but it was not automatically reliable. Its first wild scan
needed access and prerequisite checks, its original scope check was too strict
for real issues, and its Moss difficulty note incorrectly attributed a comment
from the PR author to a maintainer. I replaced that scope check with
`scope-feasible`, added non-gating difficulty notes, and required author-role
evidence before attributing maintainer direction. I claimed the issue here:
https://github.com/usemoss/moss/issues/435#issuecomment-6553382070.

**Toolkit**

- Tool: issue-scout, using my production-AI fit profile, five GitHub queries,
  result-based ranking filters, and my rubric to scan and grade candidates.
- What it produced: It accepted Moss #435, ranked it above cmux #16159 and
  OpenTelemetry #7114, and labeled it ambitious because of closed PR #446.
- Difficulties: The initial rubric missed private-code access and unresolved
  prerequisites, its scope check was too strict for wild issues, and the Moss
  difficulty summary misread the PR author's comment as maintainer approval.
- What changed: I added newcomer-access and prerequisite checks, replaced
  `one-cohesive-change` with `scope-feasible`, added difficulty notes, demoted
  frontend-only results, and required author-association evidence for comment
  attribution.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

I used a clean clone of my `ajorge2/moss` fork on macOS 26.5.1 with Python
3.14.7. The clone's `upstream` remote points to `usemoss/moss`, and I pinned
the reproduction to current `main` commit
`2e638ec577199ef4abc8bcb99fe9ccf1497a8451`. This reproduction inspects whether
the requested infrastructure exists, so it needs no installed dependencies or
credentials. Running the live 100,000-document Moss benchmark would additionally
require `MOSS_PROJECT_ID` and `MOSS_PROJECT_KEY`.

**Steps to Reproduce**

1. From the repository root, run
   `rg -n "benchmarks|run_all\.py|bench_moss" .github/workflows` to look for a
   CI job that invokes the benchmark subsystem.
2. Run
   `rg -n -i "recall|baseline|regression|json\.(dump|dumps)|benchmark_results" benchmarks --glob '*.py'`
   to look for recall, result persistence, a baseline, or a regression gate.
3. Run
   `rg --files benchmarks | rg '(^|/)(test_.*|.*_test)\.py$'` to look for
   benchmark tests, then inspect `benchmarks/bench_moss.py`, `stats.py`, and
   `run_all.py` to see the current measurement and result path.

**Reproduction Evidence**

- **Commit showing reproduction:** None; I reproduced the missing behavior on
  the unchanged upstream commit
  [`2e638ec`](https://github.com/usemoss/moss/commit/2e638ec577199ef4abc8bcb99fe9ccf1497a8451).
- **Screenshots/logs:** All three targeted searches produced no matching lines
  and exited 1:

  ```text
  $ rg -n "benchmarks|run_all\.py|bench_moss" .github/workflows
  $ echo $?
  1

  $ rg -n -i "recall|baseline|regression|json\.(dump|dumps)|benchmark_results" benchmarks --glob '*.py'
  $ echo $?
  1

  $ rg --files benchmarks | rg '(^|/)(test_.*|.*_test)\.py$'
  $ echo $?
  1
  ```

- **My findings:** `bench_moss.py` records only elapsed milliseconds and sends
  them to `BenchmarkResult`; `stats.py` computes mean, standard deviation,
  p50, p95, and p99; and `run_all.py` prints those fields to the terminal. No
  workflow runs the benchmark, no test covers it, no recall value is computed,
  no per-commit result is persisted, and no baseline or regression threshold
  is enforced. I did not perform a credentialed live query because that would
  not test whether the missing CI and tracking infrastructure exists.

**Toolkit**

- Tool: repro-check, used first on my claim and then on `claim-draft.md` plus
  `repro-draft.md` against Moss #435.
- What it produced: It verified the pinned commit and repository evidence and
  ultimately passed environment, steps, behavior-shown, honesty, and comms.
- Difficulties: Its first package run stopped after reading only the early part
  of `scan-output.md` and wrongly said Moss had never been graded. Its next run
  correctly evaluated the report but rejected narrated results because the
  rubric requires pasted evidence.
- What changed: I directed it to the saved Moss grade's exact line range, then
  replaced result summaries with literal terminal transcripts, exit codes, and
  numbered excerpts from the three benchmark files. The revised package passed
  every check.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
