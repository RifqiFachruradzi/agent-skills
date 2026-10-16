# Root-Cause Investigation

The goal is the cause that explains **all** observed symptoms and that, when changed,
makes the problem impossible — not merely the first change that makes the error go away.

## The loop

```
Observe → Reproduce → Hypothesise → Experiment → Conclude → Fix → Verify → Prevent
```

### 1. Observe

Collect facts before theories:

- Exact error message, full stack trace, exit code, logs around the time of failure.
- When did it start? What changed then (deploy, dependency bump, config, data)?
- Who/what is affected and who/what is not? The difference is a strong clue.
- Environment: OS, runtime version, dependency versions (lockfile), env vars, flags.

### 2. Reproduce

- Turn the report into a single command or test that fails reliably.
- Shrink it: remove inputs, steps, and code until removing anything else makes it pass.
- Intermittent? Run it in a loop (`for i in $(seq 50); do cmd || break; done`), vary
  timing/concurrency/seed, and record the failure rate. A fix for a 10% flake needs
  enough runs to show the rate dropped to 0.

### 3. Hypothesise

- List several candidate causes, not one. Include "my assumption about X is wrong".
- Rank by likelihood × cheapness to test.
- Write each as a falsifiable statement: "If the cache key ignores locale, then
  requesting `/en` then `/id` returns the English page."

### 4. Experiment

Change one variable at a time. Useful tools:

| Technique | Use when |
|---|---|
| Read the code path end to end | Always — first |
| Targeted logging / print / assert | You need to see actual values |
| Debugger / breakpoints | Complex state, many branches |
| `git bisect run <test>` | It worked at some commit and broke later |
| Diff environments | Works locally, fails in CI/prod |
| Minimal standalone script | Suspect library/framework behaviour |
| Reverting a suspect change | Cheap confirmation of a suspected commit |
| Checking issue trackers / changelogs | Suspect a known upstream bug or breaking change |

Record every experiment and result — negatives eliminate hypotheses and belong in the
troubleshooting note.

### 5. Conclude with "5 Whys"

Keep asking *why* until you reach something you can change and that explains every symptom.

> Checkout returns 500.
> Why? `NoneType has no attribute 'price'`.
> Why? `get_product()` returned `None`.
> Why? The product ID in the cart was deleted from the catalogue.
> Why? Deleting a product does not clean up carts referencing it.
> **Root cause:** missing referential cleanup on product deletion.
> Symptom patch would be: `if product is None: continue` in checkout.
> Real fix: handle deletion (cascade/cleanup or soft-delete) **and** make checkout
> handle missing products explicitly with a clear user-facing message.

### 6. Fix, verify, prevent

- Write the regression test **first**, watch it fail, then fix.
- Verify against the original repro and the wider suite.
- Ask: where else could the same cause bite? (Search for the same pattern.)
- Prevent recurrence: test, type, validation, lint rule, monitoring, or docs.

## Symptom patches to challenge

When an AI agent (or you) proposes one of these, ask "what is the actual cause?":

- `try/except` or `catch` that swallows the error
- `sleep`, retries, or longer timeouts to hide a race or slowness
- Skipping, deleting, or loosening a test or assertion
- `// @ts-ignore`, `# type: ignore`, `as any`, `unwrap_or_default()` without reason
- Pinning or downgrading a dependency without knowing what changed
- Clearing caches / reinstalling as the "fix"
- Special-casing the one input from the bug report

Sometimes a workaround is the right business call. If so: label it as a workaround in
code and in the report, document the real cause, and create a follow-up.

## When you are stuck

- Re-read the error from the top. Re-check assumptions you marked as "obviously true".
- Explain the problem in writing to someone else (or in the troubleshooting note) —
  rubber-ducking frequently exposes the gap.
- Widen the search: changelogs, upstream issues, official forums.
- After a timebox, escalate with a clear write-up: symptoms, repro, what was ruled out,
  current best hypothesis, what you need.
