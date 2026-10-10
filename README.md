# agentic-support

Systematic support case management with documentation, runbooks, and knowledge capture.

## Quick Start

```bash
# Create a new case
/agentic-support create <customer-name> "<brief-description>"

# Find similar issues
/agentic-support find-similar "<search-terms>"

# Resolve a case
/agentic-support resolve <case-number>

# Create a runbook from a pattern
/agentic-support runbook-create <case-number> "<runbook-title>"

# Apply a runbook to new case
/agentic-support runbook-run <runbook-number> <case-number>

# Generate session summary
/agentic-support summary

# Audit support system health
/agentic-support audit
```

## Directory Structure

```
support/
├── INDEX.md                    # Master case index
├── inbound/                    # Active investigations
│   └── S###-description.md     # Open cases
├── customers/                  # Resolved cases by customer
│   └── customer-name/
│       ├── _customer-info.md   # Customer profile
│       └── S###-case.md        # Resolved case files
└── runbooks/                   # Reusable runbooks
    ├── INDEX.md                # Runbook index
    └── R###-title.md           # Runbook files
```

## Case Lifecycle

```
1. Report → /agentic-support find-similar (search first)
   ↓
2. Open → /agentic-support create
   ↓
3. Investigate → Document in case file
   ↓
4. Fix → Apply solution
   ↓
5. Resolve → /agentic-support resolve
   ↓
6. Pattern? → /agentic-support runbook-create (optional)
```

## Philosophy

**Every issue gets a case number.** No "I'll document it later."

**Customer context is sacred.** Read `_customer-info.md` before touching anything.

**Runbooks capture knowledge.** If it happened once, it'll happen again.

**Documentation is evidence.** Timestamps, error messages, commits — not narrative.

## Examples

### Creating a case
```bash
/agentic-support create acme-language-school "Checkout returns 500"
```
→ Creates `inbound/S005-checkout-500.md`

### Finding similar issues
```bash
/agentic-support find-similar "checkout 500 error"
```
→ Shows related cases and applicable runbooks

### Resolving and creating runbook
```bash
/agentic-support resolve S005
/agentic-support runbook-create S005 "Missing scope causes checkout 500"
```
→ Moves case to customer folder, creates reusable runbook

## See Also

- [Main skill file](SKILL.md) — Full documentation
- [reference/](reference/) — Detailed sub-command docs
- [agentic-sql](../agentic-sql/) — Similar pattern for SQL queries
