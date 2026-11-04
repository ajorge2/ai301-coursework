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
**Pull Request:** https://github.com/usemoss/moss/pull/571

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

**Unit 7: I added a ship-gate and checks for the benchmark branch**

I installed the ship-gate beside my voice guard and wrote a manifest with
fresh-shell commands for repository lint, benchmark compilation, and 24
offline tests. The recheck found two promised failure tests missing, and the
opening hook later stopped because the environment-loading change was
undocumented and it could not infer the issue from `Refs #435`. I added the
tests, documented the change, and put the full issue URL in the plan and PR
description without weakening the rubric. The final gate passed all three
repository checks and all four pr-precheck criteria; both hooks then passed
and Claude Code opened upstream PR #571.

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

Moss has a benchmark people can run manually, but changes can reach the
repository without a repeatable measurement of search speed or quality.
Issue #435 asks for CI measurements, saved per-commit results, and a way to
catch regressions. A fast search is not enough if it returns worse results.

**Expected Behavior**

Run a fixed workload, record latency percentiles and a defined retrieval-quality
measure, compare results against reviewed limits, and keep a published history.
The report should identify the workload and runtime so that numbers from
different machines or model versions are not mistaken for comparable runs.

**Current Behavior**

The existing runner compares Moss, Pinecone, Qdrant, and Chroma and prints
latency statistics. At my reproduction commit, no workflow invokes it and no
per-commit report, relevance score, or regression gate exists. The timing corpus
contains 100,000 generated text variants of 20 base sentences; it does not
include an independent relevance answer key for the 15 benchmark queries.

**Affected Components**

`benchmarks/bench_moss.py`, `corpus.py`, `stats.py`, and `run_all.py` contain the
existing workload, loading, timing, and printing. The contribution adds a local
Moss runner, JSON reporting, an optional latency guard, a competitor-agreement
collector and scorer, offline tests, and `.github/workflows/benchmark.yml`.
GitHub settings and project credentials are also needed to activate live CI.

**Toolkit**

- Tool: Codex with local source/history inspection and read-only GitHub research.
- What it produced: A repository-context notebook tracing the benchmark, corpus,
  previous PRs, SDK packaging, and credential requirements.
- Difficulties: I initially treated a smaller corpus and an available shared
  index as reasonable shortcuts before establishing what they would measure
  or whether contributors could still access that index.
- What changed: I kept the full existing corpus, verified the local-session
  route, and separated measured facts from proposed infrastructure. Closed
  PRs and an index proposal do not establish current maintainer direction or
  working shared access.

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

The benchmark stops at terminal output. `bench_moss.run_async` measures queries
and `stats.BenchmarkResult` computes percentiles, but `run_all.py` does not save
a CI record and broadly catches engine failures. Its exit status therefore
cannot serve as a reliable benchmark gate. Corpus loading also depends on a
remote Moss index rather than consistently using the checked-in JSON file.

There are two separate measurement problems. Latency depends on the runner,
model, workload, and warmup policy. Retrieval quality needs an explicit
reference; agreement with other engines is useful evidence but is not
independent relevance recall. The benchmark imports an installed Moss release,
so the report must identify that runtime rather than imply it builds the SDK
from the checked-out source.

**Proposed Solution**

Extend the existing Moss workload: load all 100,000 checked-in documents, build
a fresh local session index, reuse the timing loop, and write an atomic JSON
record with measurements, versions, workload identity, and failure status.
Keep the generated index, model files, credentials, and local results out of
the contribution.

Run offline checks for pull requests. Make live Ubuntu measurements an opt-in
for trusted `main` runs, with secrets available only to that step. Add an
optional p95 guard against a reviewed baseline and a separate report-only
quality signal: Moss's coverage of sentence templates returned by at least two
of Pinecone, Qdrant, and Chroma on the same text queries.

This is a staged contribution. It does not yet provide a calibrated Ubuntu
baseline, a real three-engine reference, independent relevance recall, or
durable published history. Temporary workflow artifacts preserve individual
reports while the history location is still a proposal. I will not claim that
#435 is complete until its remaining requirements are resolved.

**Implementation Plan**

1. Validate the fixed corpus and share the existing query-measurement function
   between the legacy cloud runner and a fresh local-session runner.
2. Add complete success/failure JSON and offline tests for timing boundaries,
   result counts, cleanup, missing credentials, and sanitized errors.
3. Wire trusted, opt-in live measurements into CI; upload only the result JSON.
4. Add the opt-in p95 comparison and the saved competitor-reference scorer.
   Validate the reference's corpus, queries, model metadata, and original IDs.
5. Capture a real reference, calibrate a baseline on the intended Ubuntu
   runner, and resolve durable history and the requested recall definition
   before treating this as the full issue fix.

The plan changed during investigation. I dropped the proposed 1,000-document
shortcut to preserve the existing workload and chose fresh local indexing
after the cloud path encountered an account-credit restriction. I also changed
the reference collector from shared precomputed embeddings to each engine's
text-search path, recording its model. These changes keep the measurement
honest, but the different models limit what agreement can tell us.

I also moved environment-file loading from module import time into the legacy
benchmark entry points. Those commands still load their environment when
invoked; the CI runner can import the shared functions without discovering a
local credentials file. The opening hook caught that this intentional change
was missing from my plan and description, so I documented it in both.

**Toolkit**

- Tool: Claude Code's plan-check, plus Codex source research and independent
  review of the proposed boundaries.
- What it produced: Both the initial plan and the revised package passed all
  six plan-check criteria. The revised result covers the guard, agreement
  reference, test evidence, and documented unfinished work.
- Difficulties: The original idea mixed latency automation with unresolved
  questions about relevance labels, shared access, and durable publication.
- What changed: I made those boundaries explicit and recorded later additions
  to the latency guard and agreement reference. The revised check accepted the
  plan. Claude could not follow the voice-guide symlink during that run, so
  Codex read the canonical guide and checked the local comment separately.

---

## Testing Strategy

I saved the before/after evidence in the excluded `test_evidence.md` and kept
the original reproduction above. The original commit has no benchmark workflow
invocation or benchmark tests. The current branch has a workflow invoking the
offline suite and live runner, plus an exact-JSON upload step. That proves the
wiring exists in source. After opening PR #571, the executed GitHub Actions
jobs completed successfully. The live Moss measurement was skipped as intended
for a pull request; those checks do not establish live Ubuntu performance.

On the current branch, these checks pass:

- `.venv/bin/python -m unittest discover -s benchmarks/tests -v`: 24 tests.
- `.venv/bin/python -m compileall -q benchmarks`: exit 0.
- Repository Ruff check with generated SDKs and notebooks excluded: all checks
  pass, using Ruff 0.15.22.
- `actionlint .github/workflows/benchmark.yml`: exit 0 with actionlint 1.7.12.
- `git diff --check upstream/main...HEAD`: exit 0.

The tests cover complete 750-sample output, failure and incomplete results,
credential redaction, explicit environment loading, reference validation,
protected input paths, and the guard's boundary. A p95 of 115 ms passes a
115 ms limit and fails a 114.9 ms limit while retaining the measurements.
A zero agreement score is reported
without failing the run. These are controlled fixtures, not live performance
or relevance evidence.

I also ran the first-stage production entry point on my Mac against all
100,000 documents. It built a fresh local index and completed 750 measured
queries with exit 0: p50 13.608 ms, p95 32.357 ms, and p99 75.283 ms. The JSON
contained the actual counts, finite samples, and workload/runtime identity.
This validates that entry point, not a Linux threshold or the later optional
guard and reference in a real CI run.

A real Qdrant/Chroma smoke test on 261 documents returned five original IDs
for every one of the 15 queries. Full-corpus Pinecone capture, a real saved
reference, Ubuntu calibration, live Moss CI activation, and durable history
remain open.

**Toolkit**

- Tool: unittest, Ruff, compileall, actionlint, real local benchmark runs, and
  independent review of the result writer and workflow.
- What it produced: Passing offline checks, a complete real 100,000-document
  result, a real CLI failure record when credentials are absent, and successful
  executed GitHub Actions jobs on the PR.
- Difficulties: A fake SDK can prove harness behavior but cannot prove search
  quality or live speed. The early test suite also checked too many internal
  details and felt heavy for the change.
- What changed: I reduced repetitive fixtures and assertions while preserving
  observable contracts, and kept fixture, Mac, sample-collector, and actual CI
  evidence distinct. Independent review also caught overlapping-secret
  redaction; longest-first replacement and a regression case fixed it.

---

## Implementation Notes

**2026-11-03 — Established the workload and local route**

I traced the old benchmark and prior work before building. The corpus has
100,000 generated entries, not 100,000 independently labeled examples. I
rejected reducing it to 1,000 for convenience. The cloud-index attempt hit an
account-credit restriction; I verified a fresh local session instead. Local
indexing still needs project access, so I did not describe the live run as
credential-free. I kept the generated index out of Git.

**2026-11-03 — Built and exercised the first CI stage**

I added the fixed-file loader, shared timing loop, local builder, JSON runner,
offline tests, and trusted opt-in workflow on `fix/435-benchmark-ci`. The real
entry point indexed all 100,000 documents and completed 750 measured queries.
Setup and indexing are outside the query-latency samples. The report identifies
the installed Moss 1.22.0/core 0.36.0 runtime. After reviewing the exact patch,
I approved its push to my fork; no upstream PR or workflow run was opened.

**2026-11-03 — Simplified the patch and added optional comparisons**

I trimmed repetitive tests and unnecessary report metadata. Then I noticed
that a complete but slower run would still pass, so I added a p95 guard that
keeps the measurements when it fails. I chose competitor agreement as a
report-only signal rather than authoring relevance labels myself. Duplicate
variants count once per sentence template and each engine gets one vote.
Queries without consensus remain unscored instead of becoming perfect or zero
scores. The real local Qdrant/Chroma sample succeeded; Pinecone remains pending.

**2026-11-03 — Prepared the contribution for review**

At this stage, the reviewed fork branch was at `d22b9b5`, where 21 offline
tests passed. I prepared a local PR description using Moss's template and disclosed the AI
assistance and unfinished measurements. Neither baseline nor reference is
committed or enabled. History still needs a durable publication decision.
The draft references #435 without claiming closure; the course asks for a
closing fix, so I need to resolve that scope before presenting this stage as
the completed assignment contribution. The revised plan-check passed all six
criteria. I narrowed the title and maintainer question based on the gate's
voice-guide feedback. The final branch gate passed, and the outgoing PR was
then awaiting my review.

**2026-11-03 — Fixed a review finding before opening the PR**

Review found that an output path could overwrite an input file. The runner
now rejects the credentials file and fixed corpus as output destinations
before writing. A focused regression test brings the suite to 22 passing
tests. At this point, the fix was local in `6da41ae` and had not been pushed.
I have not rerun the full 100,000-document workload after this review fix.

**2026-11-03 — Closed gaps found by the branch gate**

The gate's recheck found that the trimmed suite lacked two tests named in my
plan. I added direct checks that a failed local build closes both contexts and
that a foreign reference ID is rejected before a live measurement starts.
The suite now passes 24 tests. These tests were recorded in local commit `64494cb`.
The final ship-gate passed all three manifest commands and all four
pr-precheck criteria using the revised title and local PR description.

**2026-11-03 — Opened the upstream pull request after review**

I approved publishing the reviewed story and review commits, and pushed the
Moss branch through `64494cb` to my fork. The first opening attempt was stopped
by the hook: the environment-loading timing change needed to be documented,
and its issue lookup needed the full URL rather than only `Refs #435`. I
updated the plan and description, reran the gate successfully, and opened
[PR #571](https://github.com/usemoss/moss/pull/571) through Claude Code with both
hooks passing. It targets `usemoss/moss:main` from my fork's
`fix/435-benchmark-ci` branch. Executed GitHub Actions jobs passed; the live
Moss measurement was skipped on the PR. Human maintainer feedback is pending.

**Toolkit**

- Tool: Codex for implementation and research, Claude Code for plan-check,
  and private contribution/context scratchpads for decisions and evidence.
- What it produced: A reviewed implementation branch, dated decisions, saved
  test transcripts, and an upstream PR without publishing the private notes.
- Difficulties: It was easy to confuse a proposed design with a verified
  resource, or a successful Mac run with an Ubuntu baseline. The initial
  shared-embedding collector also did more setup than the comparison needed.
- What changed: I recorded limitations as they arose, moved to same-text
  engine-managed search with model provenance, and required review before
  each external push or post. I kept generated indexes, credentials, caches,
  and scratch documents out of the contribution.

---

## Pull Request

**PR Link:** https://github.com/usemoss/moss/pull/571

**PR Description:** The PR describes the reproducible Moss runner,
trusted opt-in CI, saved result artifacts, optional p95 guard, and report-only
competitor agreement. It includes before/after evidence, passing checks, AI
assistance, and the remaining baseline, reference, recall, and history work.
It also explains why environment loading moved into the legacy entry points.
It uses `Refs #435` and the full issue URL; a closing claim depends on completing
or explicitly resolving the unfinished scope.

**Maintainer Feedback:**

- 2026-11-03: No human maintainer feedback yet. CodeRabbit and cubic's automated
  reviews are in progress; their activity is separate from maintainer approval.

**Status:** Awaiting review.

**Toolkit**

- Tool: Moss's PR template, the saved test evidence, and the installed
  ship-gate/voice-guard hooks, with Claude Code opening the reviewed PR.
- What it produced: The final branch gate passed repository lint,
  compilation, 24 tests, and all four pr-precheck criteria. Both creation hooks
  passed, and PR #571 opened against the original repository.
- Difficulties: The recheck rejected plan-fidelity because the trimmed tests
  omitted cleanup on failure and the CLI's early rejection of foreign IDs.
  After those fixes, the opening hook caught the undocumented environment-loading
  change and could not find an issue URL from `Refs #435` alone.
- What changed: I added the two focused failure tests, narrowed the question
  to the interim quality signal, and named the optional p95 guard in the title.
  I documented the environment change and added the full issue URL to the plan
  and description, then reran the gate and exact opening command successfully.
  I did not weaken the rubric or change the issue-lookup helper.

---

## Learnings and Reflections

**Technical Skills Gained**

I learned to separate index-building time, cold-query time, warmup, and steady
query latency. I also learned why reproducibility needs workload and runtime
identity, why failure JSON and nonzero exits matter in CI, and why secrets
must stay out of untrusted pull-request jobs. Agreement between engines can
reveal differences, but does not establish that their shared result is relevant.

**Challenges Overcome**

The main challenge was defining a useful measurement before automating it.
Tracing the synthetic corpus, installed SDK, old shared-index proposal, and
local-session option prevented me from publishing numbers with misleading
claims. I also fixed a real partial-secret redaction case and made missing
credentials fail visibly. Simplifying the tests after review helped keep the
patch understandable while preserving the failure cases that matter.

**What I'd Do Differently Next Time**

I would trace data provenance and contributor access before proposing a subset
or asking someone to set up a cloud index. I would agree on the quality metric
and result-history destination earlier, and distinguish an initial deliverable
from the full issue in the first plan. That would reduce rework and make it
easier to ask maintainers a concrete question when the draft is ready.

---

## Resources Used

- [Moss #435](https://github.com/usemoss/moss/issues/435) defined the requested
  latency, recall, regression, and history behavior.
- [Moss's contribution guide](https://github.com/usemoss/moss/blob/main/CONTRIBUTING.md)
  supplied the repository checks and contribution conventions.
- [Closed PR #446](https://github.com/usemoss/moss/pull/446) and
  [local benchmark PR #554](https://github.com/usemoss/moss/pull/554) helped me
  distinguish previous attempts and other datasets from this workload.
- [Moss session documentation](https://docs.moss.dev/docs/integrate/sessions)
  helped establish the local-session route and its project-access requirement.
- [GitHub's workflow syntax documentation](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax#jobsjob_idconcurrency)
  exposed the default pending-run cancellation behavior; I removed a global
  concurrency group so intermediate measurements would not be displaced.
