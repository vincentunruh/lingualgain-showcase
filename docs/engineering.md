# Engineering Case Studies

LingualGain combines dialogue, audio, contextual explanations and practice. The difficult work is often between those features: what happens when the learner changes direction while something else is still running?

I work across the browser experience, API, persistence and supporting content tools. These three examples describe implemented boundaries and focused checks, without publishing the private application source.

## 1. Navigation And Asynchronous Interactions

**Problem.** A learner starts recording, changes screens and only then grants microphone permission. Or a submitted response returns after the learner has switched back to typing. A late result must not take ownership of the new interaction.

**Approach.** Navigation, sentence rendering and recording have explicit lifecycle owners. Controls are scoped to the current view. Cancellation cleans up pending work and recording resources; subsequent callbacks must still belong to the active interaction before changing its UI. The dialogue feed reuses a small set of page containers, so preparation and promotion also need to preserve which page is current.

**Trade-off.** Reusing views avoids rebuilding the whole screen on every move, but stale state becomes a real risk. Explicit cleanup and transition tests are necessary; testing individual buttons is not enough.

**Verified result.** Controlled interaction tests passed for late permission after cancellation, upload cancellation, rapid recording clicks, resource release and ignored late results. All 23 pager checks passed, including scoped page access, cancelled gestures and a renderable current page after navigation. These are simulated interaction checks, not a live speech-provider or device-performance benchmark.

## 2. Contextual Help Without Context Loss

**Problem.** Looking up a word in isolation can explain the wrong thing. In a multiword expression, several tappable words may need to lead to the same contextual meaning. An unrelated example can make the explanation less useful even if the example is grammatically valid.

**Approach.** A pure selection layer chooses applicable contextual material before the sheet renders it. Expression selection respects sentence context, explanation language and review state. The sheet retains ownership of display, focus, audio and dismissal rather than mixing those concerns into selection.

**Trade-off.** Strict contextual selection can leave fewer examples to show. I prefer an omitted unsupported example to filling the space with something irrelevant. The system still depends on correctly prepared linguistic material; filtering cannot establish that material's quality.

**Verified result.** Focused fixtures passed for consistent selection across words in an expression, rejection of unrelated or unreviewed entries, removal of duplicate alternatives and preservation of input data. Example checks exclude the current sentence and unrelated constructions. This establishes selection behaviour, not linguistic approval of every explanation.

## 3. Dependent Content And Readiness

**Problem.** Updating dialogue does not automatically update its explanation, grammar table or activity feedback. A plausible-looking dependent artifact can describe an older version of the source.

**Approach.** Content and structured support have versioned identities. Validation checks their relationship before they are eligible for use, and review evidence is distinct from automated acceptance. Structured information participates in change detection alongside prose, rather than treating the prose as the whole artifact.

**Trade-off.** A small source edit can require more preparation and renewed review. That is additional authoring work, but it makes stale dependencies explicit instead of silently carrying earlier approval forward.

**Verified result.** Controlled contract tests passed for changed structured payloads, wrong-language content, unsupported structure and stale or absent review evidence. Historical compatibility is tested separately. These checks establish contract behaviour; they do not prove natural dialogue or a complete curriculum.

## Verification Snapshot

Run on **31 October 2026**, against the private application, without provider calls or changes to application source.

| Scope | Method | Result |
| --- | --- | --- |
| Contextual help selection | Pure fixtures, including expression and example selection | Passed |
| Sentence and exercise speech interactions | Simulated browser DOM, microphone and responses | Passed |
| Structured learning-support contracts | Deterministic source, locale and review fixtures | Passed |
| Combined result for the three suites above | Vitest | **67 tests passed** |
| Dialogue pager | DOM-based integration harness | **23 checks passed** |

This is a maintainer-reported snapshot of focused tests, not an independently reproducible public test suite. Private tests, fixtures and contracts are not included in this repository. It is not a full regression run, production certification or evidence of learner retention.

Playwright is also used in the application for browser regression testing; no new end-to-end browser journey or live provider assessment is claimed by this snapshot.

## What Still Needs Evidence

- A publishable gallery of real application screens with confirmed asset rights and no private information.
- Qualified review of learning content and actual speech-provider feedback.
- Observed learner journeys and delayed, unaided retrieval, rather than inferring learning from completion or passing software tests.

The wordmark in this repository is an original text-only brand asset. It is not an application capture. No scene artwork, fonts, private documents or implementation files are redistributed.

[Back to LingualGain](../README.md)
