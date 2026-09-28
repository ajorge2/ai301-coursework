# Evidence guide: where proof lives in a reproduction package

Every rubric check needs an evidence source. This guide maps the five
proof families to concrete places you can look: in the package bundle
when you are in eval mode, and in the student's draft and on GitHub
when you are live. It closes with how to read the bundle's captured
facts honestly. If a signal is not listed here, name your own source in
the rubric; just make it somewhere a grader can actually look.

## The four places evidence lives

Every eval bundle has the same headings, and nearly every check reads
one of them against another:

| Heading | What it holds |
|---|---|
| `## Repo facts` | the repo line, latest release, what the bug template asks for, and the contribution policy including any AI-disclosure rule |
| `## Issue` | the issue title, opener, labels, body, and the environment the reporter stated |
| `## Thread highlights` | prior comments with each author's association; the heading carries the total count, including `(0 comments total)` |
| `## Candidate claim comment` | the first comment, the one that says "I'm taking this" |
| `## Candidate repro report` | the second comment, the one carrying the proof |

Two things to keep in mind throughout. First, the repro report is free
prose: it has no subheadings, so "where it lives" inside it means the
sentence or code block that carries the signal, not a named section.
Second, most checks are comparisons — the evidence is rarely one place,
it is one place read against another.

## Live mode: the Path Review shape

Live mode reads the same five families, but the package is two comments
on one issue thread — the claim comment first, the repro report second,
both posted by you — and the issue side is a Path Review issue, which
is not shaped like the bug reports in the eval bundles. Three
differences change how the live rows below are read:

- **The issue states no environment.** Path Review issues are task
  shaped: a body, a "Relevant files" list, an "Estimated effort" line.
  The repo's `bug_report.md` template asks for Describe the Bug, To
  Reproduce, Expected Behavior, Relevant Files and Acceptance Criteria
  — no version or OS field — and most issues do not use it at all. So
  there is no issue-side environment to match. The live Environment
  check is self-consistency, not agreement.
- **There are no releases.** The repo publishes none, so "version" has
  no meaning here. The commit SHA on `main` is the anchor instead.
- **There is no AI policy.** `docs/CONTRIBUTING.md` (not the root, not
  `.github/`) is the contribution policy, and it states no AI-use or
  disclosure requirement. Silence passes; do not invent a rule.

## Environment

| | Where to look |
|---|---|
| In the bundle | the sentence in `## Candidate repro report` naming versions and OS (often opens the report, but it can sit anywhere, and in a weak package it is absent). Read it against the environment the reporter stated at the end of the `## Issue` body, and against the fields listed on the `bug reports` line under `## Repo facts` |
| Live | the Environment section of your draft repro comment. There is nothing on the issue side to compare it against, so check it against the checkout you actually used: the commit SHA `git rev-parse HEAD` reports, the branch, and whether `git status` was clean |

**What good looks like (live):** it pins a commit SHA on `main` and
names the OS and language runtime, and says whether the working tree
was clean. A named commit is what makes the run re-runnable when there
are no releases to cite.

**What good looks like (eval):** it names the version and OS the repo's
bug template asks for, and either matches the version the issue targets
or says plainly that it differs. A stated difference ("issue reports
0.63.1 on Termux, reproduced on 0.64.1 on Ubuntu") is a pass, not a
problem; a silently different version is the failure.

## Steps

| | Where to look |
|---|---|
| In the bundle | the commands in `## Candidate repro report`, usually a fenced block. Read them against the `Steps:` the reporter gave in the `## Issue` body |
| Live | the numbered steps in your draft repro comment. Read them against the issue body's own description and its "Relevant files" list, and against the repo's `docs/SETUP.md` for what a clean checkout actually requires |

**What good looks like:** a stranger with a clean machine could run
them top to bottom. The starting state is established (a clone, an
install, a fixture file created) before the trigger command appears,
and no step depends on state the report never set up. Watch for steps
that jump straight to the trigger and assume a repo, a config, or a
built binary that was never mentioned. A fixture, script or input the
issue itself supplies may be referenced rather than re-pasted, provided
the report says it used it unchanged and names any parameters it
varied — the reader has both comments in front of them.

## Behavior shown

Two signals live here: the artifact itself, and the expected-versus-
actual delta the report states around it.

### The artifact

| | Where to look |
|---|---|
| In the bundle | the fenced code block in `## Candidate repro report` holding pasted terminal output, a log, or a traceback. Read it against the error, exit code, or symptom the `## Issue` body describes, and read the command that produced it against the command the issue gives |
| Live | the fenced blocks in your draft repro comment. Read them against the issue body's description of the problem. On a docs or config issue the artifact is a file excerpt rather than terminal output: check the quoted lines against the file at the commit you pinned, and quote enough of it that the absence of something is visible (a complete short file, not a fragment) |

**What good looks like:** the artifact is pasted, not described, and it
shows the failing behavior through to the end rather than a first line.
The failure it shows is the issue's failure, not a neighbouring one.
The comparison that decides this is usually narrow and factual: an exit
code, an error string, the call site in a traceback. When the issue
supplies an exact command or input, the one in the report matches it,
or the report says why it differs.

Two shapes to watch for, because both read as confident:

- The report runs a different command than the issue gave, hits a
  different error, and narrates it as the reported one.
- The report pastes real output that shows a graceful error where the
  issue describes a crash, and calls them the same thing.

### The delta

This one lives in the text, not in a location, so read rather than
look. Somewhere in `## Candidate repro report` there should be two
separate claims: what the writer expected, and what actually happened.
They are often labelled `Expected:` and `Actual:`, but a package can
state both in plain sentences and still pass.

**What good looks like:** both sides are stated as observable outcomes,
and the gap between them is the behavior the `## Issue` body describes.
A report that gives only the actual result and leaves you to infer what
should have happened has not stated a delta — even when the
expectation seems obvious.

## Honesty

Also a reading task rather than a lookup. Take every factual claim in
`## Candidate claim comment` and `## Candidate repro report` and ask
which artifact in the package backs it. Claims about extra runs, extra
versions, extra platforms, or root causes are the ones that outrun
their evidence most often.

**What good looks like:** each claim is either backed by something in
the package or scoped so it does not outrun what was shown. The test is
whether an unshown run makes the conclusion *stronger* than the pasted
artifact supports. A claim that adds reach — a pass on another version
or platform, a root cause the output does not demonstrate — needs its
own pasted artifact. A run that narrows or qualifies the claim does
not: a control run whose result is stated inline, or a repetition count
standing behind a negative result ("ran it 5 times, never saw it"), is
honest reporting rather than overclaiming. A report that says it could
not reproduce, and shows the run that failed to reproduce, passes. A
report that asserts a match the pasted output does not show fails,
however careful it sounds.

## Comms

| | Where to look |
|---|---|
| In the bundle | `## Candidate claim comment` and `## Candidate repro report`, read against the `bug reports` and `contribution policy` lines under `## Repo facts`. The policy line names its own source, which varies: `CONTRIBUTING.md`, a named section inside it such as "Generative AI" or "AI usage policy", `.github/contributing.md`, or a separate `AI_POLICY.md` |
| Live | your draft claim comment and draft repro comment, read against `docs/CONTRIBUTING.md` and `.github/ISSUE_TEMPLATE/bug_report.md` in the Path Review repo, and against the issue body and the thread above you |

**What good looks like:** the comments carry the facts a reader needs
to act on them, follow any stated AI-disclosure or contribution rule,
and read as written about this specific issue. The repo's bug-report
template describes what *filing* an issue requires; these comments are
posted on an issue already filed, so judge whether the facts the
template cares about are present in substance — the version, the
platform, the inputs — not whether the comment reproduces its fields
or headings. Silence in the policy line is not a restriction — most
repos state nothing, and that passes. A stated disclosure requirement
with no disclosure in either comment does not.

Live, the Path Review repo states no AI-use rule, so that half of the
check passes by default and the weight falls on specificity: the
comment restates this issue's disagreement in your own words rather
than in boilerplate. Remember the house rules in `scope.md` — a
classmate's claim above you does not block you, and you post your own
repro even if someone already posted theirs.

## Reading the bundle honestly

The bundle's repo facts are captured on a stated date — `captured:
2026-08-17` on the package's front matter and in the `## Repo facts`
heading. Judge recency and "latest release" against that capture date,
not against today. Live mode measures against today.

The bundle is also the whole world in eval mode. If a claim's backing
is not in the bundle, it is not backed, however plausible it sounds and
however easy it would be to go look it up.
