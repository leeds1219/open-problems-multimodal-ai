# Research Track — Memory-Bounded Consistency in Long-Horizon Multimodal Agents

*A narrowed direction into the broad map: [§7.3](../problems/07-agents-and-security.md) episodic memory & state tracking → [§8.3](../problems/08-data-efficiency-evaluation.md) long-context efficiency, with the evaluation domain drawing on [§5.5](../problems/05-generation-unified-models.md) (generative consistency), [§7.1–7.2](../problems/07-agents-and-security.md) (GUI/task agents), or [§4](../problems/04-video-temporal-streaming.md) (long video). Compiled 2026-07-09.*

## Thesis (one line)
Long-horizon multimodal agents behave **inconsistently** because they cannot keep consistency-critical state — tracked entities, prior decisions, constraints established many steps earlier — straight over a long horizon. Crucially (see the evidence check below), this is **not merely running out of room**: state fidelity degrades *within* the effective context, which is an order of magnitude smaller than the advertised window, so a bigger window does not fix it. **We want to measure that drift and then bound it with structured, selective memory at a fixed token budget.**

## Why this is important and general
- **Important — it's a core bottleneck, not a niche bug.** Per-step model quality keeps improving, yet multi-step tasks still collapse because the system cannot carry its own state forward. Reliable long-horizon autonomy is gated on this. Fixing it is what lets agents stay coherent over hours/days instead of minutes.
- **General — modality- and domain-agnostic.** The same failure recurs everywhere: coding agents forgetting earlier design decisions, research agents losing prior findings, computer-use agents violating a constraint set early, video assistants losing entity state, creative agents drifting in style. One mechanism underlies all of it: *history > context → lossy compression → lost state*. A contribution here transfers across domains.
- **General ≠ vague.** The deliverable is a **domain-general capability** — measure, then bound, consistency drift under context pressure. It gets *instantiated* on one domain to run experiments, but the claim (and the memory mechanism) is domain-independent. The domain is an experimental choice, not the scope of the contribution.

## Is this actually open? (adversarial evidence check, 2026-07-09)
Stress-tested with three independent literature investigations — one tasked with **refuting** the problem (arguing it's already solved), one defending it, and one on the crux counter-argument (*does long-context scaling dissolve it?*). All three converged: **the problem is real and open — but the mechanism is sharper than "runs out of room."**

**What IS solved — do *not* claim these:**
- **Raw capacity.** 1M–10M-token windows exist (Gemini 2.5 Pro, GPT-4.1, Claude Sonnet 4 = 1M; Llama 4 Scout claims 10M).
- **Single-needle retrieval.** Near-saturated (Gemini 1.5 Pro >99.7% needle-in-a-haystack to 1M). Every serious paper dismisses NIAH as superficial.
- **Mechanical persistence across overflow.** Shipped as engineering (Anthropic context-editing / memory tool: 100-turn workflows completed with ~84% fewer tokens; LangGraph checkpointers).

**What is NOT solved — the opening:**
- **Effective context ≪ advertised.** Shortcut-free tests collapse an order of magnitude early: **NoLiMa** ✅ [arXiv:2502.05167] (ICML 2025) — 11/13 models fall below 50% of baseline at 32K; effective lengths ~2K–16K vs. 1M advertised. **RULER** ✅ [arXiv:2404.06654] concurs (~half of models fail their claimed 32K).
- **Failures aren't triggered by overflow.** **Vending-Bench** ✅ [arXiv:2502.15840] (>20M tokens/run): *"no clear correlation between failures and the point at which the context window becomes full."* → bigger windows provably don't fix it; agents drift, loop, and "melt down" mid-window.
- **Memory frameworks win on cost, not fidelity.** **Mem0** ✅ [arXiv:2504.19413]: full-context (72.9%) still **beats** the memory framework (66.9%) on LoCoMo *accuracy* — frameworks cut tokens/latency but nobody matches brute force on correctness.
- **Purpose-built memory benchmarks sit far below humans.** **LongMemEval** ✅ [arXiv:2410.10813] (ICLR 2025, ~30 pt drop under sustained interaction); **LoCoMo** ✅ [arXiv:2402.17753] (GPT-4 ~32 F1 vs. human ~88).
- **Multimodal is worse.** **MET-Bench** ✅ [arXiv:2502.10886]: entity tracking drops up to **−30 pts** text→image — your exact "tracked entities" claim. **MM-NIAH** ✅ [arXiv:2406.07230]: "long-context capability is NOT retained in MLLMs" (text needle 89% vs. image needle 28%). Visual state is token-heavy (~1 hr video ≈ 1M tokens) and position bias is amplified ~2–4× over text.

**The sharpened problem (this is what reframes the thesis):** the crux is **selective, consistency-preserving state retention** — deciding *which* entities/decisions/constraints to keep vs. compress — and it fails *within* the effective context, not just at overflow. Stronger than the original "context-limit" framing: it cannot be scaled away by a larger window.

*Caveat: this check leaned on verified 2024–2025 primary sources. Several 2026-dated benchmarks/surveys the investigations surfaced (e.g. HORIZON, LongDS-Bench, cross-session-consistency surveys) point the same way but were **not** fetched to primary-source depth — treat as ⚠️ until confirmed.*

## Problem
Any agent running over a long horizon accumulates a growing trace of observations, decisions, and generated/produced state. Within a modest number of steps this trace outgrows the context window, so the system falls back on lossy summarization or truncation — and later behavior silently violates earlier state. "Consistency" here is general: honoring an entity's tracked state across occlusion/time, adhering to a constraint set long ago, or keeping a produced artifact coherent. The failure is under-measured and there is no principled memory design for *what* to keep.

## Research questions
- **RQ1 (diagnosis):** How does consistency / state-retention degrade as a function of context pressure — horizon length, and truncation vs. summarization vs. retrieval strategy — at a fixed token budget?
- **RQ2 (mechanism):** Can a structured, retrievable **agent-state memory** (entities, decisions, constraints, reference crops) bound the drift better than simply scaling context or using generic RAG?

## Scope
- **In:** the *orchestrator's* memory — what state to store, how to compress **multimodal (esp. visual)** state without losing decision-critical detail, and when to retrieve / re-inject it.
- **Boundary (out):** raising the base model's context length itself; generator-side identity conditioning (IP-Adapter/StoryDiffusion-style) — treated as a fixed downstream tool.
- **Evaluation domain (pick one concrete testbed — the contribution is domain-general):**
  - **(a) Creative / generative** — cross-shot identity/style/motif consistency (media art is one instance).
  - **(b) GUI / computer-use** — honoring goals & constraints set early in a long task (ties to OSWorld-style horizons).
  - **(c) Long-video assistant** — entity-state accuracy across a long stream.

## Closest prior work — to extend, not repeat
- **AMA-Bench** — evaluating long-horizon memory for agentic applications ✅ [arXiv:2602.22769](https://arxiv.org/abs/2602.22769). *Closest benchmark; check what it does and does **not** measure re: consistency vs. recall.*
- **M3-Agent** — multimodal agent with long-term memory ✅ [arXiv:2508.09736](https://arxiv.org/abs/2508.09736). *Gap:* memory tuned for QA/recall, not for enforcing behavioral/creative **consistency**.
- **MovieChat** — memory for long video ✅ [arXiv:2307.16449](https://arxiv.org/abs/2307.16449). *Gap:* consumes a stream; doesn't act over a long horizon.
- **MemGPT / Generative Agents** — memory hierarchies. *Gap:* text-only; no visual state.
- ***Lost in the Middle*** (Liu et al., TACL 2023) — retrieval decay within long context → motivates why bigger context alone won't fix it.
- **Long-horizon reliability:** *Measuring Long Horizon Execution* ✅ [arXiv:2509.09677](https://arxiv.org/abs/2509.09677); *Beyond pass@1* ✅ [arXiv:2603.29231](https://arxiv.org/abs/2603.29231) — why horizon length itself degrades reliability.
- **Judge dependency:** a *reliable* consistency judge is itself open — see [§7.4](../problems/07-agents-and-security.md) / [§5.4](../problems/05-generation-unified-models.md).
- **Benchmarks to measure against / position within:** LongMemEval, LoCoMo (text memory); MET-Bench, MM-NIAH (multimodal state/entity tracking); Vending-Bench (long-horizon agent drift); NoLiMa, RULER (effective-context degradation). IDs in the evidence check above.

## Hypothesis
Drift is driven less by raw context length than by *which* state items survive compression. A small, structured, consistency-targeted memory (re-injected per step) holds consistency roughly flat as the horizon grows, where truncation/summary baselines degrade — at equal token budget.

## First entry point (one instantiation — keeps the claim general)
*This is **an** experiment, not the scope. The contribution is the general capability above; this is the cheapest way to get first evidence. The evaluation domain stays open (see Open decisions).*

- **Setup:** an agent runs an N-step task in the chosen domain, calling fixed downstream tools; state/constraints are established early.
- **Independent variables:** horizon length N; memory strategy ∈ {full-context, sliding-window truncation, LLM summary, generic RAG, *structured agent memory*}; **token budget held fixed**.
- **Metrics (drift vs. step index):** domain-appropriate consistency — identity/style/palette distance (creative), goal/constraint-adherence + task success (GUI), entity-state accuracy (video) — plus an MLLM-judge score **with an inter-rater / perturbation reliability check** on that judge.
- **Models:** an API frontier model as orchestrator + one open downstream tool → feasible on modest compute.
- **Deliverable:** a benchmark + drift curves = evidence the phenomenon is systematic and the testbed for RQ2.

## What a result looks like
Drift-vs-context curves showing (1) the failure is systematic, (2) bigger context / naive summary don't fix it at fixed budget, and (3) structured memory bounds it. Publishable as a diagnostic on its own; the memory mechanism is the phase-2 conference paper.

## Risks & mitigations
- *"Just use a bigger context / RAG."* → the diagnostic is built to show these underperform at fixed budget (**Lost-in-the-Middle** predicts it).
- *Judge unreliability contaminates the metric.* → make the reliability check a first-class result.
- *Testbed too niche.* → define the task on a public tool/environment so results reproduce beyond any one company's stack.

## Stretch / phase 2
The structured-memory mechanism: learned what-to-store policies, visual-state compression that preserves decision-critical tokens, and consistency-aware retrieval whose objective is tied directly to the drift metric above.

## Open decisions
1. **Evaluation domain** — (a) creative, (b) GUI/computer-use, or (c) long-video? Drives the metrics and the closest baselines.
2. **Where the drift originates** — orchestrator memory (this track) vs. generator/tool conditioning (out of scope).
