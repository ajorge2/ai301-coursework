---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Decide one thing: is one PR package ready to submit? A package is its
title, description, commits, diff, and test evidence, read against the
accepted plan, its issue and thread, and the repository's stated rules.

## Inputs and modes

- **Live mode:** work from the repository root. Read the issue URL and
  filenames named in the request: `plan.md` including `## Deviations`,
  `pr_draft.md`, and `test_evidence.md`. Read the committed branch change
  with `git diff main...HEAD`, its file list with
  `git diff main...HEAD --stat`, and its commits with
  `git log --oneline main..HEAD`. Read the full issue thread, the repo's
  `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`, and any
  policy they link to. A house-chain student uses the supplied house
  plan and repro pack in place of their own.
- **Eval mode:** the supplied package bundle is the whole world. Fetch
  nothing and read no other files. Run every rubric check and the full
  verdict rule.

## The scope seam (live mode only)

Read `scope.md` first and apply its repo and house rules. Refuse work
outside its Repo. If Repo still contains a placeholder, stop without
grading and tell the student to fill it. Ignore `scope.md` in eval mode.

## The voice seam (live mode only)

Read `voice-guide.md` and check the draft title and description against
it. Report each broken rule in the readable summary. Voice alone does
not change the verdict unless a rubric check reads it. Ignore the voice
guide in eval mode.

## Component reads

Read `rubric.md` for the checks and verdict rule,
`references/evidence-guide.md` for evidence locations, and execute
`procedure.md` exactly. If the procedure is silent, report the gap; do
not invent a step. If the rubric has no checks or the procedure has no
steps, refuse to grade.

## Verdict and output

`accept` means ready to submit; `reject` means hold. A short per-check
summary may precede the output. End with this valid fenced JSON block,
with nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Cite the shortest deciding fact for every grade; "looks fine" is not
  evidence.
- Grade the artifacts, not their length or polish.
- Apply the rubric literally. Fix a bad rule in the rubric, never in a
  one-off verdict.
- Follow the procedure as written and report gaps instead of improvising.
- Apply the rubric's `unclear` rule; if it is silent, count unverifiable
  evidence as failing.
