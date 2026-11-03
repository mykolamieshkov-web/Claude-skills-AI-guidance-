---
name: "myk-react-build"
description: Token-minimal React/SCSS coding mode for Myk's custom Vite + React + SCSS setup. Use whenever the user asks to build, fix, or extend a React component, section, block, or page — including when a Figma screenshot or layout image is attached, or when building UI from scratch with no image provided. Always apply this skill for any React or front-end coding task, even if the user does not name the skill explicitly.
---

# Token-Minimal React Build Mode

## Core Rule
Output only what the user cannot produce themselves. Code, diffs, commands. No preamble, no fluff, no re-stating the task.

## Layout Input — Image or No Image

### When a Figma / layout image is attached
- Read the image as a wireframe. Extract: section type, column count, element hierarchy, spacing rhythm, alignment axis.
- Ignore placeholder content (lorem text, grey boxes). Build real semantic structure.
- Map layout regions to BEM blocks and utility classes from the rules below.
- Do not ask for clarification on layout intent — infer and build.

### When no image is attached
- Apply standard UI layout principles:
  - Hero: full-width, centered column, heading + subtext + CTA.
  - Features / cards: 3-column grid on desktop, stacks on mobile.
  - CTA section: centered, constrained width, heading + button pair.
  - Testimonials: horizontal scroll or 2-column grid.
  - Nav: flex row, logo left, links right, burger on mobile.
  - Footer: 3–4 column grid, collapses to single column.
- Pick the layout that fits the section name the user gave. Build it directly.

## Front-End & Build Architecture Rules

- SCSS import for side-effects only: `import './style.scss';`
- Do NOT import styles as objects or call them as functions.
- Use global BEM utility classes from `_utilities.scss`: `__container`, `__flex-row`, `__flex-column`. Apply via `className` string.
- Component syntax — functional declaration, default export at bottom:

```jsx
function ComponentName() {
  return (
    <div className="__container __flex-row">
      {/* ... */}
    </div>
  );
}

export default ComponentName;
```

- No inline styles unless the value is dynamic (JS variable).
- One component per file. File name matches component name in kebab-case.
- Props typed via destructuring, no PropTypes unless asked.

## Response Format by Task

| Task | Output |
| --- | --- |
| Build section from image | Full component code only. |
| Build section from scratch | Full component code only. |
| Fix a bug | Diff or corrected lines. One sentence max if non-obvious. |
| Explain code | 2–3 sentences max. |
| Architecture question | Decision + one-line rationale. |
| CLI command | Command only. |
| Multiple options | Pick one. Under 10 words why. |

## What to NEVER Output
- Greetings, sign-offs, acknowledgments.
- Re-stating the prompt or task.
- Confidence statements.
- Reasoning blocks unless explicitly requested.
- Explaining what you are about to do before doing it.
- Inline comments on obvious lines.

## Code Output Rules
- Return only the changed portion unless full file context is required.
- Use `// ...` markers for targeted snippet context.
- Comments only for non-obvious decisions or gotchas.
- If the fix is one line, return one line.

## Code Review Limits (Max 3 Items)
1. One correctness issue (if any)
2. One performance or maintainability issue (if any)
3. One suggestion (if any)

No summary. No overall evaluation statement.

## Decision Strategy
Pick the approach that ships fastest, has fewest dependencies, is easiest to delete later. State the pick in one line.

## Token Pressure Cap
- Responses capped at 150 words max.
- Code only; zero surrounding prose.
- Make small operational calls without asking for confirmation.

## Example — Section from image
User attaches a Figma screenshot showing two columns: left text block, right image.

```jsx
function FeatureBlock() {
  return (
    <section className="feature-block __container __flex-row">
      <div className="feature-block__text">
        <h2 className="feature-block__heading">Heading</h2>
        <p className="feature-block__body">Body copy goes here.</p>
        <a className="feature-block__cta btn" href="#">Get started</a>
      </div>
      <div className="feature-block__media">
        <img src="/assets/placeholder.jpg" alt="Feature visual" />
      </div>
    </section>
  );
}

export default FeatureBlock;
```

## Example — Section from scratch
User: "build a hero section"

```jsx
function Hero() {
  return (
    <section className="hero __container __flex-column">
      <h1 className="hero__heading">Main value proposition</h1>
      <p className="hero__sub">Supporting line that clarifies the offer.</p>
      <a className="hero__cta btn" href="#">Get started</a>
    </section>
  );
}

export default Hero;
```