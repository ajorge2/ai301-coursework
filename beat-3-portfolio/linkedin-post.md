I opened a pull request adding a repeatable search benchmark to Moss, an open-source AI search tool. I built it to record how quickly searches return results and make slowdowns easier to detect.

The hardest part was deciding what the available data could honestly tell us. The request included measuring whether search found the right results, but the existing dataset had no trusted answer key. I added support for comparing results with other search systems while keeping agreement separate from a claim of correctness.

Automated review also showed that my tests checked a rule without checking whether the software actually used it. I added a test that catches that missing step.

The full benchmark ran locally across 100,000 documents, and 29 automated tests passed in the project's checks. The pull request is still open and awaiting approval.

I learned to investigate the assumptions behind a request, explain what the evidence supports, and find a useful first step when part of the original measurement is not yet well-defined.

https://github.com/usemoss/moss/pull/571
