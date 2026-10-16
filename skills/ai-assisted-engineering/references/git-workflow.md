# Git Workflow for AI-Assisted Development

## Before you start

```bash
git status                 # uncommitted work? (don't trample someone's changes)
git branch --show-current  # right branch?
git fetch origin
git log --oneline -15      # recent history and the repo's commit style
```

Create a branch per task from the latest default branch:

```bash
git switch -c fix/login-plus-email origin/main
```

Branch names: `<type>/<short-description>` (e.g. `feat/`, `fix/`, `chore/`, `docs/`)
unless the repository uses another convention.

## While working

- Commit in **logical units** — each commit builds and makes sense on its own.
- Review before staging: `git diff`, then stage deliberately (`git add <paths>`, or
  `git add -p` to pick hunks). Avoid blind `git add -A`.
- Check what is staged: `git diff --staged`.
- Never commit: secrets/`.env`, credentials, large binaries, build output, local IDE
  files, debug prints. Check `.gitignore`.
- Sync with the base branch regularly to catch conflicts early (`git fetch` +
  merge or rebase, per repo convention, on **your own** branch).

## Commit messages

```
<type>(<scope>): <imperative summary, ≤ 72 chars>

Why the change was needed and what the root cause was.
What was changed at a high level, and anything notable for reviewers.

Refs: #123
```

- Subject says *what*, body says *why*. "Fix bug" is not a message.
- Follow the repo's existing style if it differs (check `git log`).

## Pull requests

A good PR description contains:

- **Summary:** what and why, in two or three sentences.
- **Root cause** (for fixes).
- **Changes:** short bullet list.
- **Verification:** commands run and results; screenshots for UI.
- **Risks / follow-ups:** what was not covered.

Keep PRs small and focused; split large work into a stack of reviewable PRs.

## Safety rules

| Never (without explicit permission) | Prefer |
|---|---|
| Push directly to `main`/default branch | Feature branch + PR |
| `git push --force` on a shared branch | `git push --force-with-lease` on your own branch only |
| Rewrite history others have pulled | A new commit, or `git revert` |
| `git reset --hard` / `git clean -fd` without looking | `git stash`, then inspect |
| Delete branches you didn't create | Ask the owner |
| Skip hooks (`--no-verify`) to get past failures | Fix what the hook reports |

## Useful investigation commands

```bash
git log -p -- path/to/file          # history of a file with diffs
git log -S 'functionName'           # commits that added/removed a string
git blame -L 40,60 path/to/file     # who/when/why for specific lines
git bisect start; git bisect bad; git bisect good <sha>
git bisect run ./repro.sh           # automate finding the breaking commit
git show <sha> --stat               # what a commit touched
git diff main...HEAD                # what your branch changes vs. its base
```

## Recovery recipes

| Situation | Recovery |
|---|---|
| Committed to wrong branch (not pushed) | `git switch -c right-branch` then reset the wrong one to its upstream |
| Need to undo a pushed commit | `git revert <sha>` |
| Lost a commit after reset/rebase | `git reflog`, then `git switch -c rescue <sha>` |
| Messy uncommitted changes | `git stash push -m "wip"` and inspect `git stash show -p` |
| Merge conflict | Read both sides; keep behaviour of both; rerun tests; regenerate lockfiles with the tool, not by hand |
