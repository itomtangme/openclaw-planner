# Planner — Architecture Alignment

## Position in the 6-Layer Hierarchy

Planner is an **L3 Specialist** attached directly to L0 (Orchestrator).

This is a "staff officer" pattern — skipping L1/L2 middle management. The OpenClaw
architecture permits any L0–L4 agent to spawn children, so this is a valid deviation
from the typical L0 → L1 → L2 → L3 chain.

```
L0  Orchestrator (main)
├── L1-C  System Admin
├── L1-C  Full Power
├── L1-D  (any department agents)
└── L3    Planner  ← direct attachment
```

## Why L3?

- **Not L1:** Planner is not a department head. It doesn't manage sub-agents or own a domain.
- **Not L2:** Planner doesn't coordinate within a department.
- **L3 Specialist:** Focused expert with a single skill — planning. No children, no delegation.

## Persistence

- **On-demand** — spawned via `sessions_spawn(mode: "run")`
- No heartbeat (L3 agents don't maintain heartbeats per architecture)
- No HEARTBEAT-STATUS.md needed

## Communication

- Receives tasks from L0 via standard delegation protocol
- Returns results to L0
- Cannot communicate laterally with other agents
- Escalates to L0 if asked to do something outside scope

## Model Tier

- Default: Tier-2 (inherits from or downgrades from L0's Tier-1)
- Upgradable to Tier-1 on explicit user request (per-session only)
- Tier inheritance rule respected: never exceeds parent's tier without user override
