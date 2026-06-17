# Agent Skills

[![skills.sh](https://skills.sh/b/BhaskarDey772/agent-skills)](https://skills.sh/BhaskarDey772/agent-skills)

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

### Via skills CLI (recommended)

```bash
npx skills add BhaskarDey772/agent-skills
```

### Manual — Claude Code

```bash
git clone https://github.com/BhaskarDey772/agent-skills.git /tmp/agent-skills
cp -r /tmp/agent-skills/skills/preserve-api-contract ~/.claude/skills/
```

### Manual — VS Code / GitHub Copilot

```bash
git clone https://github.com/BhaskarDey772/agent-skills.git /tmp/agent-skills
cp -r /tmp/agent-skills/skills/preserve-api-contract .agents/skills/
```

See [agentskills.io](https://agentskills.io) for other client paths.

## Usage

Once installed, the skill activates automatically when your AI assistant detects you are editing a backend route or service. No manual trigger needed.

To invoke it explicitly in Claude Code:

```
use preserve-api-contract
```
