# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

<!-- What gets read, in what order, before any check is graded, and
what to note down from each part while reading. A complete procedure
decides the order (issue first? repro evidence first?) and says why
the order matters for the checks that come later. -->

1. Read the Issue first. Record the reported trigger, actual behavior,
   expected behavior, and the change the issue is asking for.
2. Read Repo facts. Record the contribution policy, required issue or
   comment conventions, and any AI-use or disclosure requirement.
3. Read Thread highlights. Record explicit maintainer direction,
   rejected approaches, requested tests, and unresolved technical
   claims. Keep maintainer constraints separate from claims that still
   need reproduction evidence.
4. Read Repro evidence. Record the environment, exact steps, artifacts,
   controls, actual result, and expected result. Treat reproduced
   observations and controls as the factual baseline for the diagnosis.
5. Read the Candidate plan. Record its diagnosis, in-scope and
   out-of-scope work, named files or components, selected mechanism,
   test actions and expected outcomes, and any risks, unknowns, or
   deviations.
6. Read the Candidate plan comment last. Record each claim, promise,
   test commitment, response to maintainer direction, and required
   disclosure.
7. Keep this order so the issue, repository, thread, and reproduction
   establish the baseline before the candidate's explanation can
   influence the grading.

## Evidence gathering

<!-- For each evidence family your rubric's checks name, the concrete
gathering move: which part of the package (or, live, which page or
thread location per your evidence guide) to pull the fact from, and
what to record. A complete procedure leaves no check whose evidence an
executor would have to hunt for. -->

1. For `diagnosis-grounded`, place the plan's stated cause beside each
   relevant reproduction observation and control. Note which facts
   support it and which contradict or rule it out; a technical claim in
   Thread highlights is context unless the reproduction supports it.
2. For `change-bounded`, list the requested outcome, explicit maintainer
   constraints, proposed changes, and exclusions. Mark any refactor,
   migration, feature, cleanup, or extra component that is not necessary
   to the diagnosed cause.
3. For `implementation-actionable`, extract the named files or
   components, the chosen implementation mechanism, and the first
   executable change. Record any decision the implementer would still
   have to invent or choose during the build.
4. For `test-outcome-decisive`, map the reproduction's trigger and
   current result to the plan's test action and expected post-fix result.
   Record whether the test exercises the real failure path and whether
   its observation distinguishes fixed behavior from the current bug.
5. For `uncertainty-honest`, list material uncertainties or risks exposed
   by the Issue, Thread highlights, and Repro evidence. Compare them with
   the certainty of the plan's claims and with any stated Risks,
   Unknowns, or Deviations.
6. For `comment-context-aligned`, compare every promise in the comment
   with the plan, explicit maintainer direction in Thread highlights,
   and the contribution and disclosure rules in Repo facts.
7. In eval mode, gather only from the bundle. In live mode, use the
   locations allowed by `SKILL.md` and `references/evidence-guide.md`;
   do not replace missing evidence with assumptions from elsewhere.

## Check execution

<!-- How one check runs against gathered evidence: in what order the
checks execute, what an executor does when evidence for a check is
genuinely absent, and when a check may be graded without re-reading
the whole package. A complete procedure makes two executors grade the
same package the same way. -->

1. Grade the checks in rubric order: `diagnosis-grounded`,
   `change-bounded`, `implementation-actionable`,
   `test-outcome-decisive`, `uncertainty-honest`, then
   `comment-context-aligned`.
2. For each check, use only the evidence sources named in that rubric
   row and the corresponding gathered notes. Apply every part of the
   pass condition literally; do not compensate for one failed condition
   with strength elsewhere.
3. Grade `pass` only when every part of the pass condition is supported.
4. Grade `fail` when the candidate plan or comment omits information the
   pass condition requires, contradicts the evidence, or clearly violates
   the condition. Absence from the candidate is a failure, not an
   invitation to infer the missing detail.
5. Grade `unclear` only when a source needed to judge the check is itself
   missing or genuinely ambiguous. Do not use `unclear` merely because
   the candidate omitted a required diagnosis, scope decision,
   implementation choice, test outcome, or disclosure; grade that
   omission `fail`.
6. Write one evidence line for every grade. Quote or closely identify the
   shortest fact that decides the check; for an omission, name what is
   absent and where it was required.
7. Do not repair the plan, choose an unresolved approach, or infer intent
   on the candidate's behalf. Re-read a source section only when the
   gathered notes conflict or do not contain enough information to apply
   the stated pass condition.

## Verdict assembly

<!-- How the per-check grades become the final accept or reject:
apply your rubric's verdict rule, state how unclear grades enter it,
and say what gets quoted in the output for the deciding check. A
complete procedure produces the same verdict from the same grades,
every time. -->

1. Record every check under its exact rubric name with a `pass`, `fail`,
   or `unclear` grade and its one-line deciding evidence.
2. Apply the rubric's verdict rule mechanically: return `accept` only
   when every required check is `pass`; preferred checks, if any exist,
   do not change the verdict.
3. Return `reject` when any required check is `fail` or `unclear`.
   Treat `unclear` (`?`) exactly like `fail` when computing the verdict.
4. In the readable summary, identify every required check that caused a
   rejection and include its shortest decisive quote or fact. For a
   missing item, state the required evidence that was absent instead of
   inventing a quote.
5. Emit the final JSON in the exact format required by `SKILL.md`, with
   the computed verdict, and place nothing after that JSON block.
