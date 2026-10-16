---
name: ai-assisted-engineering
description: Disciplined workflow for building or modifying real software with an AI coding agent (Claude Code, OpenCode, Codex, or similar). Use when working in a Git repository on a bug fix, feature, refactor, or investigation where correctness matters — especially when debugging, when an AI-suggested fix looks plausible but unproven, when a technical claim needs verifying against official documentation, when planning and prioritising multi-step work, or when writing progress updates, troubleshooting notes, handoffs, or technical instructions for another developer.
---

# AI-Assisted Engineering

A working discipline for shipping real software with an AI coding agent while staying
accountable for the result. The agent is fast; you (or the agent following this skill)
supply the judgment: find root causes, verify claims, keep Git history clean, finish
what you start, and report results someone else can check.

## Core principles

1. **The model is a collaborator, not an oracle.** LLM output is a hypothesis until
   evidence confirms it. See [references/llm-fundamentals.md](references/llm-fundamentals.md).
2. **Fix causes, not symptoms.** The first fix that makes the error disappear is
   rarely the right one. See [references/root-cause-investigation.md](references/root-cause-investigation.md).
3. **Verify before you claim.** Read the official docs, read the source, run the
   code. "It should work" is not a status. See [references/verification.md](references/verification.md).
4. **Small, reviewable, reversible changes.** Branch, commit in logical units, never
   rewrite shared history. See [references/git-workflow.md](references/git-workflow.md).
5. **Finish and measure.** A task is done when it is verified and reported with
   numbers, not when the code is written. See [references/task-management.md](references/task-management.md).
6. **Write so the other developer doesn't need to ask.** See
   [references/communication.md](references/communication.md) and the templates in `assets/`.

## Workflow

Follow these phases in order. Skip a phase only when it truly does not apply, and say so.

### 1. Understand the task

- Restate the goal in one sentence and the acceptance criteria as a checklist.
- Identify what "done" means measurably (tests pass, latency < X ms, error rate 0,
  feature visible at URL Y).
- Ask the collaborator only questions that change what you will build. Resolve the
  rest from the code, the docs, or a sensible default, and state the default you chose.

### 2. Orient in the repository

Before editing anything:

```bash
git status                      # clean tree? which branch?
git log --oneline -15           # recent context and commit style
git branch --show-current
```

- Read the README, CONTRIBUTING, CLAUDE.md / AGENTS.md, and the build/test config
  (`package.json`, `pyproject.toml`, `Makefile`, CI workflows).
- Find how a contributor runs tests, lint, and typecheck locally. You will need these
  commands to prove your change.
- Locate the code involved with search (`grep`/`rg`), then read it — do not edit code
  you have not read.

### 3. Plan and prioritise

- Break the work into steps small enough to verify individually.
- Order by: blockers first → highest value / highest risk → nice-to-haves.
- Track the steps in a visible checklist and update it as you go. Keep exactly one
  step "in progress".
- Timebox investigations. If a timebox expires, stop, write down what you learned,
  and decide (or ask) whether to continue, change approach, or escalate.

### 4. Investigate (for bugs and unexpected behaviour)

1. **Reproduce** the problem with a minimal, repeatable command. No repro → no fix;
   gather more data instead.
2. **Read the full error** — the whole stack trace, the first error, not the last.
3. **Form hypotheses** and rank them by likelihood × ease of testing.
4. **Test one hypothesis at a time** with a targeted experiment (log, assert, bisect,
   minimal script). Record the result, including negatives.
5. **Ask "why" until you reach a cause you can change** and that explains *all* the
   symptoms. Use `git bisect` when you know a good and a bad commit.
6. **Reject symptom patches**: catching and ignoring exceptions, adding sleeps/retries
   to hide races, disabling or loosening tests, pinning a version without knowing why,
   casting away type errors. If one is truly the right short-term call, label it as a
   workaround and document the real cause and follow-up.

Detailed techniques: [references/root-cause-investigation.md](references/root-cause-investigation.md).

### 5. Verify claims before relying on them

Whenever you (or the AI) assert an API signature, config option, CLI flag, version
behaviour, or performance property:

- Check the **official documentation for the version actually in use** (lockfile,
  `--version`), or the library source in `node_modules` / `site-packages` / vendored code.
- Prefer a 5-line experiment over a confident paragraph.
- Mark anything unverified as unverified in your notes and reports.

Source ranking and checklists: [references/verification.md](references/verification.md).

### 6. Implement

- Match the surrounding code: naming, structure, error handling, comment density.
- Keep the diff minimal and on-topic. Note unrelated problems you find; do not fix
  them silently in the same change.
- Add or update a test that fails without the fix and passes with it.
- Never commit secrets, generated noise, or debug leftovers.

### 7. Prove it works

Run, in this order, and read the output:

1. The reproduction from step 4 → now passes.
2. The new/changed tests.
3. The project's lint, format, and typecheck.
4. The relevant wider test suite.
5. For user-visible changes, actually run the app and exercise the feature.

Then re-read your own diff adversarially (`git diff --staged`): what would a reviewer
or CI reject? Fix it before committing.

### 8. Commit and push

- One logical change per commit, imperative subject ≤ 72 chars, body explains *why*.
- Follow the repository's existing convention (Conventional Commits, ticket prefixes…).
- Push your feature branch; never force-push a branch others use; never push to the
  default branch without permission.

Details and recovery recipes: [references/git-workflow.md](references/git-workflow.md).

### 9. Report

Report outcomes faithfully with measurable results:

- What changed and why (one or two sentences).
- Evidence: commands run and their results, metrics before → after.
- What was **not** done or not verified, known risks, and next steps.
- If tests fail, say so and include the relevant output. Never round "mostly works"
  up to "done".

Use [assets/progress-update.md](assets/progress-update.md),
[assets/troubleshooting-note.md](assets/troubleshooting-note.md), or
[assets/handoff.md](assets/handoff.md) as the format.

## Working with another developer

- Agree up front on ownership: who decides scope, who reviews, who merges.
- Post short async updates at natural checkpoints (plan ready, root cause found,
  PR up, blocked) rather than only at the end.
- When blocked, say exactly what blocks you, what you tried, and what you need —
  in one message.
- Hand off work so it can be resumed cold: branch name, state, next step, open questions.
- Disagree with evidence, not opinion: link the doc, the benchmark, the failing test.

## Red flags — stop and reconsider

- You are about to apply a fix you cannot explain.
- The AI's answer cites an API, flag, or option you have not seen in the docs or source.
- A test is being skipped, deleted, or weakened to get green.
- The diff touches many files unrelated to the task.
- You have retried the same approach three times with no new information.
- You are about to report "done" without having run anything.
- You are about to run a destructive command (`reset --hard`, `push --force`,
  `rm -rf`, dropping data) without having looked at what it will destroy.

## Quick self-check before saying "done"

- [ ] Root cause identified and explained (for bugs)
- [ ] Every non-obvious technical claim verified against docs, source, or an experiment
- [ ] Test added/updated; lint, typecheck, and tests run and passing (or failures reported)
- [ ] Diff reviewed, minimal, and free of secrets and debug code
- [ ] Committed with a clear message and pushed to the correct branch
- [ ] Report written with measurable results, limitations, and next steps
