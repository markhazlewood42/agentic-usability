---
name: agentic-usability-prior-art
description: Annotated bibliography of academic and industry sources informing the agent usability heuristics framework. Organized by tradition, with top-10 reading order at top.
type: project
---

# Prior Art — Agent Usability

Annotated bibliography for the v1 framework. Captured 2026-05-05 from a literature and industry scan run as Phase 1 setup. Goal: every heuristic in v1 should be traceable to (a) a precursor it extends, (b) a contemporary it distinguishes from, or (c) an empirical hook (benchmark or measurement) that operationalizes it.

If a source isn't here, the framework probably shouldn't cite it.

---

## Top 10 — read these first

In priority order. Reading time given is rough.

1. **Yang et al. (2024). SWE-agent: Agent-Computer Interfaces Enable Automated Software Engineering.** NeurIPS 2024. arXiv:2405.15793. ~45 min. *The closest existing analogue to the framework. Coins "Agent-Computer Interface (ACI)" as a designed surface and shows interface design alone (not model improvements) materially lifts task success. v1 either adopts ACI or evolves it with credit.* https://arxiv.org/abs/2405.15793
2. **Anthropic. *Writing Effective Tools for AI Agents.*** ~30 min. *The most direct competing artifact. Eight principles (choose tools thoughtfully, namespace, return meaningful context, optimize for tokens, prompt-engineer descriptions, prototype, evaluate, collaborate-with-agents). v1 must position relative to this — extension, not duplication.* https://www.anthropic.com/engineering/writing-tools-for-agents
3. **Yao et al. (2024). τ-bench: A Benchmark for Tool-Agent-User Interaction in Real-World Domains.** Sierra. arXiv:2406.12045. ~40 min. *Introduces `pass^k` (consistency over multiple trials). Shows GPT-4o-class agents succeed <50% on retail, with `pass^8` <25% — the empirical separation of capability from reliability that gives "usability" its hook.* https://arxiv.org/abs/2406.12045
4. **Nielsen, J. (1994). Enhancing the Explanatory Power of Usability Heuristics.** CHI '94. doi:10.1145/191666.191729. ~25 min. *Read for the *derivation method*, not the 10 themselves. Factor analysis of 249 problems is the credibility template. v1's ambition is "Nielsen for agents," so we need to honor the method.*
5. **Clarke, S. (2004). Describing and Measuring API Usability with the Cognitive Dimensions.** PPIG / MS Research. ~30 min. *The human-API precursor. Adapts Green/Petre's Cognitive Dimensions to APIs and runs persona-driven studies. Position agent usability as the parallel research program for agents-as-consumer.* https://www.cl.cam.ac.uk/~afb21/CognitiveDimensions/workshop2005/Clarke_position_paper.pdf
6. **Bloch, J. (2006). How to Design a Good API and Why It Matters.** OOPSLA / Google Research. ~45 min talk. *Source of "When in doubt, leave it out," "Names matter," "Every API is a little language." The bridge from human to agent — agents read the language as a prompt, not as documentation.* https://research.google/pubs/how-to-design-a-good-api-and-why-it-matters/
7. **Anthropic. *Code Execution with MCP: Building More Efficient AI Agents.*** ~25 min. *Argues for presenting MCP servers as code APIs, loading only needed tools at runtime, processing data in execution sandbox. Strongest current vendor argument that agent usability is a system architecture choice, not just tool design.* https://www.anthropic.com/engineering/code-execution-with-mcp
8. **Horthy, D. / HumanLayer. *12-Factor Agents.*** GitHub repo, ~10k stars. ~30 min. *Closest "Heroku 12-Factor analogue" published. Application-level reliability principles (own your prompts, own your context window, errors as tokens, etc.). v1 is one layer below — the surface itself. Cite to position.* https://github.com/humanlayer/12-factor-agents
9. **Luo et al. (2025). MCP-Universe: Benchmarking LLMs with Real-World MCP Servers.** arXiv:2508.14704. ~25 min. *First benchmark targeted at MCP itself. Frontier models score 33–44%. Useful for v2 scorecard.* https://arxiv.org/abs/2508.14704
10. **Willison, S. *The Lethal Trifecta for AI Agents.*** ~10 min. *Private data + untrusted content + external comms = exfiltration risk. The most-cited security frame. Directly relevant to H8 / H10 and to HubSpot CPTO's "trusted by default" commitment.* https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/

---

## Academic literature

### Heuristic-evaluation lineage (the methodological backbone)

- **Nielsen, J. (1994). Enhancing the Explanatory Power of Usability Heuristics.** CHI '94, pp. 152–158. doi:10.1145/191666.191729. The 10 heuristics most people quote came from a factor analysis of 249 usability problems. The method, not the list, is the durable artifact. https://www.nngroup.com/articles/ten-usability-heuristics/
- **Nielsen, J. & Molich, R. (1990). Heuristic Evaluation of User Interfaces.** CHI '90. The original paper introducing heuristic evaluation as a discount usability method. Worth citing when arguing why a heuristic-based framework is the right shape.

### Cognitive Dimensions of Notations / API usability

- **Green, T.R.G. & Petre, M. (1996). Usability Analysis of Visual Programming Environments: A "Cognitive Dimensions" Framework.** Journal of Visual Languages & Computing 7(2). The foundational CD paper — 14 dimensions for evaluating notational/programming systems. Many translate cleanly to agent surfaces (viscosity → cost of wrong tool call; hidden dependencies → side effects not surfaced in schema; role-expressiveness → does the tool name reveal its role).
- **Blackwell, A. & Green, T.R.G. (2003). Notational Systems — the Cognitive Dimensions of Notations Framework.** In *HCI Models, Theories, and Frameworks* (Carroll, ed.). The mature exposition. https://en.wikipedia.org/wiki/Cognitive_dimensions_of_notations
- **Clarke, S. (2004). Describing and Measuring API Usability with the Cognitive Dimensions.** PPIG. Adapts CDs specifically to APIs and runs persona-driven studies at MS. Direct intellectual ancestor.
- **Stylos, J. & Myers, B. (2008). The Implications of Method Placement on API Learnability.** FSE '08. CMU. Empirical study showing where you put a method on a class affects discoverability. Direct H1 precursor.
- **Robillard, M. (2009). What Makes APIs Hard to Learn? Answers from Developers.** IEEE Software 26(6). Empirical study of 440 MS devs. Five factors for documentation: intent, examples, scenario-matching, penetrability, format — all map to agent needs.
- **Robillard, M. & DeLine, R. (2010). A Field Study of API Learning Obstacles.** Empirical Software Engineering 16(6). The follow-up study. https://link.springer.com/article/10.1007/s10664-010-9150-8
- **Piccioni, M., Furia, C.A., Meyer, B. (2013). An Empirical Study of API Usability.** ESEM. Token-based observation method; finding-of-record on naming and discoverability. https://se.inf.ethz.ch/~meyer/publications/empirical/API_usability.pdf

### Human-AI interaction guidelines

- **Amershi, S. et al. (2019). Guidelines for Human-AI Interaction.** CHI 2019. MS Research. The 18 HAI guidelines, validated against 20 AI products. Note: human-facing (agent-as-producer of UX). The structural method (guidelines → workbook → playbook → patterns) is exactly the artifact pipeline a v1 framework could mirror. https://www.microsoft.com/en-us/research/publication/guidelines-for-human-ai-interaction/
- **Microsoft HAX Toolkit.** https://www.microsoft.com/en-us/haxtoolkit/. The packaging of the 2019 work. Studied as a publishing template, not as content.

### Agent benchmarks (the empirical hook for v2 scorecard)

- **Patil, S. et al. (2024–25). The Berkeley Function Calling Leaderboard (BFCL).** ICML 2025. AST-based correctness; v3 adds multi-turn; v4 adds holistic agentic eval. The canonical "did the model produce a valid call." https://gorilla.cs.berkeley.edu/leaderboard.html
- **Yao et al. (2024). τ-bench / τ²-bench.** arXiv:2406.12045. `pass^k` reliability metric. The closest to a usability benchmark.
- **Qin, Y. et al. (2023). ToolLLM: Facilitating LLMs to Master 16000+ Real-World APIs.** ICLR 2024 spotlight. arXiv:2307.16789. First-at-scale REST-API tool-use dataset.
- **Li, M. et al. (2023). API-Bank.** arXiv:2304.08244. Earliest runnable benchmark; structures plan / retrieve / call.
- **Huang, Y. et al. (2023). MetaTool: Whether and Which to Use.** arXiv:2310.03128. Operationalizes tool selection as measurable behavior. Direct empirical hook for H9.
- **Liu, X. et al. (2023). AgentBench: Evaluating LLMs as Agents.** ICLR 2024. 8 environments. https://openreview.net/forum?id=zAdUB0aCTQ
- **Mialon, G. et al. (2023). GAIA: A Benchmark for General AI Assistants.** Three difficulty tiers; structure for thinking about task complexity gradients.
- **Jimenez, C. et al. (2023). SWE-bench: Can LM Resolve Real-World GitHub Issues?** Code-using agents; deterministic pass/fail.
- **Yang, J. et al. (2024). SWE-agent: Agent-Computer Interfaces Enable Automated Software Engineering.** NeurIPS 2024. arXiv:2405.15793. Coins "ACI."
- **Guo, Z. et al. (2024). StableToolBench.** arXiv:2403.07714. Surfaces the "evaluation is unstable when APIs change" problem. Relevant to H2 and to any heuristic about determinism.
- **Luo et al. (2025). MCP-Universe.** arXiv:2508.14704. First MCP-specific benchmark.
- **Wang et al. (2025). MCP-Bench.** arXiv:2508.20453. Complementary to MCP-Universe; emphasizes cross-tool coordination.
- **NexusRaven Function Calling Benchmark.** Single/parallel/nested calls.

### Critical view on benchmarks

- **Establishing Best Practices for Building Rigorous Agentic Benchmarks (2025).** arXiv:2507.02825. Eight prominent benchmarks shown gameable to near-perfect without solving tasks. Cite as guard against over-reifying scores.
- **Beyond Accuracy: A Multi-Dimensional Framework for Evaluating Enterprise Agentic AI Systems (2025).** arXiv:2511.14136. Argues capability ≠ usability for enterprise agents. Direct ally for the framework's positioning.

### Tool description and selection — empirical findings

- **ToolTweak: An Attack on Tool Selection in LLM-based Agents (2025).** arXiv:2510.02554. Empirically shows tool *names* have stronger influence on selection than descriptions. Foundation for H9.
- **Learning to Rewrite Tool Descriptions for Reliable LLM-Agent Tool Use (2026).** arXiv:2602.20426. Description revision substantially improves selection/execution accuracy. Frames tool description as a tunable artifact.

### Trust and security

- **OWASP LLM Top 10 (2025).** LLM01 Prompt Injection. https://genai.owasp.org/llmrisk/llm01-prompt-injection/
- **Log-To-Leak: Prompt Injection Attacks on Tool-Using LLM Agents via MCP.** OpenReview. Directly relevant to a "trust boundaries" heuristic.

---

## Industry / vendor frameworks

### Anthropic

- ***Writing Effective Tools for AI Agents.*** Already linked above. The 8 principles. https://www.anthropic.com/engineering/writing-tools-for-agents
- ***Code Execution with MCP: Building More Efficient AI Agents.*** Already linked. https://www.anthropic.com/engineering/code-execution-with-mcp
- ***Building Effective Agents.*** Distinction between workflows and agents. https://www.anthropic.com/research/building-effective-agents
- ***Building Agents with the Claude Agent SDK.*** Practical reference for agent-loop design.
- **Model Context Protocol Specification.** Authoritative protocol surface. Resources vs tools vs prompts, sampling, capability negotiation. https://modelcontextprotocol.io/specification

### OpenAI

- ***Function Calling Guide.*** "Fewer than ~100 tools and <20 args per tool considered in-distribution." The empirical "where reliability falls off" boundary. https://developers.openai.com/api/docs/guides/function-calling
- ***o3/o4-mini Function Calling Guide.*** Reasoning-model specifics. https://cookbook.openai.com/examples/o-series/o3o4-mini_prompting_guide
- ***A Practical Guide to Building Agents.*** Vendor-side practitioner guide.

### Google

- **PAIR — *People + AI Guidebook.*** Human-AI patterns; structural template for an external-facing version. https://pair.withgoogle.com/guidebook/
- **ADK — *Agent Development Kit Documentation.*** Includes OpenAPI-tools docs that grapple with schema-quality issues for agents. https://google.github.io/adk-docs/

### Microsoft

- **HAX Toolkit.** Already cited above as packaging template.

### Practitioner / community

- **Horthy, D. *12-Factor Agents.*** Already in top 10. https://github.com/humanlayer/12-factor-agents
- **Heroku. *The Twelve-Factor App.*** The original being analogized. https://12factor.net/
- **LangChain. *Context Engineering for Agents.*** Four-bucket framework: write, select, compress, isolate. https://blog.langchain.com/context-engineering-for-agents/
- **Hamel Husain. *Your AI Product Needs Evals* + AI Evals course + forthcoming O'Reilly book *Evals for AI Engineers*.** The practitioner authority on operationalizing quality. https://hamel.dev/blog/posts/evals/
- **Eugene Yan. *Patterns for Building LLM-Based Systems & Products.*** Seven patterns. https://eugeneyan.com/writing/llm-patterns/
- **Simon Willison. llm-tool-use tag.** Ongoing commentary. https://simonwillison.net/tags/llm-tool-use/
- **Karpathy, A.** "Context window as RAM" framing. Cite as the OS analogy.
- **DSPy (Stanford NLP).** Programming-not-prompting framework. https://dspy.ai/

### REST / API design canon (the human-DX baseline to compare against)

- **Zalando RESTful API Guidelines.** Concrete enterprise REST guidance. https://opensource.zalando.com/restful-api-guidelines/
- **Stripe — payment API design retrospective.** https://stripe.dev/blog/payment-api-design
- **Auchenberg, K. *Insights from Building Stripe's Developer Platform.*** Human-DX excellence baseline. https://kenneth.io/post/insights-from-building-stripes-developer-platform-and-api-developer-experience-part-1
- **Heroku platform API design guide** (referenced widely).

### Protocol-shape arguments

- **Apollo / Paso / Runyard / OpenReplay blog series. *MCP vs REST vs GraphQL for AI Agents.*** Useful for the "what protocol shape favors agent usability" debate (introspection, dynamic discovery, persistent connection, schema-as-contract).

### Developer-experience surveys (background data)

- **Stack Overflow Developer Survey 2025.** "46% of developers distrust AI tools vs 33% trust." https://survey.stackoverflow.co/2025/
- **JetBrains Developer Ecosystem 2025.** https://devecosystem-2025.jetbrains.com/
- **GitHub Octoverse 2025.** https://octoverse.github.com/

---

## Internal HubSpot context (separate from external prior art)

These aren't "prior art" in the academic sense, but they're load-bearing for the framework's HubSpot framing.

- `knowledge/strategy-docs/cpto-vision-2026-05.md` — the CPTO trigger; full API parity, two-layer architecture, trusted by default
- `knowledge/design-quality/dx-quality-rubric.md` — the human DX rubric this framework composes with (or replaces, depending on Track 3 decision)
- `knowledge/design-quality/ux-friction-rubric.md` — the friction-counting companion rubric
- `knowledge/personas/developer-journey-lexicon.md` — 8-phase developer journey with agent consumer track for phases 3–8
- `real-projects/api-ia-study/` — downstream consumer; H1, H2, H9 land hardest there
- `real-projects/release-management/` — downstream consumer; H6, H2 are load-bearing for "what's a breaking change for an agent"

---

## Reading order beyond top 10

If/when there's time:

11. **Robillard (2009) + Robillard & DeLine (2010)** — the API-learning-obstacles pair
12. **Stylos & Myers (2008)** — method-placement-and-learnability
13. **MCP Specification** — protocol-level grounding for any heuristic touching transport
14. **OpenAI Function Calling Guide** — for the empirical "<100 tools, <20 args" floor
15. **Hamel — *Your AI Product Needs Evals*** — for operationalization
16. **Establishing Best Practices for Rigorous Agentic Benchmarks (arXiv:2507.02825)** — reality check after benchmarks
17. **ToolTweak + Learning to Rewrite Tool Descriptions** — empirical foundation for H9
18. **OWASP LLM Top 10 + Log-To-Leak** — security lens
19. **Cognitive Dimensions of Notations (Blackwell & Green, 2003)** — the dimensional framing
20. **Bloch (2006) full talk video** — for "every API is a little language"
