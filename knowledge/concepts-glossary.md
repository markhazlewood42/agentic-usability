---
name: agentic-usability-concepts-glossary
description: Load-bearing concepts the v1 agent usability framework must engage with. Each entry is a paragraph definition with the source and how it lands in v1.
type: project
---

# Concepts Glossary

The vocabulary v1 must speak fluently. Each concept here either originates outside the framework (cited) or is a synthesis the framework will own.

If a concept doesn't earn a paragraph here, v1 doesn't need to introduce it.

---

## Agent-Computer Interface (ACI)

The designed surface an agent reads, calls, and interprets — distinct from the underlying capability. Coined by Yang et al. (2024, SWE-agent paper) for the interface a coding agent sees over a repo. The empirical claim that earned the term its weight: interface design alone (independent of the underlying model) materially lifts task success on SWE-bench. v1 generalizes ACI from "code interface" to "any agent-facing surface" — API, CLI, MCP server, generated docs. Heuristics are *for designing a good ACI*. Source: Yang et al., NeurIPS 2024, arXiv:2405.15793.

## Schema-as-prompt

Every field name, every description, every type, every enum value in a tool schema is consumed by an LLM agent as natural language input. There is no separation between "API contract" and "instructions to the model" — the contract *is* a prompt fragment. Anthropic's *Writing Tools* makes this most explicit ("prompt-engineer your descriptions"). v1's H3 (Self-describing surface) and H9 (Naming as IA) follow directly. Implication: documentation-as-afterthought is a v0 design failure for agents in a way it isn't for humans. Source: Anthropic *Writing Effective Tools for AI Agents*; conceptual ancestor in Bloch's "every API is a little language."

## Tool name as primary affordance

Empirical finding from ToolTweak (arXiv:2510.02554) and *Learning to Rewrite Tool Descriptions* (arXiv:2602.20426): the *name* of a tool has stronger influence on agent selection than the description. Verb choice, namespacing prefix, and parameter ordering all behave as affordances in the perceptual sense. Direct foundation for H9. Practical implication: rename-not-redocument is the higher-leverage intervention for selection problems.

## Context economy / token cost as a usability dimension

Every token in a tool spec, every token in a returned payload, competes with every other token for finite context window space. Density of useful information per token is a usability axis the way visual hierarchy is for human UIs. LangChain's "context engineering for agents" framework (write / select / compress / isolate) is the cleanest current articulation. Karpathy's "context window as RAM" is the OS analogy. v1's H7 sits here. Source: Anthropic *Code Execution with MCP*; LangChain *Context Engineering for Agents*; OpenAI guidance on tool surface size.

## Deterministic vs stochastic side effects

Agents cannot reliably reason about non-idempotent effects because their reasoning is itself non-deterministic — combining the two compounds rather than cancels uncertainty. A surface that fails to declare which operations are safe to retry forces the agent to either over-retry (duplicates, billing problems) or under-retry (give up). Maps to Cognitive Dimensions' "viscosity" (cost to recover from a wrong move) and "hidden dependencies" (effects not visible in the call site). v1's H6 sits here. The novelty for agents (vs humans): humans accumulate institutional knowledge about which calls are safe to retry; agents have to learn that fact every session unless the surface declares it.

## Error message as second-chance prompt

Errors aren't (only) observability — they're another input to the agent's next decision. A structured error response that names the problem, lists valid options, and points to the relevant docs section enables ~100% recovery on syntactic and semantic errors in agent traces (per Anthropic guidance and recovery research). A 500 with a string body forces the agent to retry blindly or hallucinate a fix. v1's H4 sits here. The reframe is the load-bearing insight: errors are interface, not just diagnostics.

## Tool selection vs tool use

Two separable failure modes, distinguished empirically by MetaTool (arXiv:2310.03128). *Selection* failure: agent picks the wrong tool. *Use* failure: agent picks the right tool but calls it wrong. Heuristics that improve selection (clear names, namespacing, intent-aligned categorization) don't necessarily improve use (typed parameters, predictable shapes, machine-actionable errors). v1's two-half cut (reason-about / use-effectively) follows this distinction.

## `pass^k` reliability

τ-bench's metric (Yao et al. 2024, Sierra). Probability the agent succeeds *consistently across k independent attempts* on the same task. Frontier agents on τ-bench: `pass^1` ≈ 50%, `pass^8` < 25%. The gap between `pass^1` and `pass^k` is the reliability tax — much of it attributable to interface ambiguity rather than capability ceiling. This is the empirical hook that makes "agent usability" measurable separately from "agent capability." Source: τ-bench, arXiv:2406.12045.

## Lethal trifecta

Simon Willison's frame: an agent system that simultaneously has access to *private data*, *untrusted content*, and *external communication* is at structural risk of data exfiltration via prompt injection. The trifecta isn't a vulnerability; it's a structural property that makes any specific vulnerability catastrophic. v1's H8 (Scoped intent) and H10 (Observability and audit) gesture at the design countermeasures — narrow scopes, audit trails, human-in-the-loop confirmations. Useful framing because it makes "security" a *usability* concern: a surface that can't be safely used is unusable. Source: simonwillison.net (also referenced in OWASP LLM Top 10).

## Discovery model

How an agent learns what's available on a surface. Three rough patterns: *static schema* (read once at load, e.g. OpenAPI), *dynamic introspection* (query at runtime, e.g. MCP `tools/list`), *runtime negotiation* (capability handshake, e.g. MCP capability negotiation). Each has different costs and different failure modes. REST tends static, GraphQL tends introspective, MCP supports both. v1's H1 (Discoverability) varies in shape across these. Source: MCP specification; Apollo / Paso *MCP vs REST vs GraphQL for AI agents* writeups.

## Persona conditioning (for agents)

Agent surfaces aren't one-size-fits-all because agents aren't one-size-fits-all. A small specialized agent has different needs from a generalist orchestrator from a low-latency embedded model. The same heuristic can pull in different directions for different agent personas (e.g., terse responses help small agents, verbose responses help orchestrators reason). Steven Clarke's MS Research API-CD work showed this for human developer personas; v1 extends to agent personas. Direct connection to HubSpot's persona work (Gracie / Gabby / agent track) — the framework can carry the persona stance forward.

## Surface (vs capability)

The thing the agent reads. Distinct from what the platform can do. CRM v3 *can* fetch contacts; CRM v3's surface is a particular set of endpoints, parameter names, error shapes, and docs that may or may not make that capability accessible. Most benchmarks measure capability + surface tangled together. v1's reason for being: separate the two so surfaces can be evaluated and improved independent of the underlying systems. The "Beyond Accuracy" paper (arXiv:2511.14136) is one of the few academic articulations of this separation.

## Agent-as-consumer (vs agent-as-producer)

The framing distinction that separates v1 from Microsoft HAX. *Agent-as-producer:* agent generates output (text, recommendations, actions) consumed by humans. UX research field; HAX guidelines apply. *Agent-as-consumer:* agent reads an interface and acts through it. v1's domain. The two frames have different design questions and different failure modes; conflating them produces confused frameworks.

## Worked example

In framework-design: a single concrete surface (e.g., HubSpot CRM v3 Contacts) scored heuristic-by-heuristic with evidence and remediation suggestions. Worked examples ground heuristics in reality and protect against vague phrasing that survives because it can't be falsified. HAX's Workbook is built around worked examples. v1 will need 3–5 by the time it ships.

## Trust boundary

A point in a system where data or authority crosses from one trust domain to another. For agents: the boundaries between user, model, tools, external services, and untrusted content. Each boundary is a place where a surface can succeed or fail at making the boundary legible to the agent. v1's H8 and H10 are trust-boundary heuristics. The lethal trifecta frame names which trust-boundary configurations are inherently dangerous.

## Reliability tax

A coined v1 term (provisional). The amount of agent failure attributable to *interface* ambiguity rather than model capability. Operationalized roughly as `1 - pass^k / pass^1` — the larger the gap, the more reliability the surface is taxing. Useful for executive framing: "improving the surface reduces the reliability tax even when the model stays the same." Tentative; refine in v1.

---

## Concepts deliberately *not* in v1 (yet)

Listed so we don't accidentally pull them in:

- **Prompt engineering** — out of scope (CPTO vision and project README both rule this out). Surfaces, not prompts.
- **Agent runtime / scaffolding** — out of scope. v1 evaluates the surface, not the agent loop.
- **Multi-agent orchestration patterns** — adjacent but separate concern.
- **Model selection / capability tiers** — relevant only insofar as persona conditioning surfaces it.
- **Agent training / fine-tuning** — fully out of scope.

---

## Open vocabulary questions for v1

- Term for the framework itself: "Agent Usability Heuristics" / "ACI Heuristics" / "Agent Interface Design Principles" / something else? Decision pending Track 3.
- Term for "an evaluable surface": "ACI" (per SWE-agent), "agent surface," "agent-facing platform surface"? Pending Track 3.
- Whether to canonicalize "reliability tax" or leave it as an explanatory move. Pending v1.
