# Counterexamples and materiality controls

These illustrative cases explain which assumptions the reviewer must check. They are not claims about a specific project or universal exemptions for similar code.

## Accepted does not mean correct

A reviewer claims that a parser misses a decoding exception. The existing handler catches its superclass and already converts the error into the expected validation result.

An author can accept a redundant explicit catch without proving the original allegation. Inspect exception inheritance before reporting an uncaught error.

## Library names do not establish lifecycle behavior

A reviewer assumes that an asynchronous client binds to an event loop at construction. Suppose the installed implementation instead binds on first use. Construction outside the loop does not establish the alleged violation.

An accepted cleanup improvement does not validate that mechanism. Inspect the pinned implementation before reporting a lifecycle violation.

## Shared state can have shared isolation

A reviewer sees a shared rate limiter in route tests and predicts counter accumulation. A suite-level setup hook can provide fresh storage before every case and restore it afterward.

The local test body need not contain that protection. Inspect fixture scope and setup before reporting contamination.

## A local gap can have end-to-end recovery

A deletion helper can miss a record that a later reconciliation pass removes. A proposed local fallback can alter that record so reconciliation no longer finds it.

Follow the full recovery path before confirming a local failure. Confirm that a proposed fix preserves that path.

## A possible environment is not a supported environment

A script can fail under a standalone invocation while its supported launcher supplies the correct dependencies. That unsupported invocation does not establish a deployment defect.

Check the actual launcher and deployment contract. Do not report failure under an invented invocation.

## Test quality needs a program consequence

Duplicated fakes, inaccurate test names, and absent assertions do not establish a material defect. Neither does an arbitrary mutation that removes unrelated working behavior.

A qualifying example is an ownership check that masks a separate identifier mismatch. Use two owned resources to isolate the mismatch. Establish that the mismatch violates a material supported operation before reporting it.

Another qualifying example is a safety gate that reports verification without running any required test. Demonstrate that a materially invalid change receives that false success. An empty set alone does not prove harm if the gate reports that no verification occurred.

## Similar descriptions can have different materiality

An old profile response can populate a different user's session. That is an information exposure when the path is established.

A harmless flash of the same user's cached avatar is not equivalent. A missing CSS color alone also does not establish a material failure. Keep presentation concerns only when they impede access, misrepresent results, or expose information.

## Scope and truth are different decisions

A protected test, file boundary, deferred ticket, or resolved thread does not refute a demonstrated bug. Conversely, a technically valid old defect does not belong in every change review.

Establish truth, materiality, and relevance independently. Report a material in-scope failure even when its repair needs another owner. Do not append unrelated debt to a focused review unless the user requests it.

## Apply mechanisms, not historical labels

Use failure mechanisms and disproof checks to assess the current code. Another reviewer's priority labels, acceptance decisions, and finding counts do not determine whether a new finding belongs in a report.
