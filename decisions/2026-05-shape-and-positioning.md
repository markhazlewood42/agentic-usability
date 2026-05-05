# Decision: Framework shape and positioning for v1

## Status
- Status: proposed
- Date: 2026-05-05
- Decider: Mark (with input from Phase 1 literature scan)

## Context

`personal-projects/agentic-usability/` v0 (drafted 2026-05-04) is a flat 10-item heuristics list. Before v1 stabilizes, several structural questions need explicit answers — because the answers determine what v1 looks like, what readout artifact eventually ships, and how the framework cites and is cited.

Phase 1 produced three knowledge artifacts that this decision doc draws from:

- `knowledge/prior-art.md` — annotated bibliography
- `knowledge/frameworks-crosswalk.md` — positioning relative to existing frameworks
- `heuristics-v0-annotated.md` — v0 pressure-tested against the literature

This is a Phase 1 (exploration) decision doc — the questions are open. When we resolve them, restructure to Phase 2 (recommendation).

---

## Questions to resolve

Five questions, each with options pre-staged from the literature scan. Mark's input drives the answers.

### Q1. What shape is the v1 artifact?

Three coherent shapes from prior art:

- **A. Flat heuristics list (Nielsen 1994 shape).** Numbered list, each with rule + rationale + failure mode + example. Easy to cite. Low ceremony. v0's current shape.
- **B. Heuristics + dimensions (Nielsen + Cognitive Dimensions hybrid).** Heuristics for "what to evaluate," dimensions for "how to think about tradeoffs." Richer, more academic.
- **C. Heuristics + workbook + playbook + patterns (HAX shape).** Full publishing pipeline. Highest credibility ceiling. Highest ongoing maintenance cost.

**What it depends on:**

- Audience (Q3) — internal designers might be served by A; external publication likely benefits from C
- Time horizon — A is shippable in weeks, C is months
- Whether v1 is the artifact or v1 is the *first step* toward a future artifact

### Q2. One rubric or two? (Composition with `dx-quality-rubric.md`)

Two coherent positions from `frameworks-crosswalk.md`:

- **A. Two composable rubrics.** dx-quality-rubric measures human DX. v1 measures agent DX. Surfaces score on two axes. Mark's working hypothesis.
- **B. One unified rubric with persona dimension.** Each heuristic has "for agent" and "for human" rows. More integrated, heavier authoring, risks losing what's distinct.

**What it depends on:**

- Whether the human and agent stories ever genuinely conflict on a heuristic (if they always co-vary, B; if they sometimes pull opposite, A)
- Operating model: who runs the rubric? (Designers running both is heavier than two specialized rubrics)

### Q3. Audience — internal first, external first, or both?

Three options:

- **A. HubSpot-internal first.** API designers, platform PMs, API Council. Shapes language and examples to HubSpot context. Use as input to API IA Study, RM, and any API/CLI/MCP review.
- **B. External first.** Position as the "agent usability" framework in the wider community. Shapes language to be vendor-neutral and academically citable.
- **C. Both, sequenced.** Internal first for credibility-via-use, external second once it's been pressure-tested.

**What it depends on:**

- Promo narrative timing (if Mark is positioning for principal promo, external publication is a stronger artifact)
- API IA Study and RM timelines — they need v1 *now* in some form; external publication can wait
- Resource cost: internal-first is cheaper to maintain

### Q4. Term — adopt "ACI" / extend it / coin new?

Three options:

- **A. Adopt SWE-agent's "Agent-Computer Interface."** Inherit recognizability. Heuristics are "for designing a good ACI."
- **B. Extend ACI with a platform-specific term.** "Agent-Platform Interface (API)" — pun intended. Or "Agent Surface."
- **C. Coin something new.** Gives differentiation but starts the naming fight from zero.

**Default if no strong argument otherwise:** A. Adoption-with-credit is the cheap, low-risk move.

### Q5. Derivation method — Nielsen factor analysis or vendor-style opinion?

Two options:

- **A. Nielsen-style factor analysis.** Collect N agent failures (target N≥100, ideally 250 like Nielsen 1994), code them, factor-analyze. High credibility, slow, requires data collection. The credibility play for external publication.
- **B. Vendor-style opinion + worked examples.** Heuristics derived from synthesis of literature + author experience, validated by 3–5 worked examples on real surfaces. Fast, lower-credibility, but matches what Anthropic / OpenAI / 12-Factor Agents publish.

**What it depends on:**

- Q3 (audience) — external publication leans toward A; internal-only can survive on B
- Time and access — does Mark have access to enough agent failure traces (HubSpot or external) to run a factor analysis?
- Resource availability for empirical work

---

## Tentative recommendations (subject to brainstorming)

Pre-staged from the literature scan, *not* decided. Mark drives.

- **Q1 (shape):** Lean toward B (heuristics + dimensions). A is too thin for promo / publication; C is too heavy for current resourcing.
- **Q2 (rubrics):** Lean toward A (two composable). The literature suggests agent and human DX genuinely diverge on some heuristics (token economy, idempotency declaration), so unifying loses signal.
- **Q3 (audience):** Lean toward C (both, sequenced — internal first). API IA Study + RM need v1 in months, not quarters; external publication can build on validated internal use.
- **Q4 (term):** Lean toward A (adopt ACI with credit).
- **Q5 (derivation):** Lean toward B for v1, with explicit roadmap to A for v2 if external publication becomes a goal. Honest framing in v1: "this is opinion-plus-literature; a future version will run a Nielsen-style derivation."

---

## Options considered (full)

### Option A: Heuristics-only, internal-first, ACI-adoption, opinion-derived

- Description: v0 → v1 = stabilize 10–13 heuristics, ship internally to API IA Study and RM, defer publication.
- Pros: fastest path to v1. Lowest risk. Unblocks API IA Study and RM now.
- Cons: not a strong promo / external artifact on its own. May calcify before external pressure-testing.

### Option B: Heuristics + dimensions, both audiences sequenced, ACI-adoption, opinion-derived for v1 (factor analysis for v2)

- Description: v1 = 10–13 heuristics + 4–6 cross-cutting dimensions, internal use first, then external readout / paper, with v2 derivation work as a separate phase.
- Pros: balanced risk. Useful internally now. Has external publication path. Dimensions add academic depth without HAX-level ceremony.
- Cons: more authoring work than A. Harder to scope.

### Option C: Full HAX-style pipeline, external-first, coin new term, factor-analysis-derived

- Description: heuristics + workbook + playbook + pattern library, derived from a 250-failure factor analysis, positioned externally first with HubSpot context as a worked example.
- Pros: highest credibility ceiling. Strongest promo artifact. Likely citable.
- Cons: 6+ months of work. Requires data collection. Internal consumers (API IA Study, RM) wait.

---

## Consequences

What follows from this decision:

- v1 file structure (single heuristics doc vs heuristics + dimensions doc vs full toolkit directory)
- Authoring effort estimate (weeks vs months vs quarters)
- Cross-references in API IA Study and RM (citing internal v1 vs citing external paper)
- Promo narrative shape (internal influence vs external publication)
- Whether `/heuristic-eval` skill (Track 4) ships now or after derivation

What's out of scope of this decision:

- Whether to produce `/heuristic-eval` skill at all (yes, in any of the options)
- Whether to do worked examples on HubSpot surfaces (yes, in any option — only the count and depth changes)
- Specific heuristic wording for v1 (separate decision after this one)

Risks to watch:

- **Option B drift to Option C.** Heuristics + dimensions can creep into "heuristics + dimensions + patterns + workbook" if not held tight.
- **Option A under-investment** if Mark needs an external artifact for promo.
- **Q5 honesty.** If v1 is opinion-derived, say so plainly — don't oversell as research.

## Next steps

After this doc resolves to Phase 2 (recommendation):

- Write v1 heuristics text per chosen shape (sharpen wording, add dimensions if Q1=B, drop H10/H13 ambiguity if needed)
- First worked example on a HubSpot surface (recommend CRM v3 Contacts because it's central to API IA Study)
- Build `/heuristic-eval` skill in `skills/`
- Optional: kick off failure-trace collection if Q5 ladders toward A

## Related

- `heuristics-v0-annotated.md` — v0 pressure-tested against literature
- `knowledge/prior-art.md` — annotated bibliography
- `knowledge/frameworks-crosswalk.md` — positioning relative to existing frameworks
- `knowledge/concepts-glossary.md` — vocabulary v1 must speak
- `real-projects/api-ia-study/status.md` — downstream consumer
- `real-projects/release-management/status.md` — downstream consumer
- `knowledge/strategy-docs/cpto-vision-2026-05.md` — strategic trigger
