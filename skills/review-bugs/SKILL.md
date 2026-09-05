---
name: review-bugs
description: "Review a PR, diff, or named code area for consequential, reachable bugs. Use for correctness reviews that demand concrete evidence and minimal false positives. Exclude style critiques, speculative hardening, and immaterial test or documentation observations."
---

# Review material bugs

Find consequential defects through complete execution paths. Report only candidates that survive the evidence and materiality gates below.

Treat broad discovery and strict reporting as separate activities. Investigate plausible failures aggressively. Do not fill the report with speculative candidates.

## Establish the review boundary

Pin the reviewed revision and comparison base. Use the PR base or the user's supplied boundary. Inspect the complete diff and applicable behavior contract.

For a change review, identify defects the change introduces, worsens, or exposes through a new reachable path. For an explicit audit, inspect existing defects within the named area.

Read current requirements and relevant earlier review replies. A passing test, resolved thread, accepted suggestion, or reviewer confidence score does not establish correctness.

Review requests authorize inspection and applicable diagnostic checks. Keep probes and mutations in an isolated scratch directory or checkout. Publishing findings and changing application code depend on the user's authorization.

## Build candidates from behavior

For each changed operation, name its entry point, state, external effects, and required invariant. Read complete changed symbols, callers, downstream consumers, shared fixtures, and relevant dependency code.

Consult the applicable sections of [failure-patterns.md](references/failure-patterns.md). Use these dimensions to select them:

- State, concurrent actors, retries, and interruption points.
- Contracts, identity, serialization, and input boundaries.
- Trust, supported environments, and external operations.
- Tests, native UI behavior, and executable instructions.

Follow a hypothesis across files until its consequence or an effective guard becomes clear. A pattern match starts an investigation. It is not a finding.

## Admit a finding only when every gate passes

| Gate | Evidence needed |
| --- | --- |
| Scope | The defect belongs to the requested review boundary. A change review identifies the responsible change. |
| Contract | A current requirement or established supported behavior defines the expected result. The reviewer did not invent the requirement. |
| Reachability | A concrete caller, actor, input, or feasible operation sequence reaches the failure in a supported environment. |
| Materiality | The failure has a specific, nontrivial consequence for users, data, security, cost, availability, delivery, or recovery. |
| Proof | A faithful reproduction or a complete source proof establishes the failure after applicable guards and recovery. |

Rare failures can be material. Crashes, retries, and concurrent requests need no prior production incident when their supported execution path is established.

Reject naming, formatting, duplication, unused abstractions, cosmetic polish, generic hardening, and unsupported-platform concerns without a material failure. Do not promote a tiny discrepancy through a severity label.

For performance or cost, establish a reachable workload and meaningful impact. An extra allocation or theoretical complexity increase alone is insufficient.

For tests, require a concrete false success or failure that affects a material behavior or the real delivery path. A missing assertion or untested branch alone is insufficient. A mutation must represent a plausible failure of that behavior. Arbitrary mutations can make any test appear incomplete.

For documentation, establish an incorrect action or program result when someone follows the instruction. Harmless wording, caller counts, and stale commentary do not qualify.

## Try to disprove the candidate

Read [counterexamples.md](references/counterexamples.md) before promoting candidates. Check the strongest alternative explanation for each candidate:

- A caller validates, normalizes, serializes, or restores the state.
- A transaction, compare-and-set, unique constraint, or recovery pass prevents the claimed result.
- An exception superclass catches the error.
- A shared fixture isolates the state or supplies the missing dependency.
- The supported invocation or pinned library differs from the assumed one.
- A current requirement deliberately permits the result.

Inspect the applicable mechanism. Do not dismiss a candidate merely because a reply claims that a guard exists. Scope restrictions and immutable tests also do not disprove a defect. Report a proven, material defect in scope even when another file or owner must carry the fix.

Prefer an executable reproduction through the real entry point. Control concurrency with barriers or deferred responses. Observe the complete result after recovery. Use a positive control to check the setup.

A complete source proof can suffice for a deterministic failure. Name the input, exact path, consequence, and evidence for every relevant premise. Do not substitute an assumed framework behavior or serialized fake for concurrency or dependency evidence.

Keep a candidate unconfirmed when a material premise remains unknown. Continue safe checks that can resolve it. Do not publish it as a finding or treat it as disproved.

## Report only useful results

Recheck the cited revision and lines. Consolidate repeated reports of one cause. Retain independent failure modes and material consequences. Rank by demonstrated impact, using the repository's severity definitions when available.

For each finding, provide:

- The concrete trigger and material consequence.
- The responsible file and line, with the affected caller when needed.
- The expected and actual behavior, supported by the current contract.
- The proof command and result, or the complete source proof.
- The required repair outcome. Recommend a specific implementation only when the evidence supports it.

Do not duplicate an existing CI diagnostic unless the review adds a distinct material consequence or shows that the check misses it. Do not add style notes, optional improvements, speculative findings, or quotas of findings.

Finish when the relevant paths and candidate hypotheses are checked. If none qualify, say: "No confirmed material defects found." State material coverage limits briefly. If evidence was unavailable, call the review incomplete rather than clean.

## Evaluate changes to this skill

Use [evaluation.md](references/evaluation.md) when validating or tuning the skill. The aim is higher recall of material defects with zero false or immaterial reports. That aim is not a proven performance claim.
