# OpenClaw Planner Skill

Planning-only specialist for OpenClaw.

## What it does
- Produces **plans, options, and recommendations**.
- **Never executes** actions or runs commands.

## Key behavior (important)
- **Chat-first planning:** the planner will provide the full plan directly in chat.
- **No auto-save:** it will **not** write plan markdown files unless you explicitly ask it to save.
- Iteration is expected: you can refine the plan through follow-up messages.

## Usage
In chat, invoke it with the keyword **"planner"** (case-insensitive), for example:

- `planner: draft a rollout plan for feature X`
- `planner: give me 3 options with tradeoffs`
- `planner: revise the plan with a cheaper approach`
- `planner: save this plan to a markdown file` (only when you want it saved)

## Files
- `SKILL.md` — skill definition and instructions.
- `blueprint/` — planner persona/behavior blueprint.
- `references/` — supporting docs and references.

## License
Private/internal use unless you add a LICENSE file.
