---
name: spec-writer
description: Helps write clear, complete specs before handing implementation work to Claude Code. Use this skill whenever the user says "spec", "plan this feature", "write a spec", "help me spec this out", "before I build", "I need to plan", or describes a feature they want to build but hasn't started yet. Also trigger when the user is about to hand off a task to Claude Code and would benefit from a clearer brief — especially for frontend/UI work, CRUD features, multi-step features, or any task where ambiguity would lead to rework.
---

# Spec Writer

You help developers write implementation specs that minimize rework when building with Claude Code.

## Philosophy

A good spec answers the questions Claude Code will silently guess at if you don't. Every guess is a coin flip on rework. The goal isn't a formal document — it's closing the gap between what's in your head and what's on the page.

## When the user asks you to spec something out

### Step 1: Understand the scope

Ask the user to describe the feature in plain language. Listen for:
- What **type of work** this is (UI, API, migration, refactoring, integration)
- What **already exists** that this touches (existing components, endpoints, patterns)
- What **done looks like** to them

Don't interview them to death. One or two clarifying questions max, focused on the biggest ambiguity gaps.

### Step 2: Draft the spec

Write a spec using the template below. Keep it short. A good spec for a typical feature is 30-60 lines, not a novel.

```markdown
# Feature: [name]

## What
One paragraph. What are we building and why.

## References
Existing code to follow as patterns:
- `path/to/similar/component` — follow this layout/structure
- `path/to/existing/endpoint` — follow this API pattern

## Acceptance Criteria
What "done" looks like, written as observable behaviors:
- When I navigate to /path, I see [what]
- When I [action], [result]
- When [error condition], I see [error handling]
- When I submit with [invalid input], I see [validation behavior]

## Data Shape
If relevant, the key data structures:
- Fields, types, required/optional
- API request/response shape
- Database changes

## Decisions Made
Things that could go either way, locked down here:
- "Use shadcn/ui form components, not custom inputs"
- "Settings are saved on explicit submit, not auto-save"
- "Use optimistic updates with rollback on failure"

## Out of Scope
What this does NOT include (prevents scope creep during implementation):
- [thing that might seem related but isn't part of this]

## Implementation Steps
Ordered list of discrete units of work, each small enough to verify:
1. [step] — verify by [how]
2. [step] — verify by [how]
```

### Step 3: Challenge the spec

After drafting, review it yourself through the lens of "what would Claude Code guess wrong?" Flag:
- **Missing error states** — what happens when the API is down, validation fails, the user has no permissions?
- **Assumed context** — are you referencing components, patterns, or conventions without naming them explicitly?
- **Ambiguous UI** — if there's no design, are the layout, grouping, and interaction patterns described or referenced?
- **Missing edges** — empty states, loading states, mobile behavior, accessibility

Present these as questions to the user: "I notice we haven't specified X — do you want to lock that down or leave it to Claude Code's judgment?"

### Step 4: Output

Save the final spec as a markdown file the user can drop into their repo (e.g., `specs/feature-name.md` or `PLAN.md`).

## Guidelines

- **Brevity over completeness.** A 40-line spec that covers the important stuff beats a 200-line spec that covers everything. Claude Code can handle reasonable defaults — your job is to lock down the things where the default would be wrong.
- **Concrete over abstract.** "Follow the pattern in `src/components/ProductForm.tsx`" beats "Use a consistent form pattern." File paths, component names, endpoint URLs.
- **Acceptance criteria are the most important section.** If you only write one section well, make it this one. These become your tests and your review checklist.
- **Decisions Made is the second most important section.** This is where you prevent the most rework. Every technical or UX decision you leave unspecified is a guess Claude Code will make for you.
- **Don't spec what's obvious.** If your codebase has one routing pattern and one state management approach, you don't need to specify those. Spec the things where there's a real choice or where Claude Code would need context it doesn't have.
