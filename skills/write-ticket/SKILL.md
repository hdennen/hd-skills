---
name: write-ticket
description: Write useful tickets with clear context, expected behavior, and testable acceptance criteria. Use when drafting, creating, or improving a ticket.
---

# Write Ticket

Write a ticket that someone can understand and verify without relying on private context. Keep it concise, preserve known facts, and do not invent missing requirements.

## Choose the ticket structure

Use a specific, outcome-oriented title. Use the standard structure for features, improvements, infrastructure work, and other non-bug tickets. Use the bug structure when existing behavior is broken or differs from its contract.

### Standard ticket

```markdown
## Context

[Explain the problem, who it affects, why it matters, and the relevant current behavior. Link supporting discussions, designs, incidents, or code when available.]

## Expected behavior

[Describe the observable result after the work is complete. State what should happen, not an implementation approach, unless the implementation is constrained.]

## Acceptance criteria

- [ ] [A concrete, independently verifiable outcome]
- [ ] [Another observable outcome]
- [ ] [Important edge-case or failure behavior, when relevant]
```

### Bug ticket

```markdown
## Context

[Explain where and when the bug occurs, who it affects, its impact, and any relevant environment, version, logs, screenshots, or links.]

## Reproduction steps

1. [Starting state or prerequisite]
2. [Specific action]
3. [Next action needed to trigger the bug]

## Expected behavior

[Describe what should happen according to the product or system contract.]

## Actual behavior

[Describe what happens instead, including the exact error or observable result.]

## Acceptance criteria

- [ ] [The expected behavior occurs when following the reproduction steps]
- [ ] [Relevant regression or edge-case outcome]
```

Keep reproduction steps minimal, ordered, and independently repeatable. State the frequency when a bug is intermittent. Never fabricate reproduction details; mark unknowns explicitly or ask for them when they are necessary to investigate the bug.

Add an `Out of scope` section only when it prevents a likely misunderstanding.

## Labels

Assign at least one appropriate existing label, using the ticket system's exact label name:

- `security` for vulnerabilities, permissions, secrets, or security controls.
- `infrastructure` for deployment, CI/CD, hosting, networking, or platform work.
- `improvement` for enhancing existing behavior without introducing a distinct new capability.
- `feature` for a new user- or system-facing capability.
- `bug` for behavior that is broken or differs from its contract.

Choose the most specific label that reflects the primary work. Apply multiple labels only when each adds useful routing or ownership information. If drafting without access to the ticket system, state the recommended label; do not invent or create labels without approval.

## Quality bar

- Include enough context to explain the motivation and current state.
- Make expected behavior unambiguous and user- or system-observable.
- For bugs, clearly separate the expected behavior from the observed actual behavior.
- Write acceptance criteria as atomic, testable outcomes rather than implementation tasks.
- Cover important error states, permissions, and edge cases when they are part of the requested behavior.
- Avoid vague criteria such as "works correctly," unnecessary solutioning, and duplicated prose.
- Ask a focused question when a missing detail materially changes the ticket; otherwise draft with explicit assumptions.

When asked to create or update the ticket, use the available ticket-system integration after drafting the content. Preserve the user's requested destination, project, priority, assignee, and label; ask only when a required destination is ambiguous.
