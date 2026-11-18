# Building a reproducible CI benchmark for Moss

## Overview

Moss is an open-source semantic search tool whose existing benchmark printed
timings but was not connected to continuous integration (CI). I built a harness
that reuses its fixed workload, saves latency measurements, and supports an
optional slowdown guard and competitor-agreement report. The full
100,000-document benchmark ran locally, and 29 automated tests passed in CI.
My [pull request](https://github.com/usemoss/moss/pull/571) is open and awaiting
approval; live performance measurement in CI and a complete competitor
reference remain unfinished. I used Codex to help inspect and implement the
code, while I directed the measurement choices and reviewed the results.

## What happened

### Establishing what the benchmark could measure

I chose [issue #435](https://github.com/usemoss/moss/issues/435) because it
connected search evaluation with production reliability. It requested a fixed
dataset, per-commit latency and recall measurements, and a regression guard.
Before proposing a fix, I checked whether that infrastructure already existed.
At upstream commit `2e638ec`, my saved reproduction showed:

```text
$ rg -n 'benchmarks|run_all\.py|bench_moss' .github/workflows
stdout: (none)
stderr: (none)
exit: 1
```

The existing benchmark measured speed, but no workflow invoked it. Its
aggregate runner printed results and caught backend failures, so its exit
status alone could not reliably tell CI whether measurement succeeded.

I initially considered using 1,000 documents to make setup easier. After
tracing the existing workload, I kept all 100,000 documents and the same 15
queries. Reducing the corpus would have changed the workload I was supposed
to make repeatable. The corpus also contained generated variants of 20 base
sentences about AI and computer science, which mattered later when deciding
what search quality meant. Eight actual entries in
`benchmarks/bench_100k_docs.json` are:

```text
doc_0: Computer vision systems extract meaningful information from digital images and videos (variant 0, context 3278)
doc_5: Anomaly detection systems identify unusual patterns that deviate from expected behavior (variant 5, context 55302)
doc_6: Neural networks consist of interconnected layers of artificial neurons that process information (variant 6, context 3905)
doc_7: Natural language processing enables computers to understand and generate human language (variant 7, context 28657)
doc_2: Cryptographic protocols ensure secure data transmission across untrusted networks (variant 2, context 18289)
doc_4: Dimensionality reduction techniques compress high-dimensional data while preserving structure (variant 4, context 11395)
doc_9: Knowledge graphs represent structured relationships between real-world entities and concepts (variant 9, context 3478)
doc_18: Database indexing strategies improve query performance for high-throughput applications (variant 18, context 44118)
```

Eight of the 15 fixed queries in `benchmarks/corpus.py` are:

```text
computer vision image processing
anomaly detection patterns
neural network training data
natural language processing
cryptographic data encryption
dimensionality reduction compression
knowledge graph entities
database indexing performance
```

Each query asks Moss to return its five closest matches from the full
100,000-entry corpus. These examples show the search inputs, not labeled
correct answers or a recorded result list.

### Running the full workload and saving evidence

I ran the new entry point against a fresh local index. I
reused the existing timing loop and statistics, keeping indexing and warmup
outside the measured query samples. Fifty measured rounds over 15 queries
produced 750 samples. The saved output included:

```text
  Moss LOCAL SESSION (top_k=5, 100000 docs)
    iterations : 750
    mean       : 16.693 ms
    stdev      : 15.826 ms
    P50        : 13.608 ms
    P95        : 32.357 ms
    P99        : 75.283 ms
...
exit: 0
```

This was one real Mac run, not evidence of a speed improvement or a Linux
performance baseline. The JSON report records the commit, workload identity,
configuration, and installed runtime. Generated indexes, model files, and
credentials stay out of Git.

I added offline tests for pull requests and made live measurements opt-in
for trusted main-branch runs. Pull requests do not receive Moss credentials.
I left automatic regression failure disabled: enforcing a strict cutoff
before measuring normal CI variation could reject acceptable changes. One
Mac run was not enough to choose an acceptance threshold for Ubuntu CI.

I implemented an optional guard that fails a run when its 95th-percentile
latency exceeds a compatible baseline by more than a configured allowance,
while retaining the measurements. It still needs repeated CI runs to
calibrate that baseline and allowance before activation. Even when enabled,
it flags slowdowns after changes reach `main`; it does not block a push or
prevent a PR from merging. Competitor agreement also remains report-only.

### Defining an honest quality signal

The request called for recall without identifying a reference for correct
results. The checked-in workload had fixed queries but no independent
relevance answer key. I investigated that assumption instead of treating the
requested metric as already well-defined.

I added support for comparing Moss with saved Pinecone, Qdrant, and Chroma
rankings on the same text. The report measures Moss's coverage of sentence
templates supported by at least two competitors, counting duplicate variants
once. This is report-only agreement, not proof of relevance. A real local
Chroma/Qdrant smoke test succeeded on 261 documents. The complete three-engine
capture remains unfinished because I had not configured the required Pinecone
API credentials and a compatible cloud index with integrated embedding.
Qdrant and Chroma ran locally; the Pinecone path required that additional
setup.

### Changing the tests after review

After I opened the PR, CodeRabbit identified a gap in the tests for Pinecone
configuration. The review said:

> No test calls `pinecone_text_field`, so removing its call to the validator would not fail any offline test.

The production collector already called the validator correctly. My tests
checked the helper directly, so they would not catch a future removal of
that call. I added a test through the collector function
using a ready index configuration built from real SDK types. It supplies
the unsupported `dotproduct` metric and checks the specific cosine error:

```python
with self.assertRaisesRegex(capture.CaptureError, "cosine metric"):
    capture.pinecone_text_field(description)
```

That excerpt is from the
[committed test](https://github.com/usemoss/moss/blob/bfd8dfb0834e09fa4f4cce294f532f4040966876/benchmarks/tests/test_capture_configuration.py).
Removing only the validator call in an isolated source copy made the new test
fail with `CaptureError not raised`. The revised suite then ran successfully
in [Ubuntu CI](https://github.com/usemoss/moss/actions/runs/44397984201):

```text
test_pinecone_text_field_rejects_non_cosine_metric (test_capture_configuration.CaptureConfigurationTests.test_pinecone_text_field_rejects_non_cosine_metric) ... ok
Ran 29 tests in 0.577s
OK
```

This gave me a concrete result after the revision: the new test detected the
missing-call regression, and the published suite passed in CI. Constructing
SDK configuration objects required no live Pinecone request. This test covered
the existing validation logic; it did not fix a failure observed during a
live Pinecone capture. The missing Pinecone setup was a separate prerequisite.

## Limitations and what's next

The PR has not merged, and the full issue is not complete. Live Ubuntu
performance measurements, a calibrated latency baseline, a complete
competitor reference, and a durable history are still missing. Workflow
artifacts currently retain reports for 90 days. The runner also measures an
installed Moss release, not arbitrary edits to the checked-out SDK source.

My next step would be to agree with maintainers on the quality reference,
capture it, and collect repeated measurements on the intended CI runner before
enabling a slowdown threshold. A persistent store would then make per-commit
comparisons available beyond artifact retention. The main lesson was to
investigate assumptions inside a request and define a useful first stage
whose results support the claims being made.
