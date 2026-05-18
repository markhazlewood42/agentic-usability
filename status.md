# Agentic Usability — Project Status
_Last updated: 2026-05-07 (Eco All-Hands surfaced multiple parallel workstreams — Ed Langan's Epic Week talk, Rachel Kellam's agent API evaluations, Roger Brown's synthetic agent testing, Matt Rollender's Breeze Apps debugging agent; contacts and references captured in knowledge/eco-allhands-2026-05-07.md and todo.md)_

---

## Core research question

> **What are the qualities of an API or CLI interface that make it easier or harder for an agent to reason about and use effectively?**

This is the question the project exists to answer. Everything below — heuristics, scorecard, worked examples, readouts — is in service of building a defensible, citable answer to it. Hold this as the anchor when scoping work and evaluating whether a given direction moves the project forward.

The question is deliberately about *qualities of the interface*, not about agent capability or model behavior. Subject is the surface (APIs, CLIs, MCP tools); object is what makes it harder or easier to reason about. That framing keeps the work grounded in design, not ML research.

## What this is

A framework for evaluating and designing the usability of APIs, CLIs, and MCP tool surfaces from the perspective of AI agents as consumers. The parallel reference is Nielsen's 10 usability heuristics (1994) for human GUI usability:  There's no equivalent canon for agents, and the agent era now demands one.

Started 2026-05-04, triggered by HubSpot CPTO Duncan Lennox's open-ecosystem vision (`knowledge/strategy-docs/cpto-vision-2026-05.md`) committing publicly to "anything you can do inside HubSpot, you should be able to do through an API." With agents now first-class consumers of the platform's API surface, agent usability becomes a distinct design variable that needs heuristics, criteria, and evaluation tools.

## Why this matters (and why it's Mark's project)

Three reasons this is worth dedicated space:

1. **Direct relevance to active DPG work.** API IA Study (Rachel Kellam, Farheen Malik) is now load-bearing for the CPTO parity commitment. Whether the resulting IA serves agents well is a question this framework can answer. Release Management's "what counts as a breaking change" question intersects with these heuristics. Future MCP work, CLI design, and any new agent-facing surface needs this lens.
2. **It's a Principal-PD-shaped contribution.** A defensible, citable framework — eventually published as a readout, deck, or paper — is the kind of broad-impact artifact that maps to promotion criteria. AI-perspective-shaping work is one of Mark's named growth areas.
3. **No one else is building this.** There's a real gap. Whoever builds the canon early gets cited.

## Current state

**v0 heuristics drafted (2026-05-04).** Ten heuristics with rule, why-for-agents, fail mode, and HubSpot hot spots:

- H1 Discoverability without trial-and-error
- H2 Predictable shapes
- H3 Self-describing surface
- H4 Errors that are machine-actionable
- H5 Composability
- H6 Idempotency and safe retry
- H7 Token economy
- H8 Scoped intent
- H9 Naming as IA
- H10 Observability and audit

Working draft, intentionally unfinished. See `heuristics-v0.md`.

## Roadmap (rough phases, not a commitment)

### Phase 1 — Heuristics stabilize (in progress)
- v0 → v1 iteration with Mark
- Add missing heuristics: versioning (likely H11 candidate), docs-as-agent-surface (likely H12 candidate), trust boundaries (H13 candidate), stateful interactions (H14 candidate)
- Pressure-test against 1–2 specific HubSpot surfaces to surface gaps

**2026-05-05 — Phase 1 research-grounding pass complete:**
- Annotated bibliography written (`knowledge/prior-art.md`) — top 10 reading order, all major precursors and contemporaries cited with URLs
- Frameworks crosswalk written (`knowledge/frameworks-crosswalk.md`) — positioning relative to Nielsen 1994, Cognitive Dimensions, Clarke API-CD, Bloch, HAX, Anthropic *Writing Tools*, SWE-agent ACI, 12-Factor Agents, OWASP, DX Quality Rubric
- Concepts glossary written (`knowledge/concepts-glossary.md`) — vocabulary v1 must speak (ACI, schema-as-prompt, tool-name-as-affordance, context economy, error-as-second-chance-prompt, `pass^k` reliability, lethal trifecta, persona conditioning, surface-vs-capability, etc.)
- Annotated heuristics written (`heuristics-v0-annotated.md`) — every H1–H10 has prior art + distinct contribution + empirical hook; H11–H13 candidates drafted; latency and stateful-interactions discussed
- Shape-and-positioning decision doc opened (`decisions/2026-05-shape-and-positioning.md`) — five questions framed (artifact shape, one rubric or two, audience, term choice, derivation method) with tentative recommendations pending Mark's brainstorming pass

### Phase 2 — Scorecard / rubric
- Build the parallel to `knowledge/design-quality/dx-quality-rubric.md`
- Must / Should / Nice levels per heuristic
- Use to evaluate API IA Study output, an existing HubSpot API, and an MCP tool

### Phase 3 — Worked example(s)
- Apply the rubric to one well-known HubSpot surface (candidate: CRM v3 Contacts API or HubSpot MCP server)
- Document findings as a case study
- Use the case study to refine both rubric and heuristics

### Phase 4 — Readout / share
- Internal: deck or doc to DPG leadership and API Council
- Possibly external: short paper or blog if the framework lands well internally
- Visibility play tied to promo case

## Open questions (carried from `README.md`)

- How do these compose with human DX heuristics? One rubric or two?
- Where's the line between "agent usability" (surface design) and "agent capability" (what the platform can do)?
- How do we test these empirically? Synthetic agent runs? Telemetry from real MCP traffic? Both?
- Who's the audience — internal API designers, external developers, both? Different framings.

## Linear

No Linear project yet. This is currently Mark's personal IP/research project. May graduate to a real-projects entry once it has a stakeholder beyond Mark, or stay personal indefinitely depending on how it evolves.

## Slack Channels

None mapped (personal project, no team channel). If/when this becomes a shared workstream, candidates are #api-council-backroom (closest topical home) and #dpg-design.

## Files

| File | Purpose |
|---|---|
| `README.md` | Frame, scope, how to use, open questions |
| `heuristics-v0.md` | The 10-heuristic working draft (source of truth for the rules) |
| `heuristics-v0-annotated.md` | v0 pressure-tested against the literature — prior art, distinct contribution, empirical hook per heuristic; H11–H13 candidates |
| `knowledge/prior-art.md` | Annotated bibliography with top-10 reading order |
| `knowledge/frameworks-crosswalk.md` | Position relative to existing frameworks |
| `knowledge/concepts-glossary.md` | Load-bearing vocabulary v1 must speak |
| `knowledge/eco-allhands-2026-05-07.md` | Contacts, parallel workstreams, and design tensions from Eco All-Hands chat thread |
| `decisions/2026-05-shape-and-positioning.md` | Open Phase-1 decision doc on artifact shape, audience, term, derivation method |
| `status.md` | This file |

**Deferred for a follow-up session:** Track 4 skill scaffolding (`/lit-review` and `/heuristic-eval`). Workspace `skills/` directory is currently in an unexpected modified state (every skill file has been emptied locally vs git HEAD); not safe to add new skill files into that state without first resolving what's going on with the existing ones.

## Internal collaborators (from Eco All-Hands 2026-05-07)

See `knowledge/eco-allhands-2026-05-07.md` for full context.

| Person | Team / Role | Relevant work | Status |
|---|---|---|---|
| Edward Langan | API Foundations | Epic Week talk "Making Our Public APIs Work for Humans AND Agents"; research on human/agent API needs overlap | Follow up — get talk, compare with heuristics |
| Rachel Kellam | API Foundations | Agent API evaluations; API Design Guide compliance automation | Follow up — heuristics as evaluation criteria |
| Matt Rollender | Breeze Apps | Internal agent that debugs how well agents use dev docs + CLI | Follow up — get link to tool |
| Roger Brown | CLI | Synthetic agent testing (model/harness combos); CUJ automation via CLI | Follow up — empirical validation alignment |
| SJ Morris | DevRel | Has list of agent builder contacts for research | Follow up — get the DM'd list |
| Hina Shah | UXR | "Synthetic Users" concept; has a thinking group | Follow up — join thinking group |
| Chirag Chadha | — | Harness research reference (YouTube video) | Watch video |
| Sejal Parikh | — | Pushed back on "same for both" — agents stitch calls, humans don't | Captured as design tension |

## Cross-references

- `knowledge/strategy-docs/cpto-vision-2026-05.md` — strategic context that triggered this work
- `knowledge/personas/developer-journey-lexicon.md` — agent consumer track for journey phases 3–8 references these heuristics
- `real-projects/api-ia-study/status.md` — directly applies these heuristics (esp. H1, H2, H9)
- `real-projects/release-management/status.md` — has open thread on agent-aware release semantics
- `knowledge/design-quality/dx-quality-rubric.md` — the human-side analog this work parallels

## Working notes

**On framing the work for HubSpot consumption:** This is best positioned as design infrastructure for the parity commitment. "Open by design means designed *for whom*. Agents are a first-class consumer now — here's what good looks like for them." That framing keeps it grounded in the CPTO vision and avoids reading as theoretical.

**On collaboration with Claude Code:** Use this workspace to iterate. Each heuristic deserves a focused pass: real examples, edge cases, what fails the test, what passes it. Mark + Claude as a writing/thinking pair fits the model the rest of home-base operates on.

**On the name:** "Agentic usability" works for now. Alternatives considered: "agent UX," "AX (agent experience)," "machine-readable usability." If/when this goes external, may want to re-test the name. Internal use: agentic usability is fine.
