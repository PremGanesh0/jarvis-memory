# Antigravity CLI — Task Delegation Setup

> How to use `agy` (Antigravity CLI v1.2.9) to delegate tasks across the two vaults.

## Installation

- **Binary:** `C:\Users\Ganesh\AppData\Local\agy\bin\agy.exe`
- **Version:** 1.2.9
- **Install command (recommended):**
  ```powershell
  irm https://antigravity.google/cli/install.ps1 | iex
  ```
- **PATH note:** Binary lands in `%LOCALAPPDATA%\agy\bin` — may need manual PATH add or terminal restart.

## Usage Patterns

### One-shot (non-interactive)
```bash
agy --print "<prompt>" --dangerously-skip-permissions
# or with workdir:
agy --print "<prompt>" --dangerously-skip-permissions --workdir "C:/path/to/project"
```

### With a specific model
```bash
agy --print "<prompt>" --model "Gemini 3.1 Pro (High)" --dangerously-skip-permissions
```

### Bounded long runs (background)
```bash
agy --print "<prompt>" --dangerously-skip-permissions --print-timeout 20m --background
```

## Connecting to Both Vaults

Antigravity can read files directly from both vaults without MCP:

| Vault | Path |
|---|---|
| Jarvis Memory | `C:\Users\Ganesh\Documents\Obsidian Vault` |
| Bike Guardian Brain | `C:\Users\Ganesh\projects\bike_guardian\brain` |

Pass both paths via `--add-dir` or reference them in the prompt.

## Task Assignment Examples

### Analyze Bike Guardian project state
```bash
agy --print "Read brain/PROJECT_STATUS.md from C:/Users/Ganesh/projects/bike_guardian and summarize: completion %, LIVE vs SIGNED OFF counts, top 5 shipped, top 5 open items" --dangerously-skip-permissions --workdir "C:/Users/Ganesh/projects/bike_guardian"
```

### Cross-vault query
```bash
agy --print "Search both C:/Users/Ganesh/Documents/Obsidian Vault and C:/Users/Ganesh/projects/bike_guardian/brain for notes about authentication. Combine findings from both vaults." --dangerously-skip-permissions
```

### Assign a development task
```bash
agy --print "Review the open issues in the bike_guardian repo on GitHub. For each issue, analyze the codebase and propose a fix plan. Output in markdown." --dangerously-skip-permissions --workdir "C:/Users/Ganesh/projects/bike_guardian"
```

## Configuration

Settings file: `C:\Users\Ganesh\AppData\Local\gemini\antigravity-cli\settings.json`

Key settings:
- `permissions.allow` — grant specific tool permissions
- `trustedWorkspaces` — mark project dirs as trusted
- `allowNonWorkspaceAccess` — allow access outside workspace

## Notes

- `agy --print` returns plain text — no JSON envelope
- Default print timeout is 5m; raise with `--print-timeout`
- Permission prompts in headless mode auto-deny — use `--dangerously-skip-permissions` or configure `permissions.allow` in settings.json
- Run from the project directory for workspace-aware behavior

---

*Installed: 2026-09-23*
