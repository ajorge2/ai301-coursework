# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | The Candidate plan's diagnosis or stated cause, read against the Issue's reported and expected behavior and the Repro evidence's observations, artifacts, and controls. Use Thread highlights as context, not as proof when the repro evidence tests the same claim. | The proposed cause explains the reproduced failure and is supported by, and not contradicted or ruled out by, the observations and controls. | required |
| change-bounded | The Candidate plan's Scope, Changes, and Approach, read against the Issue and any explicit maintainer direction in Thread highlights. | The proposal is one coherent change tied to the diagnosed cause; any additional work is necessary to that change, and it does not bundle drive-by refactors, migrations, features, or unrelated cleanup. | required |
| implementation-actionable | The Candidate plan's Diagnosis, Scope, Changes, and Approach, especially the named files or components and the selected implementation mechanism. | Another contributor could identify where to begin and what change to make without having to invent the approach or choose among unresolved implementation directions. Line numbers, multiple files, and a fully written patch are not required. | required |
| test-outcome-decisive | The Candidate plan's Test plan, read against the Issue's reported failure and the Repro evidence's trigger, steps, controls, actual result, and expected result. | The test repeats the reported failure or exercises it through an equivalent real-code path and names an observable post-fix result that distinguishes the fix from the current bug. A precise manual test may pass; an automated test is not required, and a generic full-suite run alone does not pass. | required |
| uncertainty-honest | The certainty of claims throughout the Candidate plan, including any Risks, Unknowns, and Deviations, read against unresolved points in the Issue, Thread highlights, and Repro evidence. | Claims are no stronger than the available evidence, and any material unknown or implementation risk revealed by the package is identified and bounded instead of being presented as settled. Do not require a separate risks section when the package reveals no material uncertainty. | required |
| comment-context-aligned | The Candidate plan comment, read against the Candidate plan, Thread highlights, and Repo facts, especially maintainer direction, contribution policy, and any AI-use or disclosure rule. | The comment faithfully summarizes the grounded diagnosis, bounded change, and decisive test without promising extra work; it respects explicit maintainer direction and includes every disclosure or communication requirement stated in Repo facts. | required |

## Verdict rule

Return `accept` (ready) only when every required check is `pass`.
Return `reject` (hold) when any required check is `fail` or `unclear`
(`?`); for the verdict, `unclear` counts the same as `fail`. Preferred
checks, if any are added later, never change the verdict.
