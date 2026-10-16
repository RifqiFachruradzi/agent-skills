# agent-skills

A collection of [Agent Skills](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)
for AI coding agents such as Claude Code, OpenCode, Codex, and similar tools.

## Skills

| Skill | Description |
|---|---|
| [`ai-assisted-engineering`](skills/ai-assisted-engineering/SKILL.md) | Disciplined workflow for building and modifying real software with an AI coding agent: root-cause investigation, verifying claims against official docs, Git hygiene, task prioritisation with measurable results, and clear English progress updates, troubleshooting notes, and handoffs. |

### `ai-assisted-engineering` covers

- **LLM fundamentals** — how models fail (hallucinated APIs, version drift, false success) and how to prompt and supervise a coding agent.
- **Root-cause investigation** — reproduce, hypothesise, experiment, 5 Whys, rejecting symptom patches.
- **Verification** — source ranking, checking the installed version, minimal experiments, recording what is verified vs. assumed.
- **Git workflow** — branching, commits, PRs, safety rules, investigation and recovery commands.
- **Task management** — measurable acceptance criteria, prioritisation, WIP limits, timeboxing, before/after metrics.
- **Communication** — clear English for instructions, progress updates, troubleshooting notes, help requests, and reviews, with ready-to-use templates.

```
skills/ai-assisted-engineering/
├── SKILL.md
├── references/
│   ├── llm-fundamentals.md
│   ├── root-cause-investigation.md
│   ├── verification.md
│   ├── git-workflow.md
│   ├── task-management.md
│   └── communication.md
└── assets/
    ├── progress-update.md
    ├── troubleshooting-note.md
    └── handoff.md
```

## Installation

**Claude Code (personal):**

```bash
mkdir -p ~/.claude/skills
cp -r skills/ai-assisted-engineering ~/.claude/skills/
```

**Claude Code (project):** copy the folder into `.claude/skills/` in your repository.

**Other agents (OpenCode, Codex, …):** place the folder where your agent loads skills,
or reference `SKILL.md` from your `AGENTS.md`.

The skill activates automatically when a task matches its description, or invoke it
explicitly, e.g. `/ai-assisted-engineering`.
