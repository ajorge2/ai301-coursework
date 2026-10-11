# Procedure: how this tool grades a PR package

## Read order

1. In live mode, validate `scope.md`; then read the issue, full thread,
   template, contribution guide, and linked policies. In eval mode,
   read Issue, Thread highlights, and Repo facts from the bundle.
2. Read the plan before the PR. Record its scope, files, exclusions,
   test promises, and deviations.
3. Read the complete diff and commits. List changed files and the purpose
   of each hunk without relying on the description.
4. Read test evidence and map it to the plan's repro and test plan.
5. Read the title and description last, then verify each claim against
   the facts already recorded. In live mode, also apply the voice guide.

## Evidence gathering

1. For `plan-fidelity`, pair every changed file and behavior with a plan
   line or deviation; pair every planned deliverable and description
   claim with a diff hunk or explicit, consistently disclosed deferral.
   Judge material behavior and scope, not incidental counts. Do not
   infer effects through callers or files the package does not show.
2. For `decisive-evidence`, map the original failure path and each
   materially distinct promised behavior to a command, observed result,
   and expected result. Group paths only when the diff changes them by
   the same mechanism. Record every check the plan or repo explicitly
   says to run and its reported outcome. Route non-command checklist
   claims to `standards-compliant`.
3. For `reviewable-diff`, scan every added line and changed hunk for
   debris, churn, and unrelated work; use commits only as supporting
   context.
4. For `standards-compliant`, make a list of applicable asks from the
   template, policies, and maintainer direction, then locate truthful
   content for each in the title or description.

## Check execution

1. Grade checks in rubric order using only their named evidence and the
   gathered pairings.
2. Give `pass` only when the whole pass condition is supported. Give
   `fail` for a clear violation or an omitted required item. Use
   `unclear` only when a source needed to decide is missing or genuinely
   ambiguous, not when the candidate simply omitted required evidence.
3. Record one short deciding fact or quote per check. Do not repair the
   PR, infer hidden work, or let strength in one check offset another.

## Verdict assembly

1. List every check with its exact rubric name, grade, and one-line
   evidence.
2. Apply the rubric rule mechanically: any required `fail` or `unclear`
   yields `reject`; otherwise yield `accept`.
3. In a rejection summary, name all failing required checks; the first
   in rubric order is the deciding check.
4. End with the exact JSON block required by `SKILL.md`, and write
   nothing after it.
