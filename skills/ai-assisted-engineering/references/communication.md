# Clear Technical Communication in English

Written updates are how a remote collaborator sees your work. Optimise for a reader
who is busy, may read on a phone, and was not in your head while you worked.

## Principles

1. **Lead with the conclusion.** Status or answer first, details after.
2. **Be specific.** Names, numbers, file paths, commands, links — not "some issues".
3. **Separate facts from guesses.** "Confirmed:", "Likely:", "Unverified:".
4. **One idea per sentence; one topic per paragraph.** Short sentences survive
   non-native readers and machine translation.
5. **Use active voice and plain verbs.** "The cache returns stale data" — not
   "Stale data is being returned by the caching layer in some situations".
6. **Make asks explicit.** Who needs to do what, by when.
7. **Format for scanning.** Headings, bullets, code blocks for commands and errors.

## Words to replace

| Avoid | Use instead |
|---|---|
| "It doesn't work" | "`npm test` fails with `TypeError: x is undefined` at `cart.ts:42`" |
| "Fixed some bugs" | "Fixed double-charge on retry (#214) and timezone offset in reports (#219)" |
| "Should be fine now" | "Verified: 3 integration tests pass; not yet tested on staging" |
| "ASAP" | "By 15:00 UTC Thursday, because the release cut is Friday" |
| "I think maybe it could be the DB?" | "Hypothesis: slow query on `orders` (no index on `user_id`). Testing next." |
| "Did a lot of investigation" | "Ruled out: network, cache, config drift. Remaining suspect: ORM N+1 in `list_orders`." |

## Writing technical instructions

- State prerequisites first (versions, access, env vars).
- Number the steps; one action per step.
- Put every command in a code block exactly as it should be typed.
- Show the expected output or how to tell the step succeeded.
- Add a "Troubleshooting" section for known failure points.
- Test the instructions yourself from a clean state before sharing.

Example:

```markdown
### Run the API locally

Prerequisites: Docker 24+, Node 20, access to the `.env.example` values.

1. Copy the env file:
   `cp .env.example .env`
2. Start the database:
   `docker compose up -d db`
   Expected: `db` shows `healthy` in `docker compose ps`.
3. Install and start:
   `npm ci && npm run dev`
   Expected: `Listening on http://localhost:3000`.

Troubleshooting: if step 3 fails with `ECONNREFUSED 5432`, the database is not ready —
wait 10 s and retry, or check `docker compose logs db`.
```

## Progress updates

Short, regular, and consistent. Use `assets/progress-update.md`. Send at checkpoints:
plan agreed, root cause found, PR opened, blocked, done.

## Troubleshooting notes

Write while investigating, not after. Use `assets/troubleshooting-note.md`. Include
dead ends — they save the next person hours.

## Asking for help

Include in one message:

1. What you are trying to achieve.
2. What happens instead (exact error / output).
3. How to reproduce.
4. What you already tried and ruled out.
5. Your current best hypothesis.
6. The specific question or decision you need.

## Review comments

- Distinguish blocking from optional: prefix with `blocking:`, `nit:`, `question:`.
- Explain the *why* and suggest a concrete alternative.
- Comment on the code, not the person.
