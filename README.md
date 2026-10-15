# Code Review & Security Kit

Disciplined code review, OWASP-style security checks, safe refactoring, and dependency and secret hygiene, with findings ranked by severity and backed by evidence.

A setup for AI coding agents, made by [Kitloom](https://kitloom.site). One source, generated for every major agent: Claude Code, OpenAI Codex, Cursor, Google Antigravity, Gemini CLI, GitHub Copilot, Windsurf (Devin), opencode and Aider.

## Install

1. Download this repository as a ZIP, or use the **Check and download** button on its Kitloom page, which checks it first.
2. Read the files. Nothing here runs on its own: the kit has no hooks and no install scripts.
3. Copy the folders and files you want into the root of your project. `AGENTS.md` is the shared instructions file; edit it to fit your project.

## What is inside

### Skills

- `code-review` — Reviewing a range of changes against the repo's documented standards and the originating spec.
- `code-review-and-quality` — A multi-axis review before merging code written by you, another agent or a person.
- `ponytail-audit` — A ranked, whole-repo audit for bugs, security holes, untested risky code and dead weight.
- `security-and-hardening` — Handling user input, authentication, storage or external integrations, or auditing for vulnerabilities.
- `auth-implementation-patterns` — Reviewing or building JWT, OAuth2, session and role-based access control.
- `secrets-management` — Deciding where credentials live, how they are injected in CI, and how they are rotated.
- `oss-review` — Checking a dependency list or library for licence obligations and compatibility.
- `code-simplification` — Refactoring working code for clarity without changing its behaviour.

### Commands

Invoke by name (for example `/review-changes`).

- `review-changes` — Review the current diff or branch and report ranked findings without editing code.
- `security-audit` — Audit the code or a given path against OWASP-style categories and report evidence-backed findings.
- `dependency-check` — Review dependencies for necessity, maintenance, known-risk signals and licences.

### Sub-agents

- `code-reviewer` — Delegate for an independent, read-only review of a diff or set of files.
- `security-auditor` — Delegate for a read-only security pass over code that handles input, auth, data or secrets.

### Scoped rules

- `security-sensitive-code` — Extra scrutiny for auth, input handling, queries and crypto code.
- `secrets-hygiene` — Always-on rule: keep credentials out of code, logs, commits and chat.
- `dependency-manifests` — Care when adding or changing dependencies and lockfiles.

## Where each agent finds it

| Agent | Instructions | Skills and commands | Rules | Sub-agents |
|---|---|---|---|---|
| Claude Code | `CLAUDE.md` (imports `AGENTS.md`) | `.claude/skills/` | `.claude/rules/` | `.claude/agents/` |
| OpenAI Codex | `AGENTS.md` | `.agents/skills/` | listed in `AGENTS.md` | `.codex/agents/` |
| Cursor | `AGENTS.md` | `.agents/skills/`, `.claude/skills/` | `.cursor/rules/` | `.claude/agents/` |
| Google Antigravity | `AGENTS.md` | `.agents/skills/` | `.agents/rules/` | `.agents/agents/` |
| Gemini CLI | `AGENTS.md` via `.gemini/settings.json` | `.agents/skills/`, `.gemini/commands/` | listed in `AGENTS.md` | `.gemini/agents/` |
| GitHub Copilot | `AGENTS.md` | `.agents/skills/`, `.github/prompts/` | `.github/instructions/` | `.claude/agents/` |
| Windsurf (Devin) | `AGENTS.md` | `.agents/skills/`, `.windsurf/workflows/` | `.windsurf/rules/` | — |
| opencode | `AGENTS.md` | `.agents/skills/`, `.opencode/commands/` | listed in `AGENTS.md` | `.opencode/agents/` |
| Aider | `AGENTS.md` via `.aider.conf.yml` | — | — | — |

Skills are stored twice (`.agents/skills/` and `.claude/skills/`) because Claude Code reads only its own folder. Agents that read both may list a skill twice; delete the copy you do not need.

## Credits

The skills come from these open-source repositories, each under its own licence. Their licence and a notice are kept beside every copied skill.

| Skill | Source | Licence |
|---|---|---|
| `code-review` | [mattpocock/skills](https://github.com/mattpocock/skills) at `49dd158` | MIT |
| `code-review-and-quality` | [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) at `aeac41d` | MIT |
| `ponytail-audit` | [DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail) at `2961b52` | MIT |
| `security-and-hardening` | [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) at `aeac41d` | MIT |
| `auth-implementation-patterns` | [wshobson/agents](https://github.com/wshobson/agents) at `46891e7` | MIT |
| `secrets-management` | [wshobson/agents](https://github.com/wshobson/agents) at `46891e7` | MIT |
| `oss-review` | [anthropics/claude-for-legal](https://github.com/anthropics/claude-for-legal) at `4a6c651` | Apache-2.0 |
| `code-simplification` | [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) at `aeac41d` | MIT |

Everything else in this repository (instructions, rules, commands, sub-agents) is by Kitloom under the MIT licence.

## Use at your own risk

Read every file before you use it. Agent instructions can make an agent run commands on your machine. This kit was checked with Kitloom's pre-download check, which looks for warning signs but cannot prove anything is safe.
