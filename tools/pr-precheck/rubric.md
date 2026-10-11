# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-fidelity | The plan's scope, named files, test plan, and deviations; the diff's changed files and hunks; the PR title and description. | Every changed behavior and file is planned or recorded as a justified deviation, every material planned deliverable is present or explicitly deferred in both the plan and description, and the description's substantive behavior and scope claims match the shown diff. Necessary tests and required docs count as planned work. Do not fail on incidental counts or infer a contradiction from code outside the package. | required |
| decisive-evidence | The plan's repro and test plan; the candidate test evidence; tests visible in the diff; repo-required checks from Repo facts or live repo docs. | Evidence exercises the original failure path and each materially distinct promised behavior, showing an observable before/after or equivalent expected result. One representative case may cover mechanically identical paths changed in the same way. Checks the plan or repo explicitly says to run are named with concrete outcomes; declarative template claims such as "code formatted" are graded under standards-compliant. | required |
| reviewable-diff | The complete diff and commit list, read against the plan. | The intended change is visible without debug output, commented-out attempts, dead code, unexplained formatting churn, or unrelated edits. Commit wording alone does not fail an otherwise reviewable diff. | required |
| standards-compliant | Repo facts or, live, the PR template, contribution docs, linked policies, and issue thread; the candidate title and description. | Every applicable template, issue-link, maintainer, contribution, and disclosure ask has truthful substantive content. A reasoned `N/A` satisfies an inapplicable section. Live course PRs always disclose AI use in the student's own words. | required |

## Verdict rule

Return `accept` only when every required check passes. Return `reject`
when any required check is `fail` or `unclear`; `unclear` counts as
fail. Preferred checks, if added later, never change the verdict.
