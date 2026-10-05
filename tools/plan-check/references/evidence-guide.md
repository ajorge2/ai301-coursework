# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

<!-- Where the plan states its cause, and where the repro evidence
pins down the behavior that cause must explain. What it means for a
diagnosis to follow from the evidence rather than contradict or
ignore it. -->

**Where it lives:** In an eval bundle, find the proposed cause in the
Candidate plan's Diagnosis, Cause, or Summary and compare it with the
Issue's reported and expected behavior and every observation, artifact,
and control in Repro evidence. Use Thread highlights to locate hypotheses
or maintainer analysis, but do not treat an untested thread claim as
stronger than a reproduction result. In live mode, use the diagnosis and
quoted evidence in `plan.md`, the GitHub issue body and thread, and the
student's posted repro comment; for a house issue, use the repro-pack
evidence quoted in the drafts.

**What good looks like:** The stated cause explains the reproduced
failure, including any comparison or control, and is not contradicted or
ruled out by those observations. A diagnosis that merely repeats a
confident thread claim while ignoring a contrary control is not grounded.

## Scope

<!-- Where the plan bounds itself: the in-scope statement, the
not-in-scope line, the files or areas named. What one bounded change
looks like next to a drive-by rewrite. -->

**Where it lives:** In an eval bundle, read the Candidate plan's Scope,
Changes, and Approach against the outcome requested in the Issue and any
accepted, rejected, or constrained direction in Thread highlights. In
live mode, use the same parts of `plan.md` and compare them with the live
issue thread. Named files, components, in-scope work, and exclusions may
appear anywhere in the plan; do not require a particular heading.

**What good looks like:** The proposal is one coherent change tied to the
diagnosed cause, and its named files or areas form a credible boundary.
Extra tests or documentation may belong when they support that change;
unrelated refactors, migrations, features, and while-in-the-area cleanup
do not.

## Executability

<!-- Where the plan says what will actually be done: files or areas,
approach, order of work. What it means for a stranger to be able to
start executing without asking the author anything. -->

**Where it lives:** In an eval bundle, inspect the Candidate plan's
Diagnosis, Scope, Changes, and Approach for the affected files or
components, the selected mechanism, and the first concrete implementation
move. In live mode, use those parts of `plan.md`; supporting repository
paths may clarify a named area, but they must not supply an approach the
draft itself never chose.

**What good looks like:** Another contributor can identify where to begin
and what behavior or mechanism to change without first deciding among
unresolved alternatives. Exact line numbers, multiple files, and a
fully written patch are unnecessary, but plans to "investigate," "poke
around," or fix whichever layer is easiest are not executable.

## Test plan

<!-- Where the plan says how success will be observed, and how that
maps onto the repro evidence's steps and artifacts. What a decisive
test plan names that a vague one does not. -->

**Where it lives:** In an eval bundle, compare the Candidate plan's Test
plan with the trigger, steps, artifacts, controls, actual result, and
expected result in Repro evidence and with the behavior requested in the
Issue. In live mode, compare the test plan in `plan.md` with the student's
posted repro comment or the house repro evidence quoted in the drafts.

**What good looks like:** The test repeats the failure or exercises it
through an equivalent real-code path and names the observable result that
must change after the fix. A precise manual check can be decisive and an
automated regression test can be decisive; "run the full suite" or
"nothing feels broken" alone cannot distinguish the fix from the current
bug.

## Honesty

<!-- Where claims meet uncertainty: risks, unknowns, and deviations.
How to tell stated unknowns from false confidence, and where an
honest mid-build deviation gets recorded. -->

**Where it lives:** In an eval bundle, inspect certainty and qualifiers
throughout the Candidate plan, especially any Risks, Unknowns, and
Deviations, and compare them with unresolved points in the Issue, Thread
highlights, and Repro evidence. In live mode, use the same parts of
`plan.md`; after implementation, read the `## Deviations` section for
what changed from the posted plan and why.

**What good looks like:** Claims are no stronger than the available
evidence, and material unknowns or implementation risks exposed by the
package are named and bounded. A separate risks section is unnecessary
when no material uncertainty is present, but an obvious open decision
cannot be hidden behind confident language; an actual build deviation
states both the change and its reason.

## Comms

<!-- Where the words meet the thread and the repo: the plan comment
read against the issue's maintainer signals (thread highlights, or
the live thread) and against the repo-facts block's stated templates,
contributing asks, and contribution policy (including AI-use
disclosure requirements). What thread-aware looks like next to
boilerplate. -->

**Where it lives:** In an eval bundle, compare the Candidate plan comment
with the Candidate plan, Thread highlights, and Repo facts, especially
maintainer direction, rejected approaches, requested tests, contribution
rules, and AI-use or disclosure requirements. In live mode, compare
`comment.md` with `plan.md`, the GitHub issue thread, and the repository's
issue template, contribution guide, and stated AI policy.

**What good looks like:** The comment accurately summarizes the grounded
diagnosis, bounded change, and decisive test without adding promises that
the plan does not support. It responds to explicit maintainer direction,
does not revive a rejected approach without justification, and includes
every disclosure or communication requirement the repository states.
