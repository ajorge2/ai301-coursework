# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/135

**Branch**

`feat/27-feedback-tone-check`

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Environment preflight: `agreement: 0/0 scored items` because the
   sandboxed Claude CLI exited before grading any package.
2. Five-package diagnostic: `agreement: 4/5 scored items`.
3. Repaired check plus canaries: `agreement: 2/2 scored items`.
   (`calib-04` also agreed but is not scored.)
4. First full run: `agreement: 17/20 scored items  (bar: 18/20: below the bar)`.
5. Three repaired accepts plus canaries: `agreement: 6/6 scored items`.
6. Confirming full run: `agreement: 19/20 scored items  (bar: 18/20: PASS)`.

**Package analysis**

`pkg-11`: my rubric returned `reject`; the gold label is `accept`. The
deciding evidence was: "Description claims tests for the 'unescaped'
case, but the diff's single test only covers escaped (foo\\   ) and
double-backslash (foo\\\\   ) inputs; no unescaped-trailing-space test
exists." `plan-fidelity` requires substantive description claims to match
the shown diff, so the claimed third test case caused the rejection. The
gold label instead treats the PR as ready because its core escaped-space
repro, fix, regression test, focused diff, and required disclosure are
all present.

**Check rationale**

> | decisive-evidence | The plan's repro and test plan; the candidate test evidence; tests visible in the diff; repo-required checks from Repo facts or live repo docs. | Evidence exercises the original failure path and each materially distinct promised behavior, showing an observable before/after or equivalent expected result. One representative case may cover mechanically identical paths changed in the same way. Checks the plan or repo explicitly says to run are named with concrete outcomes; declarative template claims such as "code formatted" are graded under standards-compliant. | required |

The first version required proof for every changed sibling path and
treated every template statement as a command result. That rejected
`pkg-02` for testing `save_text` rather than all three mechanically
identical `save_*` methods, and rejected `pkg-08` because "code
formatted" had no separate command output. I revised it to require the
original failure and every distinct behavior, while allowing one case
for identical paths and routing non-command template claims to the
standards check.

**Trade-offs**

Representative evidence can miss a defect that affects only one sibling
path even when the diff appears to change all siblings mechanically. I
accepted that risk to avoid requiring duplicate runtime proof for the
same mechanism. After loosening the check, `pkg-02` and `pkg-08` changed
to `accept`, while the `pkg-07` not-tested canary and `calib-04`'s missing
second failure mode remained `reject`.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
