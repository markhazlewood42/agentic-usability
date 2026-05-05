---
name: agentic-usability-frameworks-crosswalk
description: Crosswalk between Mark's agent usability framework and existing precursors / contemporaries. For each existing framework, where v1 sits — extends, mirrors, or distinguishes from it. Used to position v1 in the literature.
type: project
---

# Frameworks Crosswalk

Where the agent usability framework sits relative to prior art. For each major existing framework: what it covers, what it misses for agents-as-consumer, and how the v1 should relate.

This file should be cited inline in v1 — every position taken on shape, scope, or naming should map to a row here.

---

## Nielsen's 10 Usability Heuristics (1994)

**What it covers:** Human-facing UI heuristics. Visibility of system status, match to real world, user control, consistency, error prevention, recognition over recall, flexibility, minimalist design, error recovery, help/docs.

**What it misses for agents:** No concept of context-window economy, no schema-as-prompt, no determinism / idempotency, no tool selection problem, no security / trust boundaries, no agency-vs-affordance distinction. Built around a human's perceptual and short-term-memory constraints — agents have neither.

**How v1 relates: explicit mirror, explicit extension.**

Borrow the structural template (numbered list, derived from observed failures, durable across surfaces) — that template is part of why Nielsen's 10 are still cited 30 years later. Call out which heuristics translate (status visibility → tool result observability; help/docs → schema descriptions; error recovery → error-as-second-chance prompt) and which need new equivalents (context economy, idempotency declaration, tool selection, trust boundaries).

If the v1 derivation method follows Nielsen 1994's factor analysis (factor-analyze N agent failures rather than declare from intuition), v1 inherits Nielsen-grade credibility. That's a Track 3 decision.

---

## Cognitive Dimensions of Notations (Green & Petre 1996; Blackwell & Green 2003)

**What it covers:** 14 dimensions for evaluating notational / programming-language / UI design (abstraction gradient, viscosity, hidden dependencies, premature commitment, role-expressiveness, hard mental operations, error-proneness, etc.). Method-level: continuous tradeoffs rather than binary rules.

**What it misses for agents:** Not derived from agent-as-reader. No token-cost dimension. No notion of "schema as prompt." Doesn't separate "reason about" from "use effectively" the way agent failure modes naturally split.

**How v1 relates: borrow dimensional thinking, evolve specific dimensions.**

Several CDs translate cleanly:

- **Viscosity** → cost of recovering from a wrong tool call (retry, undo, side-effect cleanup)
- **Hidden dependencies** → tool side effects not surfaced in schema
- **Role-expressiveness** → does the tool name reveal its role (direct H9 ancestor)
- **Premature commitment** → does the agent have to lock in choices it can't yet see consequences of
- **Hard mental operations** → reasoning effort the agent must do over the surface (token-expensive in attention, not just output)
- **Error-proneness** → call sites that statistically produce wrong-shape arguments

If v1 lands as "heuristics + dimensions" (a Track 3 option), the dimensions cut comes mostly from CDs.

---

## Steven Clarke's API Cognitive Dimensions (MS Research, 2004–05)

**What it covers:** Persona-based usability studies for APIs. Adapts CDs to API-specific dimensions including abstraction level and learning style. Runs studies with three developer personas at MS.

**What it misses for agents:** Pre-LLM. Assumes a human reader holding mental models over time, not a stateless agent reading a schema in a 200K token window. Personas are human-shaped.

**How v1 relates: explicit acknowledgment as the precursor.**

Frame agent usability as the parallel research program. Clarke did persona-driven API usability for human developers; v1 does it for agent consumers. The persona-conditioning insight transfers directly — an API that's usable for a small specialized agent isn't necessarily usable for a generalist orchestrator.

This is the citation that gives v1 academic lineage in the API-usability tradition.

---

## Joshua Bloch — *How to Design a Good API and Why It Matters* (2006)

**What it covers:** Timeless API design maxims. "When in doubt, leave it out," "Names matter," "Every API is a little language," "Minimize mutability," "Documentation matters." Framed for human library consumers.

**What it misses for agents:** Pre-LLM. Assumes a human reader who builds a mental model over time. Agents don't accumulate mental models — they read the schema fresh on every call.

**How v1 relates: cite as the bridge from human to agent.**

Bloch's "every API is a little language" is the load-bearing observation for v1: agents read the language *as a prompt*, not as documentation. Every field name, every description, every type, every enum value is consumed as natural language. This single insight motivates several heuristics (H1, H3, H9). Cite Bloch as the originator and explain how the LLM consumer makes the observation more literal than he could have known in 2006.

"When in doubt, leave it out" + OpenAI's empirical "<100 tools, <20 args" gives v1 a concrete granularity heuristic.

---

## Microsoft HAX (Amershi et al. 2019)

**What it covers:** 18 Human-AI Interaction guidelines, empirically validated against 20 AI products. Packaged as toolkit — guidelines + workbook + playbook + design-pattern library.

**What it misses for agents-as-consumer:** Different problem. HAX is *agents acting on humans* (chatbot UX, recommender behavior, trust calibration). v1 is *humans designing surfaces for agents to consume*.

**How v1 relates: borrow the artifact pipeline, distinguish the subject.**

The packaging strategy — heuristics → workbook → playbook → patterns — is the publication template v1 should plan toward. Amershi's work earned credibility partly because they published not just rules but the means to apply them.

State the distinction up front so readers don't conflate: "HAX guides how agents act on humans; this guides how humans build surfaces for agents to consume. Both are needed."

---

## Anthropic — *Writing Effective Tools for AI Agents* (2025)

**What it covers:** Eight principles. Choose tools thoughtfully. Namespace for clarity. Return meaningful context. Optimize for tokens. Prompt-engineer descriptions. Prototype. Evaluate. Collaborate-with-agents. Frames tools as "a contract between deterministic systems and non-deterministic agents."

**What it misses for agents-as-consumer at platform scale:** Tool-author-side and generic. Doesn't address persona conditioning (small agent vs orchestrator vs embedded model). Doesn't address cross-surface unification (API + CLI + MCP). Doesn't address governance / trust as a usability dimension. Doesn't address "what's a breaking change for an agent" (RM territory).

**How v1 relates: explicit extension, with credit.**

Anthropic's 8 principles are the most direct competitor. v1 should:

1. Acknowledge them as the closest contemporary
2. Map H1–H10 to the 8 principles (most overlap; Anthropic's "namespace" ≈ H9, "tokens" ≈ H7, "meaningful context" ≈ H3, etc.)
3. State the additions: persona conditioning, cross-surface unification (API + CLI + MCP, not just MCP), trust/governance as first-class dimension, alignment to platform-parity strategy (CPTO frame)
4. Be explicit that v1 is platform-side and persona-aware, while Anthropic's is tool-author-side and generic

---

## SWE-agent / Agent-Computer Interfaces (Yang et al. 2024)

**What it covers:** Coins "Agent-Computer Interface (ACI)" as a first-class designed surface. Demonstrates that interface design alone (not model improvements) materially lifts task success on SWE-bench. The most direct prior naming of "the surface an agent sees."

**What it misses for agents-as-consumer at platform scale:** Domain is software engineering on a code repo. Doesn't generalize to APIs / MCP / CLI surfaces. No heuristics, just "design matters" demonstrated empirically on one ACI.

**How v1 relates: adopt or evolve the term.**

Decision needed (Track 3): does v1 adopt "Agent-Computer Interface" as the umbrella term and offer heuristics for designing one? Or coin something more platform-shaped (e.g., "Agent Surface" / "Agent-API Interface")? Adopting "ACI" gives instant academic recognizability. Coining gives differentiation but starts the naming fight from zero.

Strong default: adopt "ACI" with credit to Yang et al., position v1 as "heuristics for designing a good ACI across API + CLI + MCP."

---

## HumanLayer 12-Factor Agents (Horthy)

**What it covers:** 12 production-reliability principles for agent applications. Own your prompts, own your context window, structured outputs, contact humans, errors as tokens, etc. Explicitly modeled on Heroku's 12-Factor App.

**What it misses for agents-as-consumer at platform scale:** Application level. Concerns the agent *application* developer's choices, not the *surface designer*'s choices. What does a good MCP / CLI / API look like is mostly out of scope.

**How v1 relates: position one layer below.**

Cleanest framing: 12-Factor Agents tells an agent-app builder how to build a reliable agent. v1 tells a platform team how to build surfaces those agents can succeed against. Different layers of the same stack.

Cite 12-Factor Agents as the application-level analogue, then state the layer below.

---

## DX Quality Rubric (HubSpot / `knowledge/design-quality/dx-quality-rubric.md`)

**What it covers:** 9-area rubric for human developer experience. Must / Should / Nice levels. Setup, Onboarding, Predictability, Error Recovery, Context, Scope, Iteration, Support, Delight.

**What it misses for agents:** Built around a human developer's journey. Onboarding is human-shaped (read docs, set up env). Delight is human-shaped (does this feel like a 10x experience). Doesn't address token economy, idempotency-as-affordance, or schema-as-prompt.

**How v1 relates: open question — Track 3 decision.**

Two coherent positions:

- **Two composable rubrics.** dx-quality measures human DX, v1 measures agent DX, they overlap in some areas (predictability, error recovery) and diverge in others. Use both, and the surface scores on two axes. Mark's working hypothesis.
- **One unified rubric with persona dimension.** Each heuristic has "for agent" and "for human" rows. More integrated but heavier; risks losing what's distinct about the agent surface.

Recommendation pending Track 3.

---

## UX Friction Rubric (HubSpot / `knowledge/design-quality/ux-friction-rubric.md`)

**What it covers:** F1–F5 friction types (Guessing, Hunting, Backtracking, Waiting, Repeating). Method-level: count observable behaviors. Used as Go / No-Go gate.

**What it misses for agents:** F1 Guessing and F2 Hunting map cleanly to agent surfaces (= H1 Discoverability + H3 Self-describing). F3 Backtracking maps to H4 Errors + H6 Idempotency. F4 Waiting and F5 Repeating need re-framing for agents.

**How v1 relates: complementary measurement method, not a competitor.**

The friction rubric is a *measurement method* (count observable friction moments), not a heuristic set. v1 heuristics define what good looks like; a friction-counting method could measure adherence. Long-term: a v2 scorecard that runs friction counts against agent traces is an empirical operationalization play.

No conflict. The friction rubric is for a different layer of the evaluation stack.

---

## Heroku 12-Factor App (the original)

**What it covers:** 12 reliability principles for cloud-native apps (codebase, dependencies, config, backing services, build/release/run, processes, port binding, concurrency, disposability, dev/prod parity, logs, admin processes).

**What it misses for agents:** Pre-LLM. Concerns deployment shape, not interface shape.

**How v1 relates: distant ancestor; cite for naming-pattern lineage.**

The 12-Factor naming pattern has spread (12-Factor Agents, 15-Factor App, etc.). v1 should NOT call itself "N-Factor anything" — that name space is overcrowded and the parallel doesn't add explanatory value.

---

## OWASP LLM Top 10 + Willison's "Lethal Trifecta"

**What it covers:** Security taxonomy for LLM applications. Prompt injection, insecure output handling, training data poisoning, model DoS, supply chain, sensitive information disclosure, etc. Willison's lethal trifecta: private data + untrusted content + external comms = exfiltration risk.

**What it misses for agents-as-consumer:** Security frame, not usability frame. Doesn't position trust boundaries as part of "what makes a surface easy to use safely."

**How v1 relates: integrate as a usability dimension.**

The unique angle (vs Anthropic, vs 12-Factor Agents): v1 treats trust / governance / audit as a *usability dimension*, not as security overlaid on top. H8 (Scoped intent) and H10 (Observability and audit) already gesture at this. The lethal-trifecta frame should be cited as the source of "what kinds of surfaces have inherent trust-boundary friction."

This is one of the differentiators the framework can claim cleanly: nobody else has integrated the security work into the usability frame.

---

## Existing benchmarks (BFCL, τ-bench, ToolBench, MCP-Universe, etc.)

**What they cover:** Capability measurement. Did the agent produce a valid call. Did the agent succeed end-to-end. Multi-turn reliability (`pass^k`).

**What they miss:** "Usability" of the surface, separate from the model's capability. Most benchmarks confound surface design with model capability — a model scoring 40% on MCP-Universe could be limited by the model OR by the surface design of the MCP servers tested.

**How v1 relates: empirical hooks for v2 scorecard.**

Each heuristic should declare which benchmark or measurement could operationalize it:

- H1, H9 → MetaTool, ToolTweak resistance
- H2 → BFCL AST-correctness, StableToolBench drift
- H3 → token efficiency on τ-bench at fixed pass rate
- H4 → recovery rate after error injection
- H5 → multi-step success on τ²-bench / MCP-Bench
- H6 → idempotency declaration coverage (can be statically measured)
- H7 → tokens per successful call (BFCL token-efficiency variants)
- H8 → scope-mismatch error rate
- H9 → tool-selection accuracy (MetaTool)
- H10 → not yet operationalized — open question

A separate doc later (call it `scorecard-v0.md`) should pull this together. Out of scope for Phase 1 setup.

---

## Position summary — one-line each

For quick reference and for the v1 introduction:

- **Nielsen 1994** → mirror the format and the derivation method
- **Cognitive Dimensions / Clarke** → academic lineage; borrow dimensional thinking
- **Bloch** → bridge insight ("every API is a little language")
- **HAX** → packaging template (heuristics → workbook → playbook → patterns)
- **Anthropic *Writing Tools*** → most direct competitor; extend with persona, cross-surface, governance
- **SWE-agent / ACI** → adopt the term; provide the heuristics for designing one
- **12-Factor Agents** → application-level analogue; v1 is one layer below
- **DX Quality Rubric** → human DX companion; compose with v1 (two rubrics) or unify (Track 3 decision)
- **UX Friction Rubric** → measurement method, not competitor
- **OWASP / Lethal Trifecta** → trust as a usability dimension; differentiator
- **Benchmarks** → empirical hooks for the v2 scorecard
