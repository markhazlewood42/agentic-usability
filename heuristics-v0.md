---
name: agentic-usability-heuristics-v0
description: First-cut 10 heuristics for evaluating agent usability of APIs, CLIs, and MCP tool surfaces. Working draft to iterate on. Use when reviewing a developer platform surface for agent-readiness or framing what agent usability means.
type: project
---

# Agent Usability Heuristics — v0

**Status:** Working draft, 2026-05-04. Iterate freely. Heuristic numbers are sticky (don't renumber), but wording and ordering of body text can change.

These are intended as a parallel to Nielsen's 10 for human GUI usability — applied to surfaces an AI agent reads, calls, and interprets. Each heuristic includes a one-line rule, why it matters for agents specifically, a "fails this when" example, and where it lands hardest in HubSpot's surfaces.

---

## H1. Discoverability without trial-and-error

**Rule:** The agent can find the right capability from a description of intent, not by trying things until something works.

**Why for agents:** Humans skim and recover from wrong choices visually. Agents recover by retrying, which is expensive in tokens and dangerous in side effects. The cost of "wrong call, then try the next one" is much higher for agents than for humans.

**Fails this when:** Two endpoints with overlapping responsibilities, no clear signal which is canonical. Names don't match the agent's natural framing of intent. No "search for capability" affordance in the surface.

**HubSpot hot spots:** REST IA (multiple ways to fetch contacts), MCP tool catalog (which tool for which intent).

---

## H2. Predictable shapes

**Rule:** Inputs and outputs are typed, deterministic, and consistent. The same concept has the same shape across the surface.

**Why for agents:** Agents pattern-match on shapes more than humans do. A `contact` that looks one way in one endpoint and another way three calls later is a hallucination magnet.

**Fails this when:** Pagination differs between resources. Date formats vary. ID fields named inconsistently (`id` vs `objectId` vs `vid`). Required fields turn optional in some contexts without clear schema signal.

**HubSpot hot spots:** v1/v2/v3 API drift, CRM object normalization across endpoints.

---

## H3. Self-describing surface

**Rule:** Every endpoint, tool, or command returns enough metadata to be understood without out-of-band documentation.

**Why for agents:** Agents have no prior context unless it was injected into the prompt. A self-describing surface lowers the cost of cold-start use. Schemas, examples, annotations, and inline help are first-class.

**Fails this when:** OpenAPI spec exists but isn't loaded/loadable by the agent. MCP tool descriptions are one-liners. Error responses don't say which field caused the problem.

**HubSpot hot spots:** MCP tool descriptions, OpenAPI quality, CLI `--help` output for `hs` commands.

---

## H4. Errors that are machine-actionable

**Rule:** Errors are typed, coded, and include structured recovery information. Prose alone fails.

**Why for agents:** Humans read "Sorry, that didn't work — try again later" and infer intent. Agents read it literally and either retry blindly or hallucinate a fix. Agents need an error shape they can branch on.

**Fails this when:** All errors are 500s with a string body. Codes are inconsistent across services. No structured "what to do next" hints. Validation errors don't identify the offending field.

**HubSpot hot spots:** Error normalization across CRM v3, public API legacy endpoints, CLI error output.

---

## H5. Composability

**Rule:** Outputs of one operation match inputs of related operations without translation.

**Why for agents:** Multi-step agent workflows chain calls. Every translation step (IDs, formats, references) is a place to fail. A composable surface lets the agent move data between calls without reshaping.

**Fails this when:** "List" returns IDs in one shape, "Get" wants a different shape. Pagination tokens aren't reusable. Reference fields aren't directly callable.

**HubSpot hot spots:** Cross-object associations, bulk vs single operations, search returning shapes that differ from get-by-id.

---

## H6. Idempotency and safe retry

**Rule:** Operations declare their idempotency. Agents can retry on failure without guessing at side effects.

**Why for agents:** Agents retry. A lot. If the platform doesn't tell the agent which operations are safe to retry, the agent will either over-retry (duplicates) or under-retry (give up too easily). Both are bad.

**Fails this when:** No idempotency keys on writes. POST that's actually upsert, not documented as such. DELETE that isn't idempotent (returns 404 on second call).

**HubSpot hot spots:** Webhook delivery, batch object writes, deploy operations.

---

## H7. Token economy

**Rule:** Responses are lean by default and expandable on request. No mandatory bloat.

**Why for agents:** Every byte of response goes into a context window the agent has to pay for. A 50-field object when the agent needed two fields is taxation by API design.

**Fails this when:** No field selection (`?fields=name,email`). Default response includes deeply nested associations. Pagination defaults to absurdly large pages.

**HubSpot hot spots:** CRM object reads (default association expansion), search results, "everything I might want" payloads.

---

## H8. Scoped intent

**Rule:** Authorization is granular and intelligible up front. The agent knows what it's allowed to do before it tries.

**Why for agents:** A 403 mid-flow is a debugging nightmare for an agent. Agents need to know the boundaries of their authority at the start, in machine-readable form, and react to scope changes cleanly.

**Fails this when:** Scopes aren't visible to the agent. Granted scopes can't be queried. Scope names don't match capability names. Failure mode is generic 403 with no hint about which scope is missing.

**HubSpot hot spots:** OAuth scope granularity, scope-to-endpoint mapping, agent runtime scope visibility.

---

## H9. Naming as IA

**Rule:** Names carry meaning. Similar things are named similarly. The lexicon agents see in tool/endpoint names matches the lexicon humans see in docs.

**Why for agents:** Agents lean on names harder than humans do because they often pick a tool from a name alone. Naming is the first and most-used affordance. A bad name is a wrong call.

**Fails this when:** Verbs and nouns drift across the surface. The same concept has three names. Internal jargon leaks into public names. Categorization in the IA doesn't match how agents would group capabilities by intent.

**HubSpot hot spots:** This is exactly the API IA Study's territory. See `real-projects/api-ia-study/`.

---

## H10. Observability and audit

**Rule:** Every action an agent takes is traceable, attributable, and visible to the customer in real time.

**Why for agents:** Agents operate at scale. One mistake propagates fast. Customers (and admins) need to see what an agent did, when, and on whose authority — both for trust and for recovery. This is the design surface behind "trusted by default."

**Fails this when:** Agent actions aren't distinguishable from user actions in audit logs. No per-agent attribution. Customer-facing visibility into agent activity is absent or generic.

**HubSpot hot spots:** Activity feeds for portal admins, agent attribution in audit logs, governance UX for "what is this agent doing in my portal."

---

## How H1–H10 cluster

The project's core research question splits cleanly into two halves: "easier or harder to **reason about**" and "easier or harder to **use effectively**." That gives us the primary organizing cut for the heuristics.

### Primary cut — reason-about vs use-effectively

**Reason-about heuristics** (predict the surface before calling it):

- H1 Discoverability — can the agent locate the right capability?
- H2 Predictable shapes — does the surface behave consistently enough to anticipate?
- H3 Self-describing — does the surface tell the agent what it does and what it needs?
- H9 Naming as IA — do names carry enough meaning to pick from?

**Use-effectively heuristics** (act on the surface and recover well):

- H4 Errors — when something fails, can the agent branch and try the right thing?
- H5 Composability — can outputs flow into downstream calls without translation?
- H6 Idempotency — can the agent retry safely?
- H7 Token economy — can the agent operate without bloat eating its context?
- H8 Scoped intent — can the agent stay inside its authority by design?
- H10 Observability — when the agent acts, is the action legible to humans after the fact?

This split matters because failures cluster differently across the two halves. Reason-about failures show up as the agent picking the wrong call or hallucinating shape. Use-effectively failures show up as runtime errors, retry storms, scope mismatches, and trust gaps. Different evaluation methods will likely apply to each half.

### Secondary cut — by quality dimension

Same heuristics, different lens. Useful when triaging which heuristics matter most for a specific surface:

- **Predictability** (H1, H2, H9) — can the agent get to the right call cleanly?
- **Legibility** (H3, H4, H7) — can the agent read what comes back without bloat or guessing?
- **Safety** (H6, H8, H10) — can the agent operate without blowing things up, and can humans see what it did?
- **Composability** (H5) — can multi-step work chain cleanly?

Both clusterings may evolve. They're sense-making aids, not category systems.

## What's not in here yet

- Latency/throughput — does perf belong as a usability heuristic for agents? Probably yes for some surfaces (interactive agents); maybe no for batch.
- Stateful interactions — sessions, long-running tool calls, async work. Most heuristics here assume request/response.
- Versioning — overlaps with predictability and idempotency, but probably deserves its own heuristic given how much agent breakage will come from version drift.
- Documentation as a surface — docs are a thing agents read too. Worth a heuristic on docs-for-agents specifically.

## Sources and influences (to be expanded)

- Nielsen's 10 usability heuristics (1994) — the obvious parallel
- Cooper / Goodwin on goal-directed design — applied to agent goals
- HubSpot API standards working group output (Rachel Kellam et al.)
- MCP design notes from Anthropic
- Internal HubSpot CHIRP design conventions
- Field experience with Claude Code and agent tool use, 2025–2026
