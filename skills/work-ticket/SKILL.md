---
name: work-ticket
description: >
  Work a Linear ticket: read the ticket, map how the relevant code works today, then present 2-3 implementation options biased to the simplest viable one and stop for a decision. Triggers on "have a look at this ticket", "get a lay of the land", "what's the best approach", "scope this ticket", or a Linear issue URL / identifier (e.g. ENG-123). Pass `implement` to carry straight through: build the simplest option, bug-review it, then branch, commit, push, and open a PR.
argument-hint: "[implement]"
---

# Work Ticket

Read the ticket, map how the code works today, and hand back the simplest way to satisfy it — options first, code only on request. Reconnaissance-then-recommendation, not "start coding immediately."

The laziest-viable bias is deliberate: prefer the smallest diff that meets the acceptance criteria, reuse what exists, and question scope the ticket doesn't require (YAGNI). If `/ponytail` is available, apply its discipline to the options and any implementation.

## Modes

- **Default (no argument)** — do phases 1-3, then STOP. Present options, recommend one, and wait for the user to choose. Do not modify files.
- **`implement`** — do phases 1-3, then phases 4-6: build the recommended (simplest viable) option, run a bug review over the changes, then branch, commit, push, and open a PR. Still surface the options first; if the recommendation is genuinely ambiguous, ask before coding.

## Phase 1 — Read the ticket

Linear issue URLs look like `https://linear.app/<workspace>/issue/<TEAM>-<n>/...`. The identifier is `<TEAM>-<n>` (e.g. `ENG-123`). Read the issue with Linear MCP if available. Otherwise query GraphQL (the identifier works as `id`):

```
curl -sS -X POST https://api.linear.app/graphql \
  -H "Authorization: $LINEAR_API_KEY" \
  -H "Content-Type: application/json" \
  --data '{"query":"query($id: String!) { issue(id: $id) { identifier title description url state { name } parent { identifier title } children { nodes { identifier title } } comments { nodes { body } } } }","variables":{"id":"<TEAM>-<n>"}}'
```

Pull out: title, description, acceptance criteria, parent/sub-issues, and any comments or attachments with real detail. If MCP/API isn't set up, fetch the URL. If the "ticket" is instead a PR, a transcript, or a plain description, use that as the source.

Then **restate the ask in one or two lines** and note the acceptance criteria, so the target is explicit before touching code. If the ticket is ambiguous, say so and ask rather than guessing.

## Phase 2 — Lay of the land

Understand how the relevant area works **today**, before proposing anything. If the repo documents its own conventions (a CONTRIBUTING, copilot-instructions, or similar), treat that as the source of truth and read it when the change touches unfamiliar ground.

- Find the modules, components, types, and tests involved.
- Trace the data flow end to end for the affected behavior. When it spans more than a couple of pieces, draw it as a Mermaid `sequenceDiagram` — the clearest way to show how they talk today.
- Cite concrete `file:line` anchors so the map is verifiable.
- Note the patterns the change should follow and any constraints (feature flags, shared state, coupling).

Keep it focused on what the ticket touches. Don't boil the ocean.

## Phase 3 — Options, biased to the laziest that works

Present **2-3 concrete options, ordered simplest → most involved**. For each:

- What changes, and which files it touches.
- **What it buys us** — the upside, and which acceptance criteria it satisfies.
- **What it costs us** — effort, blast radius, risk, and what it leaves on the table.

Always frame the tradeoffs in that buys-us / costs-us form — not a neutral "pros and cons" list.

Then **recommend one** — normally the simplest that fully meets the acceptance criteria — and say why in a sentence. Avoid an even survey; the point is a recommendation.

In default mode, **stop here and wait** for the decision.

## Phase 4 — Implement (only in `implement` mode, or once the user picks)

Build the chosen option:

- Surgical, minimal diff. Every changed line should trace to the ticket. Don't refactor adjacent code or "improve" things the ticket didn't ask for.
- Match the surrounding style and the repo's documented patterns; respect any auto-format hook rather than hand-formatting.
- Verify: run (or add) the relevant tests and state the result. Verify UI changes however the project supports.

## Phase 5 — Bug review in a subagent

Only runs when phase 4 ran. Once the implementation is in place, launch a **subagent** with the Agent tool and have it invoke the `review-bugs` skill against the changes (its default target is the current diff) — running it in a subagent keeps the review in its own context.

When it reports back, triage: fix the confirmed, consequential bugs; for anything speculative or out of scope, note it rather than gold-plating.

## Phase 6 — Branch, commits, and PR

Only runs in `implement` mode, after phases 4-5. Never commit on the default branch — if HEAD is the default branch, branch first.

1. **Branch.** Name it `<type>/<ticket-id>-<short-slug>` (type ∈ `feat` / `fix` / `chore` / `docs`), cut from an up-to-date default branch. Put the Linear identifier in the branch name (e.g. `feat/ENG-123-short-slug`) so Linear's GitHub integration can link it.

2. **Commits — always ≥2, logically separated.** Split feature work from test changes into separate commits (feature first, tests second). If there are no test changes, still split by logical concern so there's never a single commit. Why: if the repo squash-merges, a single-commit PR makes the host use that commit's subject on the default branch instead of the PR title — keeping ≥2 commits lets the **PR title** own the timeline. Write commit messages as Conventional Commits (`feat(scope): …`, `test(scope): …`).

3. **Push** the branch to the remote.

4. **Open the PR** (`gh pr create`):
   - **Title** in Conventional Commits form (`type(scope): subject`) — under squash-merge this becomes the commit on the default branch, so make it the authoritative one-liner. Include the Linear identifier (e.g. `feat(auth): fix login redirect [ENG-123]`).
   - **Body** from the repo's PR template if it has one (e.g. `.github/pull_request_template.md`), filled in — summary, Linear issue URL, and validation. Otherwise a short summary plus the issue URL.

Show the branch name, the commit split, and the PR title before pushing so they're visible, then proceed.
