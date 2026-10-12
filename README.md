# agentic-support: Claude Code skill for customer support cases and runbooks

[![Claude Code skill](https://img.shields.io/badge/Claude_Code-skill-D97757)](https://docs.claude.com/en/docs/claude-code/skills)

**agentic-support is an agent skill (`SKILL.md`) that makes Claude Code keep a written trail for every customer problem.** It opens a numbered case per report, records what was found, resolves it, and turns recurring problems into runbooks that are reused next time.

Cases are plain Markdown files in your project, with one folder per customer, so the history is searchable by people and by agents.

## What it does

- **Numbered case per report.** `create` takes the next `S###` number, re-checks for collisions and files the case under `inbound/`.
- **Search first.** `find-similar` looks through past cases and runbooks before anyone starts investigating.
- **Explicit lifecycle.** `resolve` runs a checklist and moves the case from `inbound/` to the customer's folder, so the location shows the state.
- **Runbooks from real cases.** `runbook-create` turns a resolved case into a reusable guide (with owner, escalation, rollback and last-reviewed fields); `runbook-run` applies it to a new case.
- **Safe by default.** Customer text is treated as untrusted data, secrets are redacted from notes, and actions that touch production or the customer need approval first.

## Install

### Option 1: through the AI Maestro plugin

The skill is part of the `ai-maestro` plugin from [ai-maestro-plugins](https://github.com/23blocks-OS/ai-maestro-plugins):

```bash
claude plugin marketplace add 23blocks-OS/ai-maestro-plugins
claude plugin install ai-maestro@ai-maestro-plugins
```

Claude Code namespaces plugin skills, so the command may appear as `/ai-maestro:agentic-support`.

### Option 2: standalone

```bash
git clone https://github.com/23blocks-OS/agentic-support.git ~/.claude/skills/agentic-support
```

Or copy `SKILL.md` and the `reference/` folder into `~/.claude/skills/agentic-support/`. For one project only, use `.claude/skills/agentic-support/` inside it.

## Usage

```bash
/agentic-support find-similar "checkout 500 error"      # search first
/agentic-support create acme-language-school "Checkout returns 500"
/agentic-support resolve S005
/agentic-support runbook-create S005 "Missing scope causes checkout 500"
/agentic-support runbook-run R003 S006                  # reuse it on the next case
/agentic-support summary                                # end of session
```

- `create` makes `inbound/S005-checkout-500.md` and adds it to the index.
- `find-similar` shows related cases and applicable runbooks.
- `resolve` moves the case to the customer folder; `runbook-create` writes the runbook.

On first use the skill reads `CLAUDE.md` for its settings (support directory, index path, response targets, retention period) and asks for anything missing. It does not invent response targets or a retention period.

## Sub-commands

| Command | What it does |
|---|---|
| `find-similar` | Search the index and runbooks for related cases. The first step. |
| `create` | Open a numbered case in `inbound/` and add it to `INDEX.md`. |
| `resolve` | Run the resolve checklist, move the case to the customer folder, update status. |
| `runbook-create` | Create a reusable runbook from a resolved case. |
| `runbook-run` | Execute a runbook's steps for a new case. |
| `summary` | End-of-session summary of cases worked. |
| `audit` | Read-only health check: orphan files, missing customer info, stale inbound cases. |

Detailed playbooks are in [reference/](reference/).

## Directory structure it maintains

```
support/
├── INDEX.md                    # master case index
├── inbound/                    # active investigations
│   └── S###-description.md
├── customers/                  # resolved cases by customer
│   └── customer-name/
│       ├── _customer-info.md   # customer profile
│       └── S###-case.md
└── runbooks/
    ├── INDEX.md
    └── R###-title.md
```

## Case lifecycle

```
Report -> find-similar (search first)
       -> create (open a case)
       -> investigate (document in the case file)
       -> fix
       -> resolve
       -> pattern? runbook-create
```

## How it works and why it is safe

- **Customer text is data.** Reports, emails and logs are quoted in the case file, never followed as instructions. No command, query or runbook step runs because a report asks for it.
- **Approval before action.** The exact action is shown before any production write, any message to the customer, or any `audit --fix`.
- **Least privilege.** Database lookups go through the read-only [agentic-sql](https://github.com/23blocks-OS/agentic-sql) skill, not raw SQL.
- **No secrets in notes.** Passwords, tokens, API keys and full payment data are replaced with placeholders such as `[REDACTED:token]`. Other customers are anonymized in runbooks and `find-similar` output.
- **Support files stay out of git** by default; if they are inside a repository, they are listed in `.gitignore`.

## Principles

1. **Every issue gets a case number.** No "I'll document it later".
2. **Customer context is sacred.** Read `_customer-info.md` before touching anything.
3. **Runbooks capture knowledge.** If it happened once, it will happen again.
4. **The case location is its state.** Never leave a resolved case in `inbound/`.
5. **Documentation is evidence.** Timestamps, error messages and commits, not narrative.

## Part of the AI Maestro ecosystem

agentic-support ships in the `ai-maestro` plugin for [AI Maestro](https://ai-maestro.23blocks.com), a dashboard and orchestrator for fleets of AI coding agents ([source on GitHub](https://github.com/23blocks-OS/ai-maestro)). It also works on its own in any Claude Code setup.

## Related skills

- [agentic-sql](https://github.com/23blocks-OS/agentic-sql): safe, read-only SQL against production databases, with a saved query library.
- [agentic-seo](https://github.com/23blocks-OS/agentic-seo): SEO audit and implementation for websites.

## Changelog

See [CHANGELOG.md](CHANGELOG.md). Contributions are welcome; see [CONTRIBUTING.md](.github/CONTRIBUTING.md).

## License

MIT. See [LICENSE](LICENSE).
