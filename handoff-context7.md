---
name: handoff-context7
description: Session handoff (artifact + compaction) + third-party library documentation lookup via Context7 v2 API. Use for transferring agent context between sessions, carrying decisions/work state forward, and fetching version-specific library docs without guessing.
---

# Handoff + Context7 Merged Skill

## Part 1: Handoff (Context Transfer Between Sessions)

### What It Does
Transfers agent context from one session to another with no return path. Carry mechanism: written artifact (file) or in-memory compaction (summary). New session starts with zero context—nothing carries unless explicit.

### When to Handoff
- Session context window heavy, need fresh start
- Switching roles (planner → implementer, designer → coder)
- Parallel work (fan out to multiple sessions)
- AFK run (automated task needs initial state)

### Handoff Artifact (Recommended)
Write decisions + state to file before next session reads it.

**What to carry:**
- Decisions made (why, not just what)
- Work completed + next steps
- Code/design state (files, branches, links)
- Constraints discovered (budget, scope, tech limits)
- Unresolved blockers

**Format example:**
```
# Project: MVP Landing Page Redesign

## Completed
- Sketched 3 layout options (artifact: /designs/layouts.fig)
- A/B tested copy variants (winner: benefit-first framing)
- Mapped SEO keywords (170 terms, /research/keywords.csv)

## Decisions
- Color: green (#22c55e) + blue (#3b82f6) confirmed with client
- CMS: Webflow (not headless, reduces setup time)
- Deadline: Aug 15 (hard stop, 6 weeks)

## Next Session (Implementation)
- Start Webflow project, import colors + fonts
- Build hero section from layout option C
- Implement tracking for form submissions

## Blockers
- Client hasn't approved final copy (waiting on reply, due Jul 10)
- Webflow custom code limits (CSS transforms OK, JS limited)

## Resources
- Design file: /designs/layouts.fig
- Copy doc: /docs/copy-final.md
- Keyword list: /research/keywords.csv
```

**Reuse across sessions:**
- Same handoff artifact → multiple parallel sessions
- Sessions can't ask old session questions
- Judge handoff by: "Could zero-context session act on this alone?"

### Compaction (Automatic, In-Window)
Auto-summarize heavy context into brief summary mid-session. Cheap, harder to inspect, feeds one successor.

---

## Part 2: Context7 (Third-Party Library Docs Lookup)

### What It Does
Fetches version-specific library documentation via Context7 v2 REST API. No guessing, no stale training data—actual current docs.

### When to Use Context7
**Always** invoke before writing code for external libraries when:
- Implementing unfamiliar library APIs
- Debugging errors (stack traces, "method not found", version mismatches)
- Checking version-specific syntax or breaking changes
- Verifying deprecated/removed APIs
- Encountering unfamiliar `import`/`require` statements

### Protocol

#### Step 1: Search for Library ID
```bash
scripts/context7.sh search "<library>" "<intent>"
```

Examples:
```bash
scripts/context7.sh search "tanstack-query" "optimistic updates"
scripts/context7.sh search "react-hook-form" "form validation with zod"
scripts/context7.sh search "nextjs" "middleware redirect authentication"
```

Output: Library IDs like `/tanstack/query`, `/facebook/react`, `/vercel/next.js`

If multiple results: ask user which library they meant.

#### Step 2: Fetch Documentation
```bash
scripts/context7.sh docs "<library-id>" "<specific-question>"
```

Examples:
```bash
scripts/context7.sh docs "/vercel/next.js" "middleware redirect authentication"
scripts/context7.sh docs "/tanstack/query" "useMutation optimistic update pattern v5"
scripts/context7.sh docs "/react-hook-form/react-hook-form" "field array remove method"
```

#### Step 3: Apply Context
Use returned docs to write accurate, version-correct code. Don't invent syntax.

### Critical Rules
1. **Specific queries win** - "useState hook array destructuring" beats "react hooks"
2. **Include version** - Append version to query if user mentions one: "next.js v14 image optimization"
3. **No guessing** - If search returns nothing or errors, ask user before proceeding
4. **Only run context7.sh** - Don't mix context7 with other bash commands
5. **Stack traces first** - If user provides error, search by error message + library name

### Error Handling
- **No results:** User clarifies library name or intent
- **Network error:** Check HTTPS_PROXY, internet connection
- **Rate limit:** Wait, then retry with narrower query
- **Deprecated method in docs:** Docs show replacement; use that instead

---

## Combined Workflow Example

**Scenario:** Frontend task, new session, need React Hook Form validation + handoff from planning session.

### Session 1 (Planning)
- Decide validation rules, error messages
- Create handoff artifact:

```
# Form Validation Design

## Completed
- Sketched form sections (name, email, password)
- Validation rules defined (email RFC 5322, password 12+ chars)
- Error message UX mapped

## Next Session
- Implement with React Hook Form
- Library: react-hook-form v7.x
- Validation lib: zod v3.x (or built-in)

## Decisions
- Real-time validation (onChange)
- Show error message on blur, not submit
- Custom error colors: red (#ef4444)

## Resources
- Validation rules: /docs/validation.json
- Error messages: /docs/errors.en.json
```

### Session 2 (Implementation)
Read handoff artifact. Then:

```bash
scripts/context7.sh search "react-hook-form" "form validation with zod"
scripts/context7.sh docs "/react-hook-form/react-hook-form" "useForm with resolver zod v7"
```

Fetch docs, code from current API (not training data). No guessing.

---

## When NOT to Use

- **Handoff:** Single continuous session, no context switch
- **Context7:** Built-in libraries (Node.js, browser APIs), well-known syntax you're confident in

## Resources

- Handoff patterns: See `reference/handoff-patterns.md`
- Context7 troubleshooting: See `reference/context7-errors.md`
- Library-specific guides: Fetched on-demand via Context7

---

## Quick Ref

| Task | Tool | Command |
| --- | --- | --- |
| Transfer session state | Handoff artifact | Write `/path/to/handoff.md` |
| Summarize heavy context | Compaction | Auto-summary in window |
| Search library docs | Context7 search | `scripts/context7.sh search "<lib>" "<intent>"` |
| Fetch specific API | Context7 docs | `scripts/context7.sh docs "<id>" "<question>"` |
