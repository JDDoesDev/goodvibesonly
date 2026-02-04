# Contributing to VibeCheck

## Adding VibeCheck as a Claude Code Skill

Claude Code skills live in `~/.claude/skills/` (global) or `.claude/skills/` (project-level).

### 1. Create the skill directory

```bash
mkdir -p ~/.claude/skills/vibecheck
```

### 2. Create the skill manifest

Create `~/.claude/skills/vibecheck/skill.md`:

```markdown
---
description: Security scanner for vibe-coded projects
trigger: vibecheck
---

# VibeCheck Security Scanner

Run the VibeCheck scanner to catch hardcoded secrets, SQL injection, XSS, and other vulnerabilities.

## Usage

Scan staged files (pre-commit):
```bash
python3 /path/to/vibecheck/scripts/scan.py --staged
```

Scan specific files:
```bash
python3 /path/to/vibecheck/scripts/scan.py <files>
```

Scan all tracked files:
```bash
python3 /path/to/vibecheck/scripts/scan.py --all
```

## Exit Codes
- 0: No issues found
- 1: HIGH or MEDIUM severity issues
- 2: CRITICAL severity issues
```

### 3. Update the path

Replace `/path/to/vibecheck` with the actual path where you cloned the repo.

### 4. Use it

Now Claude Code responds to `/vibecheck` or natural language like:
- "vibecheck my staged files"
- "run a security scan"
- "check for vulnerabilities"

## Adding Security Patterns

Add new patterns to the `PATTERNS` list in `scripts/scan.py`:

```python
Pattern(
    name="Pattern Name",
    severity="CRITICAL|HIGH|MEDIUM",
    pattern=r"your-regex-here",
    description="What this catches",
    fix="How to fix it",
    languages=["js", "ts"]  # or ["all"]
)
```

### Guidelines for patterns

- **CRITICAL**: Direct security vulnerabilities (secrets, injection, disabled security)
- **HIGH**: Dangerous patterns that usually indicate vulnerabilities (XSS, weak crypto)
- **MEDIUM**: Code smells that may indicate security issues (debug flags, TODOs)

### Testing your pattern

Test against the intentional vulnerabilities file:

```bash
python3 scripts/scan.py tests/vulnerable.js
```

Add test cases to `tests/vulnerable.js` for new patterns.

## Code Style

- Single-file architecture: all scanner code stays in `scripts/scan.py`
- Zero external dependencies: stdlib only (Python 3.6+)
- Patterns are data, not code: security checks are declarative

## Pull Requests

PRs welcome for:
- New vulnerability patterns
- Language-specific improvements
- Reduced false positives
- CI/CD integrations
- Documentation improvements
