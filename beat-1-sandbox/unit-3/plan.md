# Plan for #27: Add a constructive-feedback tone check

## Reproduction evidence

The [reproduction comment](https://github.com/codepath/pathreview-ai301-fa26-s1/issues/27#issuecomment-5861163895) established a missing guard in the review-generation path, not that the live model currently produces discouraging feedback:

> "What I could check is what happens to a discouraging section once it exists, and nothing in the current path stops one."

The direct safety-layer check passed constructive, vague/dismissive, and discouraging samples through unchanged:

> "Actual: all three come back identical."

The code-path inspection also found:

> "Neither class runs during generation in any case."

The reproduction was intentionally limited because the generator requires a live API key:

> "I have not shown that the LLM produces discouraging feedback, only that nothing would catch it if it did."

## Diagnosis

`ReviewGenerator.generate_section` accepts the first parsed LLM section without evaluating whether its feedback is actionable, specific, and encouraging. `generate_full_review` then adds citations and returns that section. The existing `ContentFilter` only replaces a small set of harmful regex matches and is not called from the generator, while `BiasDetector` covers demographic and educational-background bias rather than general feedback tone. As a result, the real generator path has no post-generation decision that can reject and regenerate a discouraging, vague, or dismissive section.

## Scope

In scope:

- Add a prompt-backed constructive-versus-negative tone classification operation to the existing content-safety component.
- Apply that operation to every section produced by `ReviewGenerator` before the section is accepted.
- Regenerate a rejected section with a finite retry limit, and fail closed rather than return rejected content when the limit is exhausted or the classifier response cannot be interpreted.
- Add isolated unit coverage using a mocked OpenAI-compatible client so the behavior is testable without an API key.

Out of scope:

- Reworking the existing harmful-content regexes or bias-detection patterns.
- Changing the five review-section prompts to prevent negative tone before generation; this issue asks for a post-generation classifier and retry gate.
- Replacing the repository's LLM-provider configuration or adding a mock provider to the application.
- Wiring the separate placeholder pipeline in `core/services/review_service.py` into the real RAG generator.
- Proving how often a live provider emits discouraging feedback or measuring classifier accuracy across models; those require provider-backed evaluation data that the reproduction did not have.

## Files to change

- `safety/content_filter.py`: add the prompt and result handling for classifying one generated feedback section as constructive or negative while preserving the existing harmful-content filter.
- `rag/generator/review_generator.py`: run the tone check after each section is generated, regenerate rejected sections up to a bounded attempt limit, and prevent rejected or unclassifiable content from being returned after exhaustion.
- `tests/unit/test_review_generator.py`: add mocked-client tests for acceptance, regeneration, exhaustion, and malformed classifier output.

## Approach

1. Add a focused tone-classification method to `ContentFilter` that receives the existing OpenAI-compatible client and model, sends the generated section in a classification prompt using the issue's criteria, and accepts only a normalized `constructive` result. Treat `negative` and unexpected output as non-passing so ambiguous classifier responses cannot silently release a section.
2. Update `ReviewGenerator.generate_section` so one attempt consists of generating and parsing a section, then classifying its content before returning it. Use the generator's existing client and model rather than creating a second client or configuration path.
3. Bound the loop to three total generation attempts. If a section is classified as negative or unclassifiable, log the failed tone check and regenerate it. If no attempt passes, raise an error so the existing per-section error handling in `generate_full_review` is used instead of returning the rejected feedback.
4. Keep citation handling after acceptance so source labels do not affect the tone decision.
5. Add unit tests that drive the actual generator control flow with ordered mock completion responses and assert which generated section is returned, how many retries occur, and that rejected content never escapes after exhaustion.

## Test plan

The Unit 2 reproduction could not call the live generator, so its three sample tones will be turned into checks that run through the real post-change code with the network boundary mocked.

1. Run `pytest tests/unit/test_review_generator.py -v` and verify:
   - A constructive classification accepts the first generated section without regeneration.
   - A negative classification rejects the first section, regenerates it, and returns only the later constructively classified section.
   - Repeated negative classifications stop at three attempts and do not return any rejected section.
   - An unexpected or malformed classification is not treated as constructive and follows the same bounded failure path.
   - The classification prompt sent by the code contains the candidate section and the actionable, specific, encouraging versus discouraging, vague, dismissive criteria.
2. Re-run the Unit 2 code-path check:

   ```bash
   grep -rn "ContentFilter\|BiasDetector" --include="*.py" rag/ api/ agent/ safety/__init__.py
   ```

   Expected after the change: `rag/generator/review_generator.py` imports and invokes `ContentFilter` for generated sections; `BiasDetector` remains outside this issue's scope.
3. Run `make test-unit` to check the full backend unit suite.
4. Run `make lint` and `make typecheck` to check the repository's required Python quality gates.
5. If a provider key becomes available, run the generator against the same portfolio inputs with constructive and intentionally challenging cases and record the classifications and retry behavior. This is an additional semantic check, not a prerequisite for the mocked control-flow tests.

Expected post-fix behavior: a section classified as constructive is returned normally; a section classified as discouraging, vague, or dismissive is not returned and triggers regeneration; repeated failures stop at the retry limit without leaking the rejected feedback.

## Risks and unknowns

- Each section now needs at least one additional model call, increasing latency and cost; rejected sections add both another generation call and another classification call.
- The same configured model will generate and classify unless the repository later adds separate model configuration. Its false-positive and false-negative rate is unknown without a provider-backed evaluation set.
- Strict parsing prevents ambiguous classifier output from passing, but malformed responses can exhaust retries and replace a section with the generator's existing error fallback.
- The reproduction did not observe live generated tone, so this plan addresses the verified absence of a guard and does not claim a measured rate of discouraging output.

## Deviations

The implementation followed the posted plan. Choosing `RuntimeError` for the bounded-retry failure clarified an exception type that the plan had left unspecified, but did not change the planned scope, approach, or test strategy. No deviations were necessary.
