# Verifying Technical Claims

Treat every technical statement — from an AI, a blog post, a colleague, or your memory —
as a claim with a confidence level until checked.

## Source ranking (most to least authoritative)

1. **Observed behaviour** in your environment: a test or experiment you ran.
2. **Source code** of the exact version installed (`node_modules/`, `site-packages/`,
   `go/pkg/mod/`, `~/.cargo/registry/`, vendored code, or the tagged release on GitHub).
3. **Official documentation for that version** (versioned docs, API reference, man
   pages, `--help`), plus official changelogs/release notes/migration guides.
4. **Official issue trackers and discussions** (maintainer answers carry weight).
5. Specifications and RFCs.
6. Reputable third-party content (well-known blogs, Stack Overflow answers with
   evidence) — check the date and version.
7. **AI-generated answers** and unsourced memory — hypotheses only.

## Find the version actually in use

```bash
# JavaScript / TypeScript
npm ls <pkg>           # or: cat node_modules/<pkg>/package.json | grep version
# Python
pip show <pkg>         # or: python -c "import pkg; print(pkg.__version__)"
# Go
go list -m all | grep <module>
# Rust
cargo tree -i <crate>
# Tools
<tool> --version
```

Then open the docs **for that version**, not the latest.

## Verification checklist

Before relying on a claim, ask:

- [ ] Does this API / flag / option exist in the installed version? (search the source)
- [ ] Are the parameter names, types, defaults, and return values as stated?
- [ ] Are there deprecation warnings or breaking changes between the documented version
      and ours?
- [ ] Does the claim hold for our platform/runtime/config (OS, Node vs. browser, sync
      vs. async, prod vs. dev mode)?
- [ ] Can I confirm it in a minimal experiment in under five minutes? Then do it.
- [ ] For performance claims: did I measure, with realistic data, multiple runs,
      before and after?

## Minimal experiments

Prefer a throwaway script over a debate:

```bash
python -c "import json; print(json.dumps({'a': float('nan')}))"   # does it allow NaN?
node -e "console.log(new URL('http://x/a b').pathname)"             # how is space encoded?
```

Keep experiments outside the repo (or in a scratch directory) so they don't end up in commits.

## Recording verification

In notes, PRs, and reports, distinguish:

- **Verified:** "Confirmed in `requests` 2.32.3 source (`sessions.py:L512`) that
  `timeout` is not applied to DNS resolution."
- **Documented, not tested:** "Docs for v5 state the default pool size is 10."
- **Unverified / assumption:** "Assuming the upstream API is idempotent — not confirmed."

Link the doc page, file and line, or command output. A reviewer should be able to
re-check your claim in one click.

## Questioning AI output

Productive follow-ups when an AI answer looks plausible:

- "Which documentation page or source file says that? Quote it."
- "Which version introduced this? Is it in our version (X.Y)?"
- "Write a minimal test that would fail if that claim were false." — then run it.
- "What are the reasons this approach could be wrong?"
