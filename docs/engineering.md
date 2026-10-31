# Engineering Notes

LingualGain brings together conversations, contextual explanations, audio and practice in a browser interface. These notes describe a few implementation decisions at a high level, without publishing application code or internal contracts.

## Interaction Lifecycles Matter

A learner can navigate while audio is loading, open an explanation mid-sentence or return after leaving the page. Each interaction can be correct on its own and still conflict with the next one.

The interface separates sentence rendering, navigation, audio controls and contextual overlays into explicit owners. Controls operate within the current learning view rather than assuming that the first matching element on the page is the active one. Opening and closing help also has to account for scrolling, keyboard focus and the surrounding conversation.

This is not a claim that coordination is free: lifecycle boundaries need regression tests. Browser checks complement smaller tests because focus, layout and event timing are observable behaviour, not just data transformations.

## Content Is More Than A Dialogue String

A sentence can have a translation, contextual explanation, example and associated activity. Editing the sentence can make those dependent pieces incorrect even when each one still looks plausible in isolation.

The content tooling keeps versioned material and its preparation state distinct from delivery. Validation and review are explicit steps, not an assumption attached to successful generation. The cost is additional preparation work; the benefit is a clear place to detect mismatched or unfinished material before it reaches a learner.

Automated checks can establish structural consistency. They cannot establish natural dialogue, pedagogical quality or learning effectiveness on their own. Those require separately attributed review and observation.

## Verification At Different Scales

The project uses several kinds of checks for different questions:

- Focused JavaScript tests for individual decisions and state handling.
- Backend tests for API and persistence behaviour.
- Browser tests for complete interactions, responsive layout and recovery.
- Build and static checks for frontend loading and generated assets.

The tools include Vitest, Playwright and esbuild. Their presence is not a performance benchmark or a guarantee of correctness. This showcase deliberately makes no uptime, scale, retention or learning-outcome claims.

## Boundaries Of This Showcase

The application is in development, not presented here as a launched service. This repository contains authored descriptions only: no application source, provider instructions, private data or operational configuration.

Screenshots are omitted until a set with confirmed publication rights and a completed privacy review is available. No mock interface is presented as a real app capture.

[Back to LingualGain](../README.md)
