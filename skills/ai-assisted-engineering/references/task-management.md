# Task Management, Prioritisation, and Measurable Results

## Define "done" before starting

Write acceptance criteria that someone else could check:

| Vague | Measurable |
|---|---|
| "Make the page faster" | "p95 load time of `/dashboard` < 1.5 s on the staging dataset (currently 4.2 s)" |
| "Fix flaky test" | "`test_sync` passes 200/200 consecutive runs (currently ~9% failure)" |
| "Improve error handling" | "All 4xx from the payments API produce a user-facing message and a log entry with request ID" |
| "Clean up the code" | "Remove the 3 unused modules listed in #88; no change in test results" |

## Break work down

- Split into steps of roughly ≤ 1–2 hours of work, each independently verifiable.
- First step is usually a spike: reproduce, read the code, confirm the plan.
- Identify dependencies and blockers explicitly.

## Prioritise

Order the list using:

1. **Blockers** — anything that stops you or your collaborator.
2. **Impact × urgency** — what the user/business needs first.
3. **Risk first** — the unknown, hard part early, so surprises appear while there is
   still time.
4. **Cheap wins** — small, valuable items that unblock or de-risk.
5. **Nice-to-haves** — last, and only if time remains.

A quick matrix:

|  | Urgent | Not urgent |
|---|---|---|
| **Important** | Do now | Schedule |
| **Not important** | Delegate / batch | Drop or note |

## Finish what you start

- Limit work in progress: one task in progress at a time.
- Before switching, either finish to a verifiable checkpoint or write a handoff note
  (see `assets/handoff.md`) and commit/push the WIP to your branch.
- Scope creep goes into a "follow-ups" list, not into the current change.
- Timebox investigations (e.g. 45 min). At the end of the box: report findings,
  decide to continue, change approach, or escalate.

## Measure

Capture **baseline** before changing anything, then the **result** after, with the same
method:

- Tests: count passing/failing, failure rate over N runs.
- Performance: p50/p95 latency, throughput, memory, bundle size, build time — multiple
  runs, same machine/data.
- Quality: number of errors in logs, lint/type errors, coverage of the changed code.
- Scope: issues closed, endpoints migrated, files converted (x of y).

Record the exact command used so the measurement is reproducible.

## Report

Every report answers:

1. What was done (outcome, not activity).
2. Evidence with numbers (before → after).
3. What remains, risks, and blockers.
4. Next step and owner.

Be faithful: partial results are reported as partial; failing tests are reported as
failing with output; skipped steps are named.
