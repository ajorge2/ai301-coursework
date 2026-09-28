# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment | **In the bundle:** the sentence in `## Candidate repro report` naming versions and OS (often opens the report, but it can sit anywhere, and in a weak package it is absent). Read it against the environment the reporter stated at the end of the `## Issue` body, and against the fields listed on the `bug reports` line under `## Repo facts`. **Live:** the Environment section of your draft repro comment. There is nothing on the issue side to compare it against, so check it against the checkout you actually used: the commit SHA `git rev-parse HEAD` reports, the branch, and whether `git status` was clean. | **Eval:** it names the version and OS the repo's bug template asks for, and either matches the version the issue targets or says plainly that it differs. A stated difference ("issue reports 0.63.1 on Termux, reproduced on 0.64.1 on Ubuntu") is a pass, not a problem; a silently different version is the failure. **Live:** it pins a commit SHA on `main` and names the OS and language runtime, and says whether the working tree was clean. A named commit is what makes the run re-runnable when there are no releases to cite. Fail if a version the issue pins is absent from the record or silently different. | required |
| steps | **In the bundle:** the commands in `## Candidate repro report`, usually a fenced block. Read them against the `Steps:` the reporter gave in the `## Issue` body. **Live:** the numbered steps in your draft repro comment. Read them against the issue body's own description and its "Relevant files" list, and against the repo's `docs/SETUP.md` for what a clean checkout actually requires. | A stranger with a clean machine could run them top to bottom. The starting state is established (a clone, an install, a fixture file created) before the trigger command appears, and no step depends on state the report never set up. Watch for steps that jump straight to the trigger and assume a repo, a config, or a built binary that was never mentioned. A fixture, script or input the issue itself supplies may be referenced rather than re-pasted, provided the report says it used it unchanged and names any parameters it varied — the reader has both comments in front of them. Fail if a step is missing, depends on state that neither the report nor the issue establishes, or the report gives no steps at all. | required |
| behavior-shown | Two signals live here: the artifact itself, and the expected-versus-actual delta the report states around it. **The artifact, in the bundle:** the fenced code block in `## Candidate repro report` holding pasted terminal output, a log, or a traceback. Read it against the error, exit code, or symptom the `## Issue` body describes, and read the command that produced it against the command the issue gives. **The artifact, live:** the fenced blocks in your draft repro comment. Read them against the issue body's description of the problem. On a docs or config issue the artifact is a file excerpt rather than terminal output: check the quoted lines against the file at the commit you pinned, and quote enough of it that the absence of something is visible (a complete short file, not a fragment). **The delta:** lives in the text, not in a location, so read rather than look. Somewhere in `## Candidate repro report` there should be two separate claims: what the writer expected, and what actually happened. They are often labelled `Expected:` and `Actual:`, but a package can state both in plain sentences and still pass. | Both of these hold. (a) The artifact is pasted, not described, and shows the failing behavior through to the end rather than a first line. The failure it shows is the issue's failure, not a neighbouring one. The comparison that decides this is usually narrow and factual: an exit code, an error string, the call site in a traceback. When the issue supplies an exact command or input, the one in the report matches it, or the report says why it differs. Fail if the artifact shows a different failure from the issue's (a parse or syntax error where the issue reports a panic, a crash at a different call site), even when the report asserts they are the same. Two shapes to watch for, because both read as confident: the report runs a different command than the issue gave, hits a different error, and narrates it as the reported one; or the report pastes real output that shows a graceful error where the issue describes a crash, and calls them the same thing. (b) Both sides of the delta are stated as observable outcomes, and the gap between them is the behavior the `## Issue` body describes. A report that gives only the actual result and leaves you to infer what should have happened has not stated a delta — even when the expectation seems obvious. Fail if only one side is given, or if the reader has to infer the expectation. | required |
| honesty | A reading task rather than a lookup. Take every factual claim in `## Candidate claim comment` and `## Candidate repro report` and ask which artifact in the package backs it. Claims about extra runs, extra versions, extra platforms, or root causes are the ones that outrun their evidence most often. | Each claim is either backed by something in the package or scoped so it does not outrun what was shown. The test is whether an unshown run makes the conclusion *stronger* than the pasted artifact supports. A claim that adds reach — a pass on another version or platform, a root cause the output does not demonstrate — needs its own pasted artifact. A run that narrows or qualifies the claim does not: a control run whose result is stated inline, or a repetition count standing behind a negative result ("ran it 5 times, never saw it"), is honest reporting rather than overclaiming. A report that says it could not reproduce, and shows the run that failed to reproduce, passes. A report that asserts a match the pasted output does not show fails, however careful it sounds. | required |
| comms | **In the bundle:** `## Candidate claim comment` and `## Candidate repro report`, read against the `bug reports` and `contribution policy` lines under `## Repo facts`. The policy line names its own source, which varies: `CONTRIBUTING.md`, a named section inside it such as "Generative AI" or "AI usage policy", `.github/contributing.md`, or a separate `AI_POLICY.md`. **Live:** your draft claim comment and draft repro comment, read against `docs/CONTRIBUTING.md` and `.github/ISSUE_TEMPLATE/bug_report.md` in the Path Review repo, and against the issue body and the thread above you. | The comments carry the facts a reader needs to act on them, follow any stated AI-disclosure or contribution rule, and read as written about this specific issue. The repo's bug-report template describes what *filing* an issue requires; these comments are posted on an issue already filed, so judge whether the facts the template cares about are present in substance — the version, the platform, the inputs — not whether the comment reproduces its fields or headings. Silence in the policy line is not a restriction — most repos state nothing, and that passes. A stated disclosure requirement with no disclosure in either comment does not. Live, the Path Review repo states no AI-use rule, so that half of the check passes by default and the weight falls on specificity: the comment restates this issue's disagreement in your own words rather than in boilerplate. Fail on an undisclosed AI-assisted submission where the repo requires disclosure, or on a claim comment that is generic enough to sit under any issue. | required |

## Verdict rule

Accept (ready to post) if every `required` check passes. A fail on any
required check holds the package. A `preferred` check, if one is added
later, never changes the verdict — it only shapes the feedback.
`unclear` on a required check counts as a fail: if a grader cannot
locate the evidence, neither can a maintainer.
