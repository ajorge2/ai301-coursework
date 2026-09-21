# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/27

**Verdict output**

[Your skill's live-mode output for this issue, pasted verbatim and ending with the
fenced JSON verdict block. A summary does not satisfy this field.]

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

```
All three graded live against the rubric. Repo-level checks (shared by all three): last default-branch commit 2026-09-16 by human Aburke225 (4 days ago), repo not archived, pushed 4 days ago, and docs/CONTRIBUTING.md states no AI policy at all.

Accepted, in fit order:

1. #27 — feedback tone check (tier-2, safety/, rag/generator/). Best fit: a production-AI reliability guardrail — LLM-prompted classification with reject-and-regenerate — in Python, and it's squarely about whether the product's output is actually good for its users, not just syntactically correct.
2. #55 — skill extractor misses JS/TS (tier-1, Python parser). Bounded, named file, five named failing tests to flip from xfail; the cleanest ramp-in but less of the AI-systems depth you're after.
3. #46 — ingestion benchmark (tier-3, pytest-benchmark). Reliability-flavored and backend, but CI/tooling work with a concrete 30s threshold rather than product behavior.

No rejections. Note one tension worth flagging: the rubric's specification-ready check passes #27 because nothing is explicitly marked TBD, but "constructive vs. negative" is left for you to operationalize in the prompt — that's implementation uncertainty, which the rubric deliberately does not fail on.

[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/27",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Default-branch commit 2026-09-16T21:42:18Z by human Aburke225 — 4 days before today."},
      {"name": "repo-in-use", "grade": "pass", "evidence": "isArchived=false; pushedAt 2026-09-16T21:48:27Z (within 365 days); no release, acceptable."},
      {"name": "one-cohesive-change", "grade": "pass", "evidence": "One feature — a post-generation tone classification step across safety/content_filter.py and rag/generator/review_generator.py; no umbrella/tracking language."},
      {"name": "specification-ready", "grade": "pass", "evidence": "Body defines the outcome ('constructive (actionable, specific, encouraging) or negative'; 'Reject and regenerate sections that fail'); no TBD, zero comments so no maintainer disagreement."},
      {"name": "attempt-history", "grade": "pass", "evidence": "Timeline shows only three 'labeled' events; repo-wide `gh pr list --state all` returns []."},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; no linked PRs; zero comments."},
      {"name": "ai-allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md contains no AI/LLM/generated-code policy; no AI_POLICY.md; silence passes."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/55",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Default-branch commit 2026-09-16T21:42:18Z by human Aburke225 — 4 days before today."},
      {"name": "repo-in-use", "grade": "pass", "evidence": "isArchived=false; pushedAt 2026-09-16T21:48:27Z (within 365 days)."},
      {"name": "one-cohesive-change", "grade": "pass", "evidence": "One defect in _detect_languages/_detect_tools/_detect_databases in a single file, ingestion/parsers/skill_extractor.py."},
      {"name": "specification-ready", "grade": "pass", "evidence": "Repro given: 'pytest tests/unit/test_skill_extractor.py -q' with five named xfailed tests defining expected behavior."},
      {"name": "attempt-history", "grade": "pass", "evidence": "Timeline shows only three 'labeled' events; no closed-unmerged linked PRs (repo has zero PRs)."},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; no linked PRs; zero comments (updatedAt 2026-09-16 reflects a manifest-sync commit, not a claim)."},
      {"name": "ai-allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no AI policy; no outright ban anywhere in repo root or .github/."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/46",
    "checks": [
      {"name": "maintainer-active", "grade": "pass", "evidence": "Default-branch commit 2026-09-16T21:42:18Z by human Aburke225 — 4 days before today."},
      {"name": "repo-in-use", "grade": "pass", "evidence": "isArchived=false; pushedAt 2026-09-16T21:48:27Z (within 365 days)."},
      {"name": "one-cohesive-change", "grade": "pass", "evidence": "Single new benchmark test in tests/benchmarks/test_ingestion_performance.py; not an umbrella issue."},
      {"name": "specification-ready", "grade": "pass", "evidence": "Concrete threshold and tool named: '5 repos + resume ... within 30 seconds' using pytest-benchmark; no TBD."},
      {"name": "attempt-history", "grade": "pass", "evidence": "Timeline shows only three 'labeled' events; zero linked PRs."},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; no linked PRs; zero comments; updatedAt equals createdAt (2026-09-10)."},
      {"name": "ai-allowed", "grade": "pass", "evidence": "No AI ban in docs/CONTRIBUTING.md, README, or PR template (template asks only for CI/tests)."}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

[The agreement score of each run you did, in order. A single run is a complete answer if
only one run occurred. **The last score in your list must match the agreement line in the
`eval-run.txt` you committed** — that file is the record of your final run.]

agreement: 14/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in scope)
agreement: 13/20 scored items  (bar: 18/20: below the bar)
agreement: 20/20 scored items  (bar: 18/20: PASS)

**Issue analysis**

[One scored issue, identified by id (`issue-01` through `issue-20`; the `calib-`
issues are not scored). State your rubric's decision, the gold label, and the
reasoning that produced your rubric's result.]

issue-12  reject  reject   yes
Repo's ban on ai-generated output fails the required ai-allowed check

**Check rationale**

[One check from the `rubric.md` uploaded to `tools/issue-select/`, quoted as it is
currently written, with the reasoning behind its current form.]

| maintainer-active | Eval item `captured` date; Repo facts `last 5 default-branch commits`; Repo facts `maintainer first-response sample`; issue comment thread author associations | Pass when at least one of the last 5 default-branch commits is within 90 days of capture and is either human-authored or a bot merge of a human PR, OR an Owner/Member/Collaborator responded to a non-maintainer issue within 90 days before capture. Automated dependency/version/leaderboard updates alone do not pass. | required |

Checks if maintainers are considering edits to main branch, which includes issue comments since those indicate consideration.

**Trade-offs**

[What the quoted check gives up. Any one of these is a complete answer: an issue whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

Passing solely on default-branch commits gives up the nuance of whether non-maintainer issues are fully acknowledged. My attempt-history check takes care of this later though.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three:

1. The issue's fit to your interests and to the time available.

Very high. I enjoy solving ambiguous natural-language problems and implementing agentic workflows. Unsure about the time constraint but seems solvable in less than a week.

2. What the verdict identified correctly, and what you weighed that the rubric could
   not.

The verdict identified that the user impact was something important to me, which is true. The fit to my interests was something I was able to weigh in when selecting the three candidates in the first place.

3. The anticipated difficulty in claiming it.

Unsure, but I don't see a reason why it would be hard to claim if I explain my plan well.

]

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
