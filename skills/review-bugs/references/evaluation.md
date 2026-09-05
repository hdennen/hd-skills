# Evaluate precision, materiality, and recall

Read this file when testing or tuning the skill. It is not part of every code review.

The target is higher recall of confirmed material defects than a chosen baseline, with zero observed false or immaterial reports. A finite evaluation cannot guarantee zero future false positives.

## Build the reference cases

Pin the source revision that each reviewer saw. Establish the behavior contract from the requirements and supported environment at that revision.

Adjudicate each candidate through a faithful reproduction or complete source proof. Keep a matching fixed or guarded variant when possible. Treat accepted and resolved comments as evidence to inspect, not truth labels.

Include cases that test distinct decisions:

| Case | Expected review decision |
| --- | --- |
| Delayed response reveals the old user's profile in a new session | Report after proving the reachable session transition. |
| Runtime failure becomes success after a log append overwrites the exit status | Report after executing the supported command sequence. |
| Concurrent moves in a hierarchy form a cycle despite separate revision checks | Report after proving a feasible schedule under the actual database semantics. |
| Newly inserted records lack the timestamp every supported date query reads | Report after checking the real insert-to-query path. |
| Native radios for separate records share a group because their labels match | Report when one action silently changes another record's effective selection. |
| An ancestor exception handler already catches the alleged missing error | Reject the missing-handler claim. |
| An autouse fixture resets the supposedly shared counter for every test | Reject the contamination claim. |
| A pinned async client binds on first use, contrary to a construction-time allegation | Reject that lifecycle mechanism. |
| Duplicate fakes or a missing success assertion with no material false verdict | Omit the observation. |
| A harmless color difference or same-user cached repaint | Omit the observation. |
| Failure exists only on an unsupported platform or invented launcher | Reject the reachability premise. |
| A required database or deployment fact remains unavailable | Mark the affected review coverage incomplete. Do not invent a verdict. |

Use controls across the complete pattern catalog, especially identity, parsing, trust, recovery, and executable instructions. This table is a calibration set, not complete evaluation coverage.

## Run a blind comparison

Keep every review round, near duplicate, and derived variant of one PR in the same dataset partition. Tune on development cases. Evaluate separately on unseen PRs or held-out cases.

Give each reviewer the same source, context, tools, and execution budget. Hide prior review comments, adjudication, fixes, and later regression tests. A reviewer that reads the answers cannot establish comparative recall.

Match findings by trigger, failure mechanism, and consequence. Adjudicate valid new findings absent from the reference set. Score each underlying defect once.

Do not equate different reporting policies. Exclude immaterial findings from the positive reference set for both reviewers. Count an immaterial report as a failure of this skill's output policy.

## Record the result

- Confirmed material defects found and missed, by category.
- False reports, including incorrect mechanisms and unreachable triggers.
- Immaterial reports, even when the literal observation is true.
- Duplicate reports and unconfirmed candidates promoted to findings.
- Incomplete coverage, elapsed time, and cost.

Compute precision as confirmed material reports divided by all distinct reports. Compute recall as confirmed material defects found divided by reference defects.

An empty report has undefined precision, not automatic perfect precision. Coverage gaps remain visible and do not disappear from recall accounting. Separate unmeasurable cases and explain their exclusion.

Promote a skill revision only after it improves or preserves recall with zero observed false and immaterial reports on the holdout. State the case count and uncertainty. Do not claim superiority from structural validation or this calibration table alone.
