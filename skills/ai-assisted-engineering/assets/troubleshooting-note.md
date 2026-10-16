# Troubleshooting Note — <short problem title>

**Status:** Investigating | Root cause found | Resolved | Workaround in place
**Owner:** <name>  **Started:** <date/time>  **Last updated:** <date/time>

## Symptom
<What is observed, by whom, since when. Exact error message in a code block.>

```
<error / stack trace / log excerpt>
```

## Environment
- Branch / commit: <sha>
- Runtime & key dependency versions: <e.g. Node 20.11, next 14.2.3>
- Where it happens / where it doesn't: <local ✗, CI ✓, staging ✗>

## Reproduction
```bash
<minimal command(s) that reproduce the problem>
```
Failure rate: <always | n/N runs>

## Hypotheses and experiments
| # | Hypothesis | Experiment | Result | Conclusion |
|---|---|---|---|---|
| 1 | <…> | <…> | <…> | Ruled out / Confirmed / Inconclusive |

## Root cause
<The cause that explains all symptoms, with evidence (file:line, doc link, command output).>

## Fix
<What was changed and why it addresses the cause. Link the commit/PR.>
<If this is a workaround: say so, and link the follow-up for the real fix.>

## Verification
- <Repro now passes: command + result>
- <Tests / lint / typecheck results>

## Prevention / follow-ups
- <Regression test, monitoring, docs, lint rule, ticket links>

## References
- <Official docs, changelog entries, upstream issues consulted>
