# Multi-Vault MCP Setup

> How Hermes connects to multiple Obsidian vaults simultaneously.

## Setup

Two MCP server instances are configured in `~/.hermes/config.yaml`:

### 1. `obsidian` — Jarvis Memory Vault

- **Path:** `C:/Users/Ganesh/Documents/Obsidian Vault`
- **Purpose:** Personal knowledge base, long-term memory, user profile
- **MCP Name:** `obsidian`

### 2. `obsidian-bg` — Bike Guardian Brain Vault

- **Path:** `C:/Users/Ganesh/projects/bike_guardian/brain`
- **Purpose:** Bike Guardian project second brain (558 notes — architecture, sprint kanban, completion logs, scoping docs, agent memory)
- **MCP Name:** `obsidian-bg`

## How It Works

Each MCP server instance is a separate `uvx obsidian-mcp-server` process with its own `OBSIDIAN_VAULT_PATH`. Hermes discovers tools from both servers and makes them available in the same session.

When you ask a question, Hermes routes to the right vault based on context:
- Questions about **you, your preferences, personal memory** → `obsidian` (Jarvis Memory)
- Questions about **Bike Guardian, its architecture, sprint status, tickets** → `obsidian-bg` (Bike Guardian brain)

## Vault Paths

| Vault | Windows Path | GitHub |
|---|---|---|
| Jarvis Memory | `C:\Users\Ganesh\Documents\Obsidian Vault` | [PremGanesh0/jarvis-memory](https://github.com/PremGanesh0/jarvis-memory) |
| Bike Guardian Brain | `C:\Users\Ganesh\projects\bike_guardian\brain` | [PremGanesh0/bike_guardian](https://github.com/PremGanesh0/bike_guardian) (embedded in repo) |

## Available Tools (per vault)

Each vault exposes the same MCP tool set:
- **Navigation:** `list_notes`, `search_notes`, `read_note`
- **Creation:** `create_note`, `append_to_note`, `patch_note`
- **Analysis:** `get_vault_stats`, `get_links`, `get_backlinks`
- **Graph:** `get_local_graph`
- **Skills:** `list_agent_skills`, `get_agent_rules`

## Notes

- The Bike Guardian brain is a **project-specific vault** embedded inside the `bike_guardian/` repo — it tracks the project's full lifecycle (scoping, architecture decisions, QA logs, sprint board).
- The Jarvis Memory vault is your **personal long-term memory** — profile, preferences, general knowledge.
- Both vaults can be queried in the same conversation; Hermes picks the right one based on what you're asking about.

---

*Configured: 2026-09-23*
