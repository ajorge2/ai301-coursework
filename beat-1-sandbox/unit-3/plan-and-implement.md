# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

ajorge2

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/27#issuecomment-5985941342
The reproduction showed that a discouraging section can reach the review output unchanged because the current generator path has no tone check. It did not show that the live model currently produces discouraging feedback, since I could not run the generator without a provider key.

Following the issue's requested post-generation classification and regeneration approach, I plan to:

Add a prompt-backed constructive-versus-negative classifier to safety/content_filter.py, using actionable, specific, and encouraging versus discouraging, vague, and dismissive as the decision criteria.
Call that classifier from rag/generator/review_generator.py before accepting each generated section. A negative or unclassifiable result will trigger regeneration, bounded to three total attempts. Exhausting the limit will use the generator's existing error path instead of returning rejected feedback.
Add tests/unit/test_review_generator.py with a mocked OpenAI-compatible client covering first-pass acceptance, rejection followed by successful regeneration, repeated rejection, and malformed classifier output.
I will re-run the Unit 2 code-path check and verify that ReviewGenerator now invokes ContentFilter, then run the new unit tests, the full backend unit suite, lint, and type checking. The expected result is that constructively classified sections return normally, while discouraging, vague, or dismissive sections never reach the returned review and instead trigger the bounded retry path.

This stays limited to the issue's tone gate. It does not change the existing harmful-content regexes, bias detector, review prompts, provider configuration, or the placeholder pipeline in core/services/review_service.py. The added classifier will increase API calls and latency, and its live false-positive and false-negative rate will remain unknown until it can be exercised against a real provider.

---

## Your branch

**Branch**

feat/27-feedback-tone-check

**Evidence**

Before (from the Unit 2 reproduction, `ajorge2/pathreview-ai301-fa26-s1` fork at `f89c06f`
on `main`): feeding three sample tones directly through the safety layer showed none of
them were treated differently, and neither safety class ran during generation at all.

```
$ python tone_baseline.py
--- constructive
    ContentFilter.was_filtered = False
    BiasDetector.is_biased     = False ''
    text unchanged             = True
--- vague/dismissive
    ContentFilter.was_filtered = False
    BiasDetector.is_biased     = False ''
    text unchanged             = True
--- discouraging
    ContentFilter.was_filtered = False
    BiasDetector.is_biased     = False ''
    text unchanged             = True
```

```
$ grep -rn "ContentFilter\|BiasDetector" --include="*.py" rag/ api/ agent/ safety/__init__.py
(no matches)
```

After (same repo, on `feat/27-feedback-tone-check`, built per `plan.md`): the harmful-content
filter and bias detector are unchanged (out of scope, confirmed by re-running the identical
baseline script against the built change), but `ReviewGenerator` now calls `ContentFilter`
for every generated section, and the Unit 2 sample tones were turned into unit tests that
drive the real post-change control flow with the network boundary mocked (no live provider
key required, since the Unit 2 reproduction could not obtain one either).

```
$ python tone_baseline_check.py   # same script as tone_baseline.py in Unit 2
--- constructive
    ContentFilter.was_filtered = False
    BiasDetector.is_biased     = False ''
    text unchanged             = True
--- vague/dismissive
    ContentFilter.was_filtered = False
    BiasDetector.is_biased     = False ''
    text unchanged             = True
--- discouraging
    ContentFilter.was_filtered = False
    BiasDetector.is_biased     = False ''
    text unchanged             = True
```

```
$ grep -rn "ContentFilter\|BiasDetector" --include="*.py" rag/ api/ agent/ safety/__init__.py
rag/generator/review_generator.py:8:from safety.content_filter import ContentFilter
rag/generator/review_generator.py:91:            if ContentFilter.is_constructive(self.client, self.config.model, section.content):
```

```
$ pytest tests/unit/test_review_generator.py -v
collected 6 items

tests/unit/test_review_generator.py::TestReviewGeneratorToneCheck::test_constructive_first_pass_accepted_without_regeneration PASSED
tests/unit/test_review_generator.py::TestReviewGeneratorToneCheck::test_negative_section_is_regenerated_and_constructive_retry_returned PASSED
tests/unit/test_review_generator.py::TestReviewGeneratorToneCheck::test_repeated_negative_classifications_stop_at_three_attempts PASSED
tests/unit/test_review_generator.py::TestReviewGeneratorToneCheck::test_malformed_classification_is_not_treated_as_constructive PASSED
tests/unit/test_review_generator.py::TestReviewGeneratorToneCheck::test_classification_prompt_contains_section_and_criteria PASSED
tests/unit/test_review_generator.py::TestReviewGeneratorToneCheck::test_classification_reuses_generator_client_and_model PASSED

============================== 6 passed in 0.78s ===============================
```

```
$ make lint
.venv/bin/ruff check .
All checks passed!
```

```
$ make typecheck
.venv/bin/mypy api/ core/ ingestion/ rag/ agent/ safety/
Success: no issues found in 76 source files
```

```
$ make test-unit
================= 381 passed, 53 xfailed, 4 warnings in 11.56s =================
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

20/20

**Package analysis**

`pkg-20`: My rubric decided `reject`, matching the gold label of `reject`. The candidate's
technical plan was grounded, bounded, actionable, and decisively testable. It followed the
thread's performance constraint and proposed regression tests for both reproduced failures.
However, the Repo facts required all AI usage to be disclosed, including the tool and extent
of assistance, and the Candidate plan comment contained no such disclosure. That caused the
required `comment-context-aligned` check to fail, so the verdict rule produced `reject`.

**Check rationale**

> | comment-context-aligned | The Candidate plan comment, read against the Candidate plan, Thread highlights, and Repo facts, especially maintainer direction, contribution policy, and any AI-use or disclosure rule. | The comment faithfully summarizes the grounded diagnosis, bounded change, and decisive test without promising extra work; it respects explicit maintainer direction and includes every disclosure or communication requirement stated in Repo facts. | required |

I revised this check from an earlier `comment-faithful` version that compared the comment
with the issue, thread, reproduction, and plan but did not read Repo facts. Reviewing the
package structure and calibration examples showed that repository-specific contribution and
AI-use requirements live in Repo facts, while maintainer direction lives in Thread
highlights. I rejected a generic "the comment matches the plan" condition because it would
accept technically strong comments that ignore the repository's rules, as `pkg-20`
demonstrates.

**Trade-offs**

Making `comment-context-aligned` required means a technically excellent and buildable plan
can still be rejected for a communication-policy omission. `pkg-20` is the clearest example:
its diagnosis, implementation approach, scope, and tests were ready, but its comment omitted
Ghostty's required AI-use disclosure. I accept that strictness because the skill decides
whether a plan is ready to post as well as build, and a comment that violates the target
repository's stated policy is not ready to post.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
