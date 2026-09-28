# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

ajorge2

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/27#issuecomment-5860980025
Picking this up. Nothing checks the tone of generated feedback right now, so a discouraging section ships the same as a constructive one. Next I'll run the generator on a few inputs picked to pull different tones and post what comes back as a baseline.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/27#issuecomment-5861163895
## Result

I could not reproduce discouraging generated feedback. I was not able to get the
generator to produce any feedback at all, for the reason below, so I cannot say what
tone it actually writes in. What I could check is what happens to a discouraging section
once it exists, and nothing in the current path stops one.

## Environment

- OS: macOS 26.5.1 (arm64)
- Python: 3.14.6
- Repo: `ajorge2/pathreview-ai301-fa26-s1` (my fork) at `f89c06f` on `main`, clean tree
  apart from the script below
- Docker services: not started. No `OPENROUTER_API_KEY` set.

## What I tried, and why it stopped

`rag/generator/review_generator.py` builds its client directly:

```python
self.client = openai.OpenAI(api_key=config.api_key, base_url=config.base_url)
```

There is no mock branch on that path. `LLM_PROVIDER=mock`, the default in
`.env.example`, only covers embeddings: the mock class is `MockEmbeddingProvider` in
`ingestion/embeddings/provider.py`, and nothing in `rag/generator/` reads
`llm_provider`. So running the generator needs a live API key, which I do not have. That
is the part of this issue I could not reproduce.

## What I could check instead

Feed sections of different tones through the safety layer directly. Save as
`tone_baseline.py` in the repo root, with `structlog` installed:

```python
from safety.bias_detector import BiasDetector
from safety.content_filter import ContentFilter

SECTIONS = [
    ("constructive",
     "Your README explains the setup steps clearly. Adding a short example "
     "of the output would help a first-time reader know it worked."),
    ("vague/dismissive",
     "This project is pretty weak overall. Not much here worth talking about."),
    ("discouraging",
     "Honestly, this portfolio is a mess and nobody is going to read past the "
     "first project. You are wasting your time applying with this."),
]

for label, text in SECTIONS:
    filtered, was_filtered = ContentFilter.filter(text)
    is_biased, reason = BiasDetector.detect_bias(text)
    print(f"--- {label}")
    print(f"    ContentFilter.was_filtered = {was_filtered}")
    print(f"    BiasDetector.is_biased     = {is_biased} {reason!r}")
    print(f"    text unchanged             = {filtered == text}")
```

Expected: the discouraging section is treated differently from the constructive one.

Actual: all three come back identical.

```
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

Neither class runs during generation in any case:

```
$ grep -rn "ContentFilter\|BiasDetector" --include="*.py" rag/ api/ agent/ safety/__init__.py
(no matches)
```

`ContentFilter` has no caller anywhere in the repo, and `BiasDetector` has no caller
outside its own tests in `tests/unit/test_bias_detector.py`. `generate_full_review` runs `generate_section`,
`_add_citations` and `_consolidate_feedback`, with no safety step between generation and
return, and the templates in `rag/generator/prompt_templates.py` do not mention tone.

## Limits

This is not what I said I would do in my claim comment. I planned to run the generator
on inputs picked to pull different tones; I could not, for the reason above, so I
checked the path instead.

The three strings above are mine, written to stand in for generator output. I have not
shown that the LLM produces discouraging feedback, only that nothing would catch it if
it did. Confirming the tone question needs a run against a real provider.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

16/20, then 8/8 on a partial re-run, then 19/20.

**Package analysis**

pkg-03, the ripgrep one. My rubric rejected it, gold says accept. It is the only
package I still get wrong.

Four of my five checks pass it. Honesty fails it, and the reason my run recorded was:

> Claims dropping -r '$1' gives '1, 4, 7, 10 correctly' with no pasted output for that
> run — an unshown root-cause-supporting claim that outruns the one artifact actually
> pasted.

The report pastes the failing run and only describes the control run. Gold accepts it
because that control is what ties the bug to `--replace`, and it matches what the owner
said in the thread.

My honesty check is supposed to exempt exactly this, since a control narrows a claim
instead of stretching it. The grader read it as stretching because it supports a root
cause. Both readings fit what I wrote, which is the problem. The same package passed on
my `--only` re-run and failed on the full run right after, same rubric and same model,
so the check is still arguable enough to flip on its own.

**Check rationale**

The honesty check from `tools/repro-check/rubric.md`, both columns as they read now.

Evidence:

> A reading task rather than a lookup. Take every factual claim in `## Candidate claim
> comment` and `## Candidate repro report` and ask which artifact in the package backs
> it. Claims about extra runs, extra versions, extra platforms, or root causes are the
> ones that outrun their evidence most often.

Pass condition:

> Each claim is either backed by something in the package or scoped so it does not
> outrun what was shown. The test is whether an unshown run makes the conclusion
> *stronger* than the pasted artifact supports. A claim that adds reach — a pass on
> another version or platform, a root cause the output does not demonstrate — needs its
> own pasted artifact. A run that narrows or qualifies the claim does not: a control run
> whose result is stated inline, or a repetition count standing behind a negative result
> ("ran it 5 times, never saw it"), is honest reporting rather than overclaiming. A
> report that says it could not reproduce, and shows the run that failed to reproduce,
> passes. A report that asserts a match the pasted output does not show fails, however
> careful it sounds.

The first version failed any run you did not paste, and named "ran it ten times" as the
example. That cost me pkg-09, an honest cannot-reproduce where the writer tried five
times and never saw the bug, so my rule punished the sentence that made it honest. I
thought about dropping the rule and letting behavior-shown carry it, but pkg-02 shows
why not: one pasted artifact propping up a claim that was never tested is the case this
check exists for. So I changed what it measures, from whether a run was pasted to
whether leaving it unpasted makes the conclusion stronger than the evidence supports.

**Trade-offs**

Loosening three checks at once can flip packages that already agreed, so I re-ran my
four targets with four canaries, one per category the changes could touch: pkg-20 for
the disclosure floor, pkg-02 because its report says "I ran this ten times", pkg-06 for
steps and pkg-13 for no-evidence. All four held at reject, all four targets flipped, and
the full run confirmed it at 19/20.

What I give up is pkg-03. I could exempt control runs by name, but that swaps one
arguable word for another and would let through anything a report calls a control. I
would rather miss a case I can explain.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
