# Agent Strategy: Subagents vs. Agent Teams

**Date:** 2026-03-10
**Status:** Approved

## Problem

The superpowers skills only know about subagents — fire-and-forget workers that report results back. Claude Code now supports Agent Teams: coordinated multi-instance sessions with shared task lists, inter-agent messaging, and self-coordination. The skills need to know when each approach is appropriate.

## Design

### Two new/updated skills

#### 1. New skill: `choosing-agent-strategy`

A lightweight decision-tree skill that evaluates the current task and recommends subagents vs. agent teams. Triggers when facing parallelizable work and the right approach isn't obvious.

**Decision criteria:**

| Factor | Subagents | Agent Teams |
|--------|-----------|-------------|
| Communication | Only need results back | Agents need to share findings, challenge each other |
| Task coupling | Independent, no shared state | Cross-cutting, agents inform each other's work |
| Complexity | Focused, well-scoped tasks | Multi-role research, feature building, debugging across files |
| Cost sensitivity | Lower token usage | Higher token usage (each teammate = separate Claude instance) |
| Coordination | Main agent manages all | Teammates self-coordinate via shared task list |

**Includes:**
- Decision flowchart (dot graph)
- Quick-reference table
- Inline enable check: if agent teams are recommended but not enabled, provide the one-liner to enable them
- Examples of each pattern applied to the same scenario

#### 2. Expand `dispatching-parallel-agents`

Rename conceptually to cover both dispatch patterns. Add a new section on Agent Team orchestration alongside the existing subagent patterns.

**New content:**
- "Agent Team Pattern" section paralleling the existing "The Pattern" section
- How to structure team prompts (team lead instructions, teammate roles, task decomposition)
- When to require plan approval for teammates
- How to use the shared task list for coordination
- Verification after team completion
- Real examples showing agent teams applied to research, debugging with competing hypotheses, and cross-layer feature work

**Preserved content:**
- All existing subagent guidance stays intact
- The skill becomes the comprehensive reference for both approaches

### Cross-references

- `choosing-agent-strategy` points to `dispatching-parallel-agents` for execution details
- `subagent-driven-development` and `executing-plans` get a brief note that for complex coordinated work, consider `choosing-agent-strategy` first
- Agent Teams experimental status noted inline with enable instructions

### What we're NOT changing

- `subagent-driven-development` — stays focused on sequential task execution with review gates (subagents are the right tool here)
- `executing-plans` — stays focused on single-session plan execution
- No new agents or scripts needed — this is guidance/decision-making, not tooling

## Files to create/modify

1. **Create** `skills/choosing-agent-strategy/SKILL.md` — new decision-tree skill
2. **Modify** `skills/dispatching-parallel-agents/SKILL.md` — expand with agent team patterns
3. **Modify** `skills/subagent-driven-development/SKILL.md` — add cross-reference
4. **Modify** `skills/executing-plans/SKILL.md` — add cross-reference
