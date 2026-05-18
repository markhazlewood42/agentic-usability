# Ecosystem All-Hands — Agentic Usability Thread (2026-05-07)

Mark posted about agentic usability research in the Eco all-hands chat. Got significant engagement and surfaced several parallel workstreams.

## People and active work

### Edward Langan
- **Position:** His research shows APIs good for humans are good for agents and vice versa. Design principles overlap: response size limits, discoverability of valid properties, etc. But not completely 1:1 — sandboxes and idempotency matter more for agents.
- **Artifact:** Epic Week talk "Making Our Public APIs Work for Humans AND Agents" — https://learnathubspot.plusplus.app/events/14a236e8-f4da-4169-9821-301e156e8ad0_making-our-public-apis-work-for-humans-and-agents/
- **Relevance:** Most directly aligned to the heuristics work. His "same but different" framing is a key tension to address.

### Rachel Kellam (API Foundations)
- **Workstream 1:** Evaluations for how well an agent can navigate the HubSpot API
- **Workstream 2:** Automations to flag APIs out of compliance with the API Design Guide (planned for next sprint as of 2026-05-07)
- **Relevance:** The agentic usability heuristics could be the criteria these evaluations measure against.

### Matt Rollender
- **Built:** Internal agent for Breeze Apps that debugs how well the agent can use developer docs + CLI
- **Relevance:** Direct empirical tool. Could serve as a test harness for validating heuristics.
- **Action needed:** Get the link he offered to share.

### Roger Brown
- **Doing:** Synthetic user testing — agents with different model/harness combos use the product, then patterns across sessions inform prioritization
- **Also:** Automating all CUJs (Critical User Journeys) via CLI so users can augment productivity with agent support
- **Relevance:** Empirical validation methodology. His model/harness matrix approach maps to pass^k reliability testing.

### SJ Morris (DevRel)
- **Offered:** List of agent builders Mark could talk to for research
- **Action needed:** Follow up if she hasn't DM'd yet.

### Chirag Chadha
- **Referenced:** Harness research ("if you're not the agent, you're the harness") — https://www.youtube.com/watch?v=Xxuxg8PcBvc
- **Relevance:** Scoping question for the framework: heuristics evaluate the surface, but the harness is the other half of agent success.

### Hina Shah (UXR)
- **Exploring:** "Synthetic Users" concept for product feedback within UXR
- **Has:** A thinking group, invited people to join
- **Relevance:** Complementary methodology. Synthetic users test the product; agentic usability tests the API/CLI/MCP surface.

### Sejal Parikh
- **Pushback on Ed:** "Agents will code on the fly to solve the problem by stitching API calls together, humans won't." Consumption patterns diverge even if design principles overlap.
- **Relevance:** Validates that agent-specific heuristics (not just human DX heuristics) are needed.

### Casey Collins
- **Noted:** "I think Aviator is our harness" — positions Aviator as HubSpot's agent orchestration layer
- **Relevance:** Harness vs. surface boundary question for heuristic scoping.

### Jon McLaren
- **Mentioned:** Enterpret MCP as a tool for model-based product feedback

## Key tensions

1. **Same vs. different needs** — Ed says mostly the same; Sejal says consumption patterns diverge. The framework needs to address both the overlap and the gaps.
2. **Harness vs. surface** — Heuristics evaluate the surface (API/CLI/MCP). The harness (Aviator, agent frameworks) is the other half. Worth scoping explicitly in the framework.
3. **Multiple evaluation approaches forming independently** — Rachel (compliance automation), Roger (synthetic testing), Matt R (agent debugging), Hina (synthetic users). The heuristics/scorecard could unify the criteria.
