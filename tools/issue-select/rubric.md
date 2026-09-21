# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-active | Eval item `captured` date; Repo facts `last 5 default-branch commits`; Repo facts `maintainer first-response sample`; issue comment thread author associations | Pass when at least one of the last 5 default-branch commits is within 90 days of capture and is either human-authored or a bot merge of a human PR, OR an Owner/Member/Collaborator responded to a non-maintainer issue within 90 days before capture. Automated dependency/version/leaderboard updates alone do not pass. | required |
| repo-in-use | Eval item `captured` date; Repo facts repo line including `archived`; Repo facts `last push to any branch`; Repo facts `latest release` | The repository is not archived, and either its last push is within 365 days of capture or its latest release is within 365 days of capture. A missing release is acceptable when the push condition passes. | required |
| one-cohesive-change | Issue title and body; full issue comment thread | Fail only when the issue explicitly identifies itself as an umbrella, tracking, or megaissue, or explicitly requests open-ended/codebase-wide work for which the contributor must first choose a target or subtask. Otherwise pass. Several named files, several instances of one defect, a list ending in `etc.`, optional/lower-priority follow-ups, multiple possible causes, and multiple implementation ideas are all compatible with one cohesive change and must not cause failure. | required |
| specification-ready | Issue title and body; full issue comment thread | Fail only when the text explicitly marks an input essential to the core change as TBD, or Owner/Member/Collaborator comments show an unresolved disagreement about the expected product behavior. Otherwise pass; do not infer a missing specification from terse, incomplete, or truncated prose, a non-maintainer author, zero comments, lack of formal acceptance criteria, multiple files/items, `etc.`, or uncertainty about implementation rather than outcome. Optional or lower-priority follow-ups do not affect this check. | required |
| attempt-history | Repo facts `this issue: linked PRs`; full issue comment thread | Pass when there are fewer than two closed-unmerged linked PRs. Fail when there are at least two closed-unmerged linked PRs unless a later Owner/Member/Collaborator comment explicitly says the earlier blockers are resolved or obsolete. Merged PRs do not count as abandoned attempts. | required |
| unclaimed | Repo facts `this issue: assignees` and linked-PR states; issue comment thread | There are no assignees, no open linked PRs (including PRs from forks), and no still-active claim or in-progress implementation stated in the comments. Closed-unmerged or merged PRs are not active claims. A claim is stale only when a later comment explicitly withdraws it, a maintainer/bot explicitly unassigns or reopens the issue to takers, or the claimant's associated PR is closed and no newer active claim exists. | required |
| ai-allowed | Repo facts `contribution policy` | Pass when AI-assisted code/documentation is allowed, allowed with conditions, discouraged but permitted with human review, or no policy is stated. Fail only for an outright ban on AI-generated code or documentation. | required |

## Verdict rule

<!-- State how the grades above combine into accept or reject, and how
unclear is treated. Example shape (write your own): "accept if every
required check passes; preferred checks never change the verdict, they
rank accepted issues; unclear counts as fail." -->

Accept if and only if every required check passes. Reject if any required check fails or is unclear. Preferred checks, if added later, may rank accepted issues but never change the verdict.
