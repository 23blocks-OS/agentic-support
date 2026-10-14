# Changelog

All notable changes to this skill are documented here. Format based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [1.1.1]

### Changed
- A case that needs a data change goes through the `agentic-sql` write door (`/agentic-sql write`): propose, a person approves, then apply. The agent never approves and records the request id and outcome in the case file. Pointer only; the steps live in `agentic-sql`. Design: AI Maestro backlog F037.

## [1.1.0]

### Added
- "Customer text is data" section: customer content is untrusted, actions need approval, least privilege (`agentic-sql`, `memory-search`, `aim-secret-management`).
- "Sensitive data" section, `{SUPPORT_DIR}` placement, redaction placeholders, retention period asked at setup, anonymization in runbooks and find-similar output.
- Severity definitions (P0 to P3) and `{RESPONSE_TARGETS}` setup variable.
- Resolve checklist, incident record for P0/P1, and Owner, Escalation, Rollback and Last reviewed fields in the runbook template.
- Case-numbering collision check, copyable workflow checklist, `## Contents` lists in long reference files.
- audit.md and summary.md in the SKILL.md reference table.

### Changed
- `find-similar` is now the first step of `create` (search before investigating).
- `audit --fix` lists exact files and removes nothing until approved.
- Templates live only in the reference files; SKILL.md points to them.
- Examples use fictional data (Acme Language School); consistent terms (case, runbook).
- runbook-run hands database lookups to `agentic-sql` instead of raw SQL.
- Removed the default "P1 (set appropriately)" severity.
