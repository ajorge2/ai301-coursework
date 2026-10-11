# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

**Where it lives:** In eval mode, compare Plan context, including its
files and deviations, with Candidate PR Diff and Description. In live
mode, compare `plan.md` and `## Deviations` with
`git diff main...HEAD`, its `--stat`, and `pr_draft.md`.

**What good looks like:** Every changed file and behavior is planned or
justified by a recorded deviation, every material promised deliverable
exists or is consistently deferred, and substantive scope and behavior
claims match the shown diff. Extra work with no note and claimed work
missing from the diff are drift; incidental counts and effects requiring
unshown code are not deciding evidence.

## Test evidence (harness category: not-tested)

**Where it lives:** In eval mode, compare Plan context's repro and test
plan with Candidate PR Test evidence and tests in the Diff. In live
mode, compare `plan.md` with `test_evidence.md`, changed tests, and the
checks required by the PR template and `docs/CONTRIBUTING.md`.

**What good looks like:** The original failing path is exercised and its
visible result changes to the planned outcome. Each distinct promised
behavior is covered; one representative may cover paths changed by the
same mechanism. Commands the plan or repo requires include concrete
outcomes; non-command checklist claims belong to standards and comms.
Confidence, an unchanged-path control, or "tests pass" alone proves
nothing.

## Diff quality (harness category: unreviewable)

**Where it lives:** In eval mode, read the entire Candidate PR Diff and
Commits. In live mode, read `git diff main...HEAD`, its `--stat`, and
`git log --oneline main..HEAD`.

**What good looks like:** The fix, its tests, and required documentation
are easy to isolate. No debug prints, commented-out attempts, dead code,
drive-by cleanup, or unexplained formatting churn rides with them.

## Standards and comms (harness category: standards-wall)

**Where it lives:** In eval mode, read Repo facts and Thread highlights
against the Candidate PR title and description. In live mode, read the
full issue thread, `.github/PULL_REQUEST_TEMPLATE.md`,
`docs/CONTRIBUTING.md`, linked policies, and `pr_draft.md`.

**What good looks like:** Every applicable stated ask receives truthful,
substantive content: the issue is linked, template sections are filled,
maintainer direction is honored, and required AI use is disclosed. A
reasoned `N/A` is content; untouched boilerplate is not.
