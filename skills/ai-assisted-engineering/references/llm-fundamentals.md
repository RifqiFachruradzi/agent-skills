# LLM Fundamentals for Coding-Agent Users

What you need to know about large language models to use them safely on real code.

## How an LLM produces an answer

- An LLM predicts the next token based on patterns in its training data and the
  current context window. It optimises for *plausible*, not *true*.
- It has a **knowledge cutoff**. Libraries released or changed after the cutoff may be
  described from memory of older versions.
- It only "knows" your project through what is in its **context window**: files it has
  read, command output, your messages. Anything it has not read, it is guessing about.
- Output is **non-deterministic**. The same prompt can produce different code; a
  confident tone carries no information about correctness.

## Typical failure modes

| Failure mode | What it looks like | Countermeasure |
|---|---|---|
| Hallucinated API | Function, flag, or config key that does not exist | Check docs / source for the installed version |
| Version drift | Correct for v2, broken on your v4 | Read the lockfile; consult versioned docs |
| Symptom fix | Wraps error in `try/except: pass`, adds retry/sleep | Demand a root-cause explanation |
| Over-reach | Rewrites unrelated code "while it's there" | Review the diff; keep scope tight |
| False success | "Fixed! Tests should pass now" without running them | Require command output as evidence |
| Stale context | Edits based on a file version it read earlier | Re-read before editing after changes |
| Sycophancy | Agrees with a wrong premise in your prompt | Ask neutrally; ask it to argue the opposite |
| Lost context | Forgets constraints in long sessions | Keep a written plan/checklist; restate constraints |

## How coding agents work

A coding agent (Claude Code, OpenCode, Codex CLI, Cursor agent, Aider…) wraps an LLM in
a loop: it reads files, runs shell commands, edits code, observes results, and repeats.
That means:

- It can **verify its own work** — if you (or this skill) require it to.
- It can also **cause real side effects**: delete files, push commits, call APIs.
  Review permission settings and destructive commands.
- Project instruction files (`CLAUDE.md`, `AGENTS.md`, `.cursorrules`, etc.) shape its
  behaviour on every task. Keep them accurate: build commands, test commands, conventions.

## Prompting an agent effectively

- State the **goal, constraints, and acceptance criteria**, not only the action.
  - Weak: "Fix the login bug."
  - Strong: "Users with `+` in their email get 401 on login (repro: `pytest tests/test_auth.py::test_plus_email`). Find the root cause, fix it, add a regression test, and don't change the public API."
- Point it at the evidence: error output, failing test, file paths, docs URL.
- Ask for a plan first on non-trivial work; review it before it edits.
- Ask it to **show** verification (command + output), not to assert it.
- When an answer seems off, ask: "What is the evidence for that? Where in the docs/source?"

## Context hygiene

- Long sessions degrade. Summarise state into a checklist or notes file and start fresh
  when the agent starts contradicting earlier decisions.
- Keep secrets out of prompts and logs.
- Large generated files and dependencies waste context — point the agent at the
  specific files that matter.
