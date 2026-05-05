---
name: agentic-usability
description: Working area for agent usability — heuristics, criteria, and frameworks for evaluating whether APIs, CLIs, and MCP surfaces are usable by AI agents. Cite this when reviewing API/CLI/MCP designs, framing the agent-as-consumer view alongside human consumers, or defining what "good" means for agent-facing surfaces.
type: project
---

# Agentic Usability

**Core research question this work aims to answer:**

> What are the qualities of an API or CLI interface that make it easier or harder for an agent to reason about and use effectively?

Nielsen wrote his 10 usability heuristics in 1994 for human users of GUIs. There is no equivalent canon for agents as consumers of APIs, CLIs, and tool surfaces. This is a working area for building one — starting with HubSpot's developer platform context, generalizable beyond it.

## Why this exists

Two things changed at once in 2025–2026:

1. Agents became a real consumer of API and tool surfaces, not a hypothetical one (MCP, ChatGPT/Claude/Gemini connectors, multi-step tool use).
2. HubSpot publicly committed to "anything you can do inside HubSpot, you should be able to do through an API" (CPTO blog post, 2026-05-04). See `knowledge/strategy-docs/cpto-vision-2026-05.md`.

That combination means the API surface needs to be usable by two distinct readers — human developers and AI agents — and the constraints on each are different.

Some of what makes a surface usable for humans helps agents (consistent naming, good errors, clear scopes). Some doesn't (visual hierarchy, terse human-readable copy). Some things help agents specifically and don't matter for humans (typed errors over prose, lean response shapes, self-describing schemas).

This area is where we build the framework, then apply it to the work DPG ships.

## What's here (and what's coming)

| File | Purpose |
|---|---|
| `heuristics-v0.md` | First-cut list of 10 agent usability heuristics, working draft |

Planned:
- A scorecard or rubric, parallel to the DX quality rubric, for reviewing a surface against the heuristics
- A short paper / readout deck framing this for DPG and broader audiences (visibility play)
- Worked examples: applying the heuristics to a known HubSpot API or MCP tool

## Scope

**In scope:** API design, CLI design, MCP tool design, error semantics, schemas, naming/IA, scopes/permissions, audit/observability surfaces.

**Out of scope (for now):** prompt engineering, agent runtime design, model-side tool selection. The frame here is the *server side* of agent interaction — what the platform offers, not what the agent does with it.

## How to use this

When reviewing or designing an agent-facing surface:

1. Read `heuristics-v0.md`.
2. Apply each heuristic as a question. Where does the design fail it? Where is it ambiguous?
3. Note which heuristics are most load-bearing for the specific surface (APIs lean on H4, H5, H9; CLIs lean on H2, H7; MCP tools lean on H3, H8).
4. Feed findings back into the heuristics. This is v0 and will evolve.

## Open questions to work through

- How do these compose with human DX heuristics? Is one rubric enough, or two?
- Where is the line between "agent usability" and "agent capability"? A capability gap (no API for X) is different from a usability gap (the API for X is hard to use).
- How do we test these empirically? Synthetic agent runs against a surface? Telemetry from real MCP traffic?
- Who's the audience for the heuristics — internal API designers? External developers? Both? Different framings.

## Related knowledge
- `knowledge/strategy-docs/cpto-vision-2026-05.md` — the strategic context that makes this work matter now
- `knowledge/personas/developer-journey-lexicon.md` — the human journey we're paralleling for agents
- `knowledge/design-quality/dx-quality-rubric.md` — the human-side analog this work parallels
- `real-projects/api-ia-study/` — the API IA work that depends on these heuristics
