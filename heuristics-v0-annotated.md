---
name: agentic-usability-heuristics-v0-annotated
description: v0 heuristics annotated with prior art, distinct contribution, and empirical hooks. Pressure-tests v0 against the literature scan. Reading this lands the framework in the academic and industry context before v1 stabilizes.
type: project
---

# Agent Usability Heuristics — v0, Annotated

Companion to `heuristics-v0.md`. Each H1–H10 gets three appendices:

- **Prior art** — who said something close. Links into `knowledge/prior-art.md`.
- **Distinct contribution** — what v1 adds that isn't already in the literature.
- **Empirical hook** — which benchmark, study method, or measurement could operationalize this heuristic for a v2 scorecard.

Plus a "candidate v1 additions" section at the end (H11–H13) and discussion of latency / stateful interactions.

This is the working document for stabilizing v1. Read alongside `heuristics-v0.md` (the source rules), `knowledge/prior-art.md` (the bibliography), and `knowledge/frameworks-crosswalk.md` (the positioning).

---

## H1. Discoverability without trial-and-error

> *Rule:* The agent can find the right capability from a description of intent, not by trying things until something works.

**Prior art:**

- Nielsen 1994 Heuristic 1 (visibility of system status) and Heuristic 6 (recognition over recall) are the human-side ancestors — but they're about reacting to a present interface, not searching a capability space.
- **Stylos & Myers (2008)** *The Implications of Method Placement on API Learnability* (FSE '08) is the closest empirical precursor — placing a method on the right class affects discoverability empirically.
- Anthropic *Writing Effective Tools* — "choose tools thoughtfully" + namespacing guidance.
- MetaTool (arXiv:2310.03128) operationalizes tool selection as a measurable agent behavior.

**Distinct contribution:**

The agent-specific reframe: humans skim and recover visually, agents recover by retrying. Retry cost asymmetry (cheap for humans, expensive in tokens and side effects for agents) is what makes discoverability *more* load-bearing for agents than for humans, not just differently load-bearing. Existing API-usability work doesn't reckon with this asymmetry because human retry is essentially free.

**Empirical hook:**

- **MetaTool** "whether-and-which tool" accuracy on a HubSpot tool catalog
- **τ-bench `pass^k` reliability** — discoverability problems show up as pass-rate drop from `pass^1` to `pass^8`
- Tool-name / endpoint-name overlap measurements (string similarity, embedding clustering)
- Synthetic agent runs: given N realistic intents, what % land on the canonical tool first try?

---

## H2. Predictable shapes

> *Rule:* Inputs and outputs are typed, deterministic, and consistent. The same concept has the same shape across the surface.

**Prior art:**

- Bloch (2006) "Names matter" + "minimize mutability" — the API design canon.
- Cognitive Dimensions: *Consistency* and *Role-expressiveness*.
- BFCL (Berkeley Function Calling Leaderboard) operationalizes "did the model produce a syntactically and semantically valid call" — predictable shapes are what makes that achievable.
- StableToolBench (arXiv:2403.07714) directly studies shape drift over time.

**Distinct contribution:**

The "concept-with-the-same-shape across the surface" framing as a *cross-endpoint* property. Most prior work measures shape correctness per call. v1 names *cross-call shape consistency* as a distinct usability dimension because agents pattern-match across calls in a session — a `contact` that looks one way in a list response and another way in a get-by-id response is a hallucination magnet.

**Empirical hook:**

- BFCL AST-correctness on a HubSpot subset
- Schema-similarity score across endpoints returning the same conceptual type (e.g., does every endpoint that returns a Contact return the same shape?)
- Agent confusion rate after a shape change in a synthetic eval

---

## H3. Self-describing surface

> *Rule:* Every endpoint, tool, or command returns enough metadata to be understood without out-of-band documentation.

**Prior art:**

- Robillard (2009) *What Makes APIs Hard to Learn?* — five factors of documentation: intent, examples, scenario-matching, penetrability, format. All map to agent needs even though Robillard's subjects were humans.
- Anthropic *Writing Tools* — "return meaningful context" + "prompt-engineer descriptions."
- MCP specification — capability negotiation as a self-description mechanism.
- Bloch's "every API is a little language" framing.

**Distinct contribution:**

The schema-as-prompt insight (see `concepts-glossary.md`). For agents, there is no separation between "API contract" and "instructions to the model." Schema text is consumed as natural language. Documentation-as-afterthought is a v0 design failure for agents in a way it isn't for humans, who can fall back on tutorials, Stack Overflow, or asking a teammate. This makes the heuristic *load-bearing* for agents in a way it's only *helpful* for humans.

**Empirical hook:**

- Token-efficiency at fixed pass rate on τ-bench (more self-describing surfaces should reach the same pass rate with fewer tokens)
- Agent recovery rate when passed schema-only vs schema-plus-docs
- Description-quality eval: rewrite descriptions per "Learning to Rewrite Tool Descriptions" methodology, measure selection accuracy delta

---

## H4. Errors that are machine-actionable

> *Rule:* Errors are typed, coded, and include structured recovery information. Prose alone fails.

**Prior art:**

- Nielsen 1994 Heuristic 9 (help users recognize, diagnose, recover from errors) — direct human-side ancestor.
- Anthropic *Writing Tools* — explicit guidance on error responses as agent recovery surfaces.
- 12-Factor Agents: "errors as tokens" / "compact errors into context window."
- Robillard (2009) — error documentation is one of the five factors of API learnability.

**Distinct contribution:**

The "error as second-chance prompt" reframe (see `concepts-glossary.md`). Errors aren't just observability — they're another input to the agent's next decision. Existing error-design work treats error messages as user feedback; v1 treats them as part of the prompt the agent will read on the next turn. This shifts what counts as a good error: not "what went wrong," but "what would help the agent succeed on retry."

**Empirical hook:**

- Recovery rate after error injection (controlled fault: invalid arg, unauthorized, rate-limited) — measure % of agent traces that recover within N retries
- Error-content quality: does the error name the field, list valid values, point to docs? Static measurement.
- Tokens-to-recovery: how many tokens does an agent spend after an error before producing a successful retry?

---

## H5. Composability

> *Rule:* Outputs of one operation match inputs of related operations without translation.

**Prior art:**

- Bloch (2006) — API design canon on small surfaces that compose.
- Cognitive Dimensions: *Hidden dependencies* and *Viscosity* both touch on this.
- 12-Factor Agents — "structured outputs" as an enabling pattern.
- Anthropic *Code Execution with MCP* — the strongest current articulation of why agent composability matters at scale (agents writing code over tool outputs is more efficient than chaining direct calls).

**Distinct contribution:**

Cross-surface composability as a usability dimension, not just within a single API. v1's territory is API + CLI + MCP, and a key value claim is that composability across surfaces (CLI output flows into API input flows into MCP tool call) is a property of *coherent platform design*. No existing framework names cross-surface composability as a specific heuristic.

**Empirical hook:**

- Multi-step task success on τ²-bench / MCP-Bench
- "Translation overhead" measurement: in agent traces, count places where the agent reshapes data between calls (regex, key renaming, type coercion) — translation steps are the unit of composability failure
- Static analysis: what fraction of "outputs of X" are valid "inputs to Y" without translation?

---

## H6. Idempotency and safe retry

> *Rule:* Operations declare their idempotency. Agents can retry on failure without guessing at side effects.

**Prior art:**

- Cognitive Dimensions *Viscosity* — cost to recover from a wrong move. Direct ancestor.
- 12-Factor Agents — "trigger from anywhere, meet users where they are" assumes retry-safe operations.
- HTTP method semantics (RFC 9110) — GET / PUT / DELETE idempotency conventions; POST not.
- Stripe API design retrospective — idempotency keys as a first-class developer affordance.

**Distinct contribution:**

The agent-retry argument: agents retry *much more than humans do*, and they retry stochastically (the model's reasoning is itself non-deterministic). Combining stochastic retry with non-idempotent operations compounds rather than cancels uncertainty. Existing API design treats idempotency as a developer convenience; v1 frames it as a usability *necessity* for agent surfaces. The novel claim: idempotency declaration belongs in the schema (machine-readable), not just the docs.

**Empirical hook:**

- Static measurement: what % of write operations declare idempotency in machine-readable form (idempotency-key support, OpenAPI extension, MCP annotation)?
- Duplicate-side-effect rate in agent traces under controlled fault injection (force a 500 mid-write, count duplicates)
- Survey: ask agent builders how often they write retry wrappers for HubSpot APIs (revealed-preference signal)

---

## H7. Token economy

> *Rule:* Responses are lean by default and expandable on request. No mandatory bloat.

**Prior art:**

- Anthropic *Writing Tools* — "optimize for tokens" is one of the 8 principles.
- Anthropic *Code Execution with MCP* — token economy is the load-bearing argument for code-execution-over-MCP architectures.
- LangChain *Context Engineering for Agents* — write / select / compress / isolate.
- Karpathy's "context window as RAM."
- Cognitive Dimensions *Diffuseness/terseness* — somewhat related but framed for human readability, not context-window pressure.

**Distinct contribution:**

The "taxation by API design" reframe. Existing token-economy guidance treats verbosity as inefficiency; v1 frames mandatory bloat as a *usability tax* — the surface forcing the agent to spend context on data it didn't want is structurally analogous to a UI requiring a user to read a paragraph to find a button. This connects token economy to the broader heuristic frame.

**Empirical hook:**

- Tokens per successful call at fixed pass rate (BFCL token-efficiency variants exist for some models)
- Default-response size distribution per endpoint family
- "Useful tokens" ratio: in a returned payload, what fraction was used by the agent in its next decision (requires trace inspection)

---

## H8. Scoped intent

> *Rule:* Authorization is granular and intelligible up front. The agent knows what it's allowed to do before it tries.

**Prior art:**

- OAuth 2.0 / 2.1 scope design (RFC 6749 + downstream).
- OWASP LLM Top 10 — LLM06 Sensitive Information Disclosure, LLM08 Excessive Agency.
- Willison's lethal trifecta — argues for narrow scopes as a structural mitigation.
- Anthropic *Building Effective Agents* — workflows-vs-agents distinction implies scope visibility for agentic patterns.

**Distinct contribution:**

Frames scope as a *usability* dimension, not just a security one. Existing OAuth/IAM design treats scopes as a security primitive; v1 argues that *machine-readable scope visibility* (the agent can query its own granted scopes, not just discover them via 403s) is a usability precondition for any non-trivial agent. The novel claim: a 403 mid-flow is a usability failure, not just an auth failure.

**Empirical hook:**

- Scope-mismatch error rate per task type (% of agent traces that hit 403 mid-flow)
- Static measurement: does the surface expose granted scopes to the authenticated agent in machine-readable form?
- Scope-to-capability mapping coverage: are scope names self-describing relative to the capabilities they gate?

---

## H9. Naming as IA

> *Rule:* Names carry meaning. Similar things are named similarly. The lexicon agents see in tool/endpoint names matches the lexicon humans see in docs.

**Prior art:**

- **Bloch (2006) — "Names matter."** The original.
- Cognitive Dimensions *Role-expressiveness* — does the name reveal the role? Direct ancestor.
- **ToolTweak (arXiv:2510.02554)** — empirical proof that tool names dominate selection.
- **Learning to Rewrite Tool Descriptions (arXiv:2602.20426)** — empirical proof that description revision improves selection.
- Stylos & Myers (2008) on method placement — IA-as-affordance precursor.
- Anthropic *Writing Tools* — "namespace for clarity."

**Distinct contribution:**

Two moves. (1) Tying naming explicitly to information architecture, not just per-tool naming — categorization across the surface affects agent groupings. (2) Pulling in HubSpot's API IA Study work (`real-projects/api-ia-study/`) as the empirical companion. The framework's H9 is the territory the IA study is studying, and the IA study's findings will refine H9.

**Empirical hook:**

- ToolTweak resistance: replace tool names with generic identifiers, measure selection-accuracy delta
- Tool-selection accuracy on MetaTool with HubSpot's actual tool taxonomy
- Card-sort agreement (from API IA Study) between agent-derived groupings and human-developer groupings

---

## H10. Observability and audit

> *Rule:* Every action an agent takes is traceable, attributable, and visible to the customer in real time.

**Prior art:**

- OWASP LLM Top 10 — LLM08 Excessive Agency, LLM06 Sensitive Info Disclosure.
- Willison's lethal trifecta — argues for audit as a structural mitigation.
- 12-Factor Agents — "small focused agents" + "structured outputs" assume legible action streams.
- HAX guidelines — Guidelines 13 (efficient correction), 14 (scope agency), 15 (notify changes) — human-side ancestors.
- HubSpot CPTO vision: "trusted by default" commitment makes this load-bearing for the platform.

**Distinct contribution:**

Frames audit and observability as *usability* dimensions for the customer / admin persona, not just operational concerns. The novel claim: an agent surface where actions aren't legible to the human-in-the-loop is *unusable for trust* even if every individual call succeeds. This connects to the lethal-trifecta argument: surfaces that can't be audited can't be safely composed with private data and external comms.

**Empirical hook:**

- Static measurement: per-agent attribution coverage in audit logs (% of API actions that record agent identity distinctly from user identity)
- Customer-facing agent activity feed coverage: % of agent actions visible to portal admins in real time
- Time-to-detect for anomalous agent behavior (operational metric)

---

## Candidate v1 additions

These were flagged as gaps in `heuristics-v0.md`. Annotated below with prior art and tentative phrasing.

### H11. Versioning legibility

> *Provisional rule:* Surface versions and breaking changes are declared in machine-readable form. Agents can detect surface drift before it produces wrong behavior.

**Prior art:**

- Semantic Versioning (semver.org) — the developer canon.
- StableToolBench (arXiv:2403.07714) — empirically studies surface drift over time.
- Stripe API versioning model (date-pinned versions) — the human-DX gold standard.
- HubSpot Release Management work (`real-projects/release-management/`) — "what counts as a breaking change for an agent?" is an open thread Kevin Porter / RM is grappling with.

**Distinct contribution:**

The agent-specific reframe: humans pin versions and follow upgrade guides; agents discover drift through failure. Versioning legibility for agents requires that the surface declare its version *and* the agent can detect when its expectations no longer match. This is the bridge between v1 and the RM project's "agent-aware breaking change" question.

**Empirical hook:**

- Breaking-change detection rate in agent traces under simulated drift
- Static measurement: does the surface expose version + change-history in machine-readable form?

### H12. Documentation as agent-readable surface

> *Provisional rule:* Documentation is structured, addressable, and consumable by agents at runtime — not just as a human reference.

**Prior art:**

- Robillard (2009) five factors of API documentation — for humans.
- Anthropic *Writing Tools* — touches on this via "prompt-engineer descriptions."
- llms.txt / llms-full.txt convention emerging in 2024–25.
- MCP `prompts` resource type — documentation as a callable surface.

**Distinct contribution:**

H3 (Self-describing surface) covers what the surface itself returns. H12 covers the *broader* documentation corpus as a separate addressable surface — examples, recipes, conceptual docs. The novel claim: the existence of `llms.txt` and structured documentation as a runtime resource means docs-for-agents is a distinct design surface, not just human docs the agent reads.

**Empirical hook:**

- Agent recovery improvement when given access to indexed docs vs schema-only
- Coverage: % of API endpoints with worked examples in agent-readable form
- Doc-call frequency in real agent traces (how often do agents fetch docs at runtime?)

### H13. Trust boundaries are legible

> *Provisional rule:* Trust boundaries — between user data, untrusted content, and external comms — are visible to the agent and the human in the loop.

**Prior art:**

- Willison's lethal trifecta.
- OWASP LLM Top 10.
- Log-To-Leak (OpenReview) — empirical demonstration that MCP surfaces leak via prompt injection.
- Anthropic guidance on prompt injection and untrusted content handling.

**Distinct contribution:**

H8 covers scoped authority; H10 covers post-hoc audit. H13 covers the *design surface* for boundary legibility — does the agent know when it's about to mix tainted content with sensitive data, and does the human in the loop see boundary crossings? Currently this concern is treated as a security overlay; v1 integrates it as a usability dimension (matching the framework's positioning differentiator vs Anthropic / 12-Factor).

**Empirical hook:**

- Prompt-injection resistance under structured red-team scenarios
- Static measurement: does the surface tag "untrusted content" zones in returned data?
- Trifecta-detection coverage: can the agent identify when it's operating in a trifecta-risky configuration?

---

## Discussion: latency / stateful interactions

Both were flagged in `heuristics-v0.md` as candidates. Tentative position:

**Latency / throughput** — likely *not* a v1 heuristic. Performance is a quality concern; making it a usability heuristic dilutes the frame and overlaps with general API-quality guidance that already exists. *Exception:* a "predictable latency" property might be a sub-aspect of H2 (Predictable shapes) — agents pattern-match on latency too, and a surface that responds in 200ms most of the time and 30s sometimes confuses agent retry logic. Park as a sub-aspect of H2, not its own heuristic.

**Stateful interactions** — likely a v1 heuristic, possibly H14. Sessions, long-running tool calls, async work, MCP sampling. The current heuristics assume request/response. A surface with stateful interactions has different rules for H6 (idempotency on a session vs an operation), H4 (errors mid-stream), H7 (token cost over a session). The right move: draft H14 *Stateful interactions are explicit* in v1, with the rule that any state held by the surface is declared, queryable, and resettable.

---

## What this annotation revealed about v0

A few patterns surfaced from running every heuristic against the literature:

1. **H1, H4, H6, H7, H9 have strong empirical hooks.** These can be operationalized into a v2 scorecard relatively cleanly.
2. **H10 is the hardest to operationalize.** Trust and observability resist single-metric reduction. v2 scorecard work will need to think about this carefully.
3. **H5 (Composability) is the most underspecified.** "Without translation" is fuzzy. v1 should sharpen this — possibly into "the surface declares its composability properties (which outputs are valid inputs where) in machine-readable form."
4. **H8 + H13 overlap.** Track 3 should decide whether they merge or stay separate. Current preference: separate (H8 = "what am I allowed to do," H13 = "where are the boundaries between trust domains"), but this is a v1 call.
5. **H9 has the strongest competitive moat for v1.** Combination of Bloch + ToolTweak + the in-flight HubSpot API IA Study + Mark's HCI background gives v1 a defensible position on naming-as-IA that nobody else has assembled.

These are inputs to Track 3 and to the v0 → v1 stabilization pass.
