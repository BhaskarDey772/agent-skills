# Agent Skills

A collection of reusable [Agent Skills](https://agentskills.io) for AI coding assistants (Claude Code, GitHub Copilot, Codex, etc.).

## Skills

### [`preserve-api-contract`](skills/preserve-api-contract/SKILL.md)

Prevents silent API response shape changes that crash frontend consumers.

**Activates when:** modifying any backend route, controller, or service that returns a response.

**What it enforces:**
1. Snapshot the current response shape before editing
2. Edit the code
3. Compare field-by-field after editing
4. If shape changed — stop, list what broke, wait for explicit approval

## Installation

### Claude Code

Copy any skill folder into `~/.claude/skills/`:

```bash
cp -r skills/preserve-api-contract ~/.claude/skills/
```

### VS Code / GitHub Copilot

Copy into `.agents/skills/` in your project root:

```bash
cp -r skills/preserve-api-contract .agents/skills/
```

### Any agentskills-compatible client

Copy the skill folder into the skills directory your client reads from. See [agentskills.io](https://agentskills.io) for client-specific paths.
