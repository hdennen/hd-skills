---
name: work-project
description: Coordinate a project's development tasks through implementation, independent review, and merge using the current session's native subagents with configurable worker and reviewer models. Discover repository settings and conventions from the current checkout. Use only when the user explicitly authorizes the captain to merge.
---

# Work project

Run the selected project's authorized task queue through workers, independent reviewers, and merge. Use the current checkout as the repository and discover its publication remote, integration branch, and conventions from repository configuration and instructions. Merge authorization does not authorize releases, deployments, unrelated work, or changes to repository protections.

All workers and reviewers run through the native subagent capabilities of the session where this skill is invoked. The runtime is inherited from that session; only the model and optional effort vary by role.

## Arguments

Accept named arguments or their unambiguous natural-language equivalents. Apply the role settings through the current session's native subagent controls.

| Argument | Meaning and default |
| :--- | :--- |
| `--project <name-or-id>` | Project whose tasks form the queue; required unless already clear from context. |
| `--worker-model <id>` | Implementation model; use the user's existing selection, otherwise the current session's model. |
| `--reviewer-model <id>` | Review model; use the user's existing selection, otherwise the resolved worker model. |
| `--worker-effort <level>` | Optional implementation reasoning effort, when supported by the current session. |
| `--reviewer-effort <level>` | Optional review reasoning effort, when supported by the current session. |

Example (substitute available model identifiers):

```text
work-project --project "Project name" --worker-model <implementation-model> --reviewer-model <review-model>
```

Resolve and report both role configurations before dispatch. Apply model and effort through the session's actual subagent controls. Verify inheritance is supported when relying on the session model. Leave omitted effort at the session's subagent default. Do not silently substitute an unavailable model or explicitly selected effort.

Create workers and reviewers as native subagents of the invoking session. Reviewers may share the worker's model, but each review must start in a fresh context without the worker's conversation. If the session cannot create the required subagents or apply the selected settings, report that limitation before claiming tasks. Do not launch another runtime or CLI as a fallback. If repository execution requirements conflict with native session delegation, surface the conflict before dispatch.

## Discover the repository contract

Read applicable agent instructions, contribution guidance, build configuration, CI definitions, and existing change-request templates. Resolve these facts before dispatching dependent work:

- The selected project's task source, authorized scope, acceptance criteria, dependencies, and existing branches or pull/merge requests.
- Integration branch, publication remote, branch/commit conventions, and permitted merge method.
- Workspace setup, dependency installation, relevant checks, and any required execution location.
- Required reviews, status checks, protected paths, approvals, and credential-loading procedures.
- Available task-tracker, code-host, claim/lock, and notification integrations.

Use the repository's supported environment loader and credential source; do not assume credential variable names, file locations, hooks, or shared dependencies. Do not create new infrastructure to replace a missing optional integration.

Load tasks from the selected project through its existing tracker or repository-backed project records. Do not require a separate task-list argument. Map tracker states to their actual workflow when one is used; do not assume state names or automatic closure. Use an existing claim protocol if present, otherwise keep task ownership in the captain's run record. External notifications are optional and require an authorized destination and scope. Missing chat or coordination services do not block a repository that does not require them.

Resolve the remote and integration branch from Git configuration, repository instructions, and existing change requests. Ask only if those sources leave a material ambiguity before publishing; do not require repository, remote, base, or task-list arguments or a new configuration file.

## Start

1. Load all tasks in the selected project's authorized scope, following pagination when needed. Inspect existing work before assigning it; do not take over another active owner's work without authorization.
2. Record each task's owner, dependencies, branch, change-request URL, current head, review result, and checks. Use the existing project record or a local run record.
3. Claim work through the configured protocol, if any. Send a kickoff only through an authorized notification integration.

## Per task

1. **Build.** Fill [worker-brief.md](worker-brief.md) with the resolved repository contract, task text, acceptance criteria, and relevant specifications. Dispatch the selected worker in an isolated workspace. Update the tracker if configured. Run independent tasks in parallel within available capacity. For dependent tasks, wait for prerequisites or use the repository's established stacking workflow; reassess the diff and checks when the base changes.
2. **Review.** Start a fresh native reviewer on the exact candidate head and resolved base, using a detached review worktree or equivalent isolated snapshot. Supply the task contract and relevant repository instructions, including a review skill if one is available. Require concrete, reachable correctness findings with locations, failing scenarios, and evidence. Record the reviewed head and base alongside the result. Distinguish a completed review with no findings from an incomplete or blocked review. Evaluate findings, have the worker address accepted issues, and record evidence for declined ones. Review the updated head after fixes or rebases.
3. **Open or update.** Use the selected code host and repository template. Link the task using the source's supported syntax, explain the behavior change, map acceptance criteria to verification, and record the reviewed head and results. Preserve existing pull/merge requests. If the repository requires an early draft to run checks, open it at that point and keep it unready until review completes.
4. **Settle.** Wait for the actual required checks and approvals using the code host's supported tools. Inspect failures and human or automated review threads. Fix valid findings through the worker, explain declined findings with evidence, and resolve threads only where permitted. Re-run checks when appropriate; do not assume a particular bot, job name, check count, or re-review trigger. Refresh all gate information after a head change. If checks time out or a required service is unavailable, record the blocker; absence of results is not success.
5. **Merge.** Confirm that the current head is the reviewed head, required checks and approvals pass, blocking threads are resolved, and repository merge policy is satisfied. Use the permitted merge method and an exact-head precondition where supported. If the host cannot guard the head atomically, prevent concurrent updates through its supported locking/queue mechanism or report that limitation before merging. Confirm the merge, update the task record, and unblock dependents. Do not bypass hooks, protected paths, or branch protections.

## Finish

Continue until the scoped tasks are merged or the remaining work needs authority, information, or an external change. Report completed work, change-request links, verification, and specific blockers. Release owned claims and stop services started for this run. Remove only run-owned workspaces whose work is preserved; retain failed reviews or uncommitted work for inspection. Update parent tasks, persistent records, and notifications only according to the selected workflow.
