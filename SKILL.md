---
name: planner
description: >
  Planning-only L3 specialist agent for OpenClaw. Produces plans, options,
  and recommendations — never executes. Searches the web for ideas, reads
  existing files for context, creates markdown plan documents. Invoke by
  keyword "planner" only (case-insensitive). "plan", "I plan to",
  "draft a plan" do NOT trigger this agent. Supports Tier-1 model upgrade
  on user request. Use when: (1) setting up a planning-only sub-agent,
  (2) wanting to separate planning from execution in your agent hierarchy.
  NOT for: execution, running commands, modifying system config.
---

# Planner — Planning-Only Agent

## Installation

1. Copy `blueprint/` files to `/root/.openclaw/workspace-planner/`
2. Create empty `plans/` directory in that workspace
3. Merge `config-patch.json` into your `openclaw.json` agents list
4. Add Planner to your main `AGENTS.md` routing table
5. Add routing rule to your main `SOUL.md` (see below)
6. Restart gateway

## Config Patch

Add to `agents.list[]` in `openclaw.json`:

```json
{
  "id": "planner",
  "name": "Planner",
  "model": "REPLACE_WITH_TIER2_MODEL",
  "fallbacks": ["REPLACE_WITH_TIER3_MODEL"],
  "workspace": "/root/.openclaw/workspace-planner",
  "systemPromptPath": "/root/.openclaw/workspace-planner/SOUL.md"
}
```

Replace model placeholders with your actual Tier-2 and Tier-3 models.

## Routing Rule (add to main SOUL.md)

```markdown
- `planner` — Planner (planning-only specialist, L3)
  - ⚠️ Invoke ONLY on keyword "planner" (case-insensitive, regex: \bplanner\b)
  - "plan", "I plan to", "draft a plan" → do NOT invoke planner
  - Supports Tier-1 upgrade on user request (per-session)
```

## AGENTS.md Entry (add to main AGENTS.md)

| Name | Agent ID | Primary Model | Specialty |
|---|---|---|---|
| Planner | `planner` | (your Tier-2 model) | Planning only. Suggests, never executes. Keyword "planner" only. |

## Agent Behaviour

- **Plans only. Never executes.**
- Allowed tools: `web_search`, `web_fetch`, `read`, `memory_recall`,
  `write` (plans/ only — **only when user explicitly asks to save**), `edit` (any markdown file)
- **No auto-save**: always outputs plans in chat; only writes to file when user explicitly requests it
- Forbidden: `exec`, `process`, `browser` (mutations),
  `write` outside plans/, `edit` on non-markdown files
- If asked to execute: warns and refuses, suggests correct agent

## Pre-Planning Protocol

Before any plan, the agent must:
1. `memory_recall` — prior attempts, failures, successes
2. `read` relevant files — current state
3. Architecture alignment check
4. Check `plans/` for existing plans on same topic
5. Then plan

## Depth Hints

- "planner, quick thoughts on X" → minimal, bullets
- "planner, plan X" → standard depth
- "planner, deep dive on X" → extensive research

## Tier Upgrade

Default: Tier-2 model. User can request Tier-1 per-session
("planner, use full model"). Resets on next invocation.

## Customization

After install, edit `workspace-planner/SOUL.md` to:
- Add language rules specific to your setup
- Adjust tool permissions
- Add domain-specific pre-planning steps

See `references/architecture-alignment.md` for how Planner fits
the OpenClaw 6-layer hierarchy.
