---
name: ai-writing-slop-detection-correction
description: Detect and correct AI-style writing patterns in landing-page copy and cold emails while preserving factual accuracy, intended meaning, audience, and conversion goals.
---

# SKILL: AI Writing Slop Detection & Correction

## PURPOSE

Detect and correct AI-style writing problems in two contexts:

1. Landing-page copy visible in attached screenshots, especially vibe-coded websites.
2. Cold-email copy.

The goal is not to prove whether text was written by AI. The goal is to remove low-quality AI-style patterns and improve clarity, specificity, naturalness, and conversion effectiveness.

## OPERATING PRINCIPLES

- Be conservative. Change only what needs changing.
- Never invent facts, proof, statistics, testimonials, credentials, guarantees, prices, results, or product capabilities.
- Preserve the author's intended meaning, offer, audience, and factual constraints.
- Prefer concrete language over abstract marketing language.
- Prefer simple words when they communicate the same idea.
- Do not explain every correction.
- Do not produce multiple rewritten variants unless explicitly requested.
- Before returning corrected copy, ask only for the requested style choice if style has not already been specified:

Style:
1. Friendly - like talking to a friend over coffee.
2. Professional - default for client-facing cold email and conversion-focused website copy.

If the user already specified a style, do not ask again.

## AI-SLOP HEURISTICS

Flag a phrase or passage when one or more of these are present:

- Generic praise that could describe almost any company.
- Abstract claims without concrete meaning.
- Inflated adjectives or superlatives without proof.
- Repetitive sentence patterns.
- Formulaic transitions.
- Excessive corporate jargon.
- Predictable “AI vocabulary,” especially when repeated: crucial, delve, enhance, foster, robust, showcase, pivotal, landscape, tapestry, underscore, meticulous, vibrant, valuable.
- Repeated “not only X, but also Y” structures.
- Repeated three-item rhetorical lists.
- Empty phrases such as “in today’s fast-paced world,” “unlock your potential,” “take your business to the next level,” or similar generic positioning.
- Claims of transformation without explaining what changes for the customer.
- Words that sound impressive but add no information.
- Restating the same benefit in different language.
- Overly symmetrical or polished prose that lacks a distinctive human point of view.

IMPORTANT:
A listed word is NOT automatically bad. Do not mechanically replace words just because they appear on a list.

## LANDING-PAGE MODE

Input:
- One or more screenshots.
- Optional business/product context from the user.

First inspect the screenshot for visible copy and its role in the page:
- announcement
- navigation
- hero
- value proposition
- feature/benefit section
- proof/social proof
- process
- FAQ
- CTA
- footer
- other visible copy

Correct only copy that is actually visible. If text is unreadable, do not invent it.

Evaluate:
- clarity
- specificity
- relevance to the intended customer
- benefit communication
- credibility
- scannability
- unnecessary repetition
- AI-style phrasing
- CTA clarity
- mismatch between headline and supporting copy

Correction rule:
Keep the original information architecture unless the structure itself makes the message unclear.

OUTPUT FOR WEBSITE

Return a compact table:

| Section | Original | Corrected copy | Why |
|---|---|---|---|
| Hero | ... | ... | ... |

However, when the user wants minimal-token output, omit the “Why” column and return:

| Section | Corrected copy |
|---|---|
| Hero | ... |

Do not rewrite unseen sections.

After correction, perform a silent second pass:
- Remove remaining generic AI phrasing.
- Remove repetition.
- Verify every factual claim against the supplied context.
- Check that the headline communicates a concrete benefit.
- Check that CTA language is direct.
- Check that the copy sounds like one coherent brand rather than a collection of generated blocks.

## COLD-EMAIL MODE

Input:
- Email draft.
- Optional prospect/company context.

Treat the email as separate parts:
1. Subject
2. Opening / relevance
3. Problem or observation
4. Offer
5. Proof, if available
6. CTA
7. Sign-off

Evaluate:
- relevance
- specificity
- naturalness
- brevity
- credibility
- personalization
- one clear offer
- one clear CTA

Default structure:
Subject
Opening
Problem / observation
Offer
Proof
CTA
Sign-off

Do not force every part into every email. If proof is unavailable, do not invent it.

A first-touch cold email should normally have one core idea and one next step. Avoid a menu of CTAs.

After rewriting, silently run a second pass:
- Remove generic opener/filler.
- Remove unnecessary self-description.
- Remove generic agency language.
- Remove repeated benefits.
- Replace vague claims with concrete observations where evidence exists.
- Ensure the CTA is small enough for a cold relationship.
- Ensure personalization is real and relevant, not token name insertion.
- Read the final email once as a skeptical recipient.

## CORRECTION LOOP

Use this internal sequence:

1. READ
2. IDENTIFY HIGH-CONFIDENCE PROBLEMS
3. REWRITE
4. RUN SLOP AUDIT
5. RUN CONTEXT/FACT AUDIT
6. RUN CONVERSION/READABILITY AUDIT
7. REWRITE AGAIN IF NEEDED
8. OUTPUT FINAL COPY ONLY

Do not narrate this process unless the user asks.

## QUALITY TEST

Before final output, ask internally:

- Does every sentence earn its place?
- Is the language specific to this business, offer, audience, or situation?
- Could this sentence be pasted onto 50 unrelated websites/emails? If yes, make it more specific or remove it.
- Is any claim unsupported?
- Is the copy clearer after editing, not merely different?
- Does it sound like a competent human wrote it for this exact context?
- Does the CTA tell the reader what to do next?
- Is there unnecessary polish, jargon, or rhetorical symmetry?

## FAILURE MODES TO AVOID

Do NOT:
- claim certainty that text was AI-generated
- use AI-detection scores as proof
- rewrite everything just to make it “less AI”
- add fake imperfections
- add slang merely to sound human
- add emojis unless the requested brand style calls for them
- fabricate personalization
- invent proof
- create multiple alternatives by default
- give a long explanation of edits
- change facts or business positioning without permission

## STYLE QUESTION

If no style has been supplied, ask exactly:

“Which style?
1. Friendly - like talking to a friend over coffee.
2. Professional - default for client-facing copy.”

Then wait for the answer before producing the corrected version.

## SOURCE NOTE

The skill is based on:
- Wikipedia: Signs of AI writing
- Shaib et al. (2025), Measuring AI “Slop” in Text
- Saha & Feizi (2025), Almost AI, Almost Human
- OpenAI research on limitations of AI-text classification
- Nielsen Norman Group usability and microcopy guidance
- HubSpot cold-email guidance

The skill should treat these sources as heuristics and quality principles, not as absolute rules.
