# Failure patterns

Read the sections selected by the changed behavior. These patterns describe recurring failure mechanisms across applications and development tools. Apply the reporting gates in `SKILL.md` to every candidate.

## State and concurrency

Track each read through its eventual write. Identify actors that can change the same state between those operations.

Probe a stale preflight, two updates to different records that share an invariant, and concurrent first inserts. A revision check on one record does not necessarily protect an ancestor chain or related record.

Deliver an old response after a newer request, socket mutation, deletion, navigation, or user change. Check shared entities as well as individual query pages.

Test retry, cancellation, reconnect, and terminal completion from reachable intermediate states. Check markers and leases across repeated runs.

Disproof: inspect the actual atomic predicate, isolation behavior, serialization, cancellation, or generation guard. Distinguish a temporary state from a terminal error.

## Partial failure, retries, and cleanup

List writes, enqueue operations, external commands, and cleanup in execution order. Fail each applicable boundary and inspect the complete durable state.

Test failure after commit but before enqueue. Test failure after enqueue but before acknowledgment. Test a process death after a claim. Then retry the same request and a distinct request that shares the marker.

Check whether a logging command overwrites the original exit status. Distinguish an inspection error from confirmed absence. Check stalled operations that hold shared locks.

Disproof: follow existing retries, transactions, compensation, reconciliation, and lease expiry. Do not prescribe another recovery mechanism before understanding the existing one.

## Contracts and wiring

Trace identifiers, namespaces, revisions, statuses, provenance, and activity IDs from producer to consumer. Include serializers, stored records, events, caches, and alternate entry points.

Compare consumer mappings against every real producer. Exercise legacy records when the system still supports them. For a new runtime, inspect dispatch, liveness, cancellation, finalization, and image cleanup.

Disproof: inspect supported external launchers and shared adapters. A local search with no caller is insufficient when the entry point is external.

## Parsing and input boundaries

Select adverse values the real input boundary can admit: absent, null, empty, whitespace, wrong type, Unicode, quoted paths, or nested declarations.

Check integers separately from booleans in Python. Check Git path quoting, rename records, hidden files, and newline delimiters. Compare equivalent target sets in different orders.

Disproof: inspect upstream schema enforcement and exception inheritance. A deliberately unsupported input needs no successful result, but rejection must preserve the established boundary behavior.

## Identity, joins, and pagination

Create distinct records with the same name, text, or timestamp. Check whether identifiers retain their native database type through storage and cursors.

Place equal sort keys across a page boundary. Check missing join keys before database lookup. Test day boundaries for inclusive date queries.

Disproof: establish the actual ordering and consistency contract. Keyset pagination does not promise a snapshot when sort keys change. Stable input order can satisfy repeatability without a new tie-breaker.

## Trust and security

Name the actor that controls each input and the authority of the operation that consumes it. Follow values into shell commands, paths, URLs, privileged jobs, and tenant queries.

Check whether a fallback reads caller-controlled data after trusted data becomes unavailable. Check the revision that supplies a verifier. For file operations, consider symlinks and special files at reachable paths.

Distinguish successful inspection, confirmed absence, denied permission, timeout, and missing executable. Check URL scheme, credentials, and origin separately.

Disproof: inspect actual privileges, data ownership, validation, and deployment topology. A self-administered input is not automatically a hostile actor boundary. Do not invent a second user or privilege level.

## Runtime environment

Exercise the supported entry command with its actual interpreter, PATH, dependency set, and platform. Cron, non-login SSH, and interactive shells can differ.

Check successful execution, not only executable discovery. Compare installed dependencies with the code revision that runs. Inspect pinned library code when a claim depends on its lifecycle.

Disproof: locate the real caller. A hypothetical standalone invocation failure is irrelevant when all supported callers use a configured application environment.

## Test validity

Identify the exact material behavior the test claims to protect. Construct the smallest realistic incorrect implementation that still passes it.

Keep unrelated guards satisfied. For identifier mismatch, use two resources owned by the same user. For pagination, make the boundary depend on the tie-breaker. For cleanup, cause the protected operation to fail.

Check that mocks replace the dependency actually called. Check fake query operators and transaction semantics. Read shared fixtures before claiming leaked state.

Disproof: run the real test setup and inspect all assertions that protect the behavior. A helper can leave some coverage to another test. A base import failure does not establish behavioral discrimination.

Report a test defect only when it falsely certifies a concrete material behavior or breaks a supported delivery path. Exclude test naming, duplicated fakes, and optional coverage improvements alone.

## UI behavior

Use a real browser when the result depends on native grouping, focus, disabled controls, or paint order. Duplicate display names can join independent radio controls.

Check late responses across user changes, nullable stored profile fields, and fallback presentation needed to complete the workflow.

Disproof: establish a broken action, inaccessible operation, misleading result, or information exposure. A brief harmless repaint or unused color token alone is immaterial here.

## Executable documentation and workflows

Follow the documented action through actual commands and state predicates. Check whether an operation lacks necessary inputs or contradicts another branch.

Disproof: distinguish a supported operational procedure from stale explanation or approved requirement removal. Report only instructions that cause a material program or operational failure.
