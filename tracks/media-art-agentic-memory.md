# Research Track — Memory-Bounded Consistency in Media-Art Agents

*A narrowed direction into the broad map: [§7.3](../problems/07-agents-and-security.md) episodic memory → [§5.5](../problems/05-generation-unified-models.md) generative consistency → [§8.3](../problems/08-data-efficiency-evaluation.md) long-context efficiency. Compiled 2026-07-09.*

## Thesis (one line)
A media-art "director" agent produces **inconsistent** creative output because its multimodal history exceeds the context window; when history is compressed or truncated, the state that guarantees consistency (character identity, palette, motifs, prior narrative decisions) is lost. **We want to measure that drift and then bound it with a structured creative memory.**

## Problem
An agent orchestrating a multi-shot creative artifact accumulates a long trace of instructions, decisions, and generated assets. This trace outgrows the context window within a handful of shots, so the system falls back on lossy compression, summarization, or truncation — and downstream generations silently violate earlier constraints. The failure is **subjective and multi-faceted** (identity + style + narrative continuity), so there is no off-the-shelf metric for it and no principled memory design for what to keep.

## Research questions
- **RQ1 (diagnosis):** How does creative consistency degrade as a function of context pressure (history length, truncation/compression strategy, number of shots)? Is the degradation smooth or a cliff?
- **RQ2 (mechanism):** Can a structured, retrievable **creative-state memory** (an "art bible" of entities, style tokens, reference crops, decisions) bound the drift better than context-window scaling or generic RAG, at fixed token budget?

## Scope
- **In:** the *orchestrator's* memory (mechanism A) — what creative state to store, how to compress **visual** state without losing identity-critical detail, and when to re-inject it.
- **Boundary (out, for now):** generator-side identity/style conditioning (mechanism B — IP-Adapter/StoryDiffusion-style). Treated as a fixed downstream tool so the contribution is isolated to memory.

## Closest prior work — to extend, not repeat
- **M3-Agent** — multimodal agent with long-term memory ✅ [arXiv:2508.09736](https://arxiv.org/abs/2508.09736). *Gap:* memory built for QA/reasoning recall, not for enforcing **creative consistency** across generation.
- **MovieChat** — memory for long video ✅ [arXiv:2307.16449](https://arxiv.org/abs/2307.16449). *Gap:* consumes video, doesn't *produce* a coherent multi-shot artifact.
- **MemGPT / Generative Agents** — text memory hierarchies. *Gap:* text-only; no visual state.
- ***Lost in the Middle*** (Liu et al., 2023) — retrieval decay within long context. Motivates *why* bigger context alone won't fix it.
- **VBench** consistency dimensions ✅ [arXiv:2311.17982](https://arxiv.org/abs/2311.17982); **StoryDiffusion** — consistency *in generation*. *Gap:* per-clip, not agent-level cross-shot state.
- **Reward/eval dependency:** a *reliable* consistency judge is itself open — see [§7.4](../problems/07-agents-and-security.md) / [§5.4](../problems/05-generation-unified-models.md).

## Hypothesis
Consistency drift is driven less by raw context length than by *which* creative-state items survive compression. A small, structured, consistency-targeted memory (re-injected per shot) will hold consistency roughly flat as shot count grows, where summarization/truncation baselines degrade.

## First experiment (the diagnostic — a self-contained first paper)
- **Task:** generate N-shot creative sequences (character/scene/style specified up front) with an agent that calls a fixed image/video generator.
- **Independent variables:** shot count N; memory strategy ∈ {full-context, sliding-window truncation, LLM summary, generic RAG, *structured creative memory*}; token budget held fixed.
- **Metrics (consistency drift vs. shot index):** identity similarity (face/object embedding), style/palette distance, motif/entity recurrence, plus an MLLM-judge narrative-continuity score — **with an inter-rater/perturbation reliability check on that judge** (ties back to the reward problem).
- **Models:** an API frontier model as orchestrator + one open generator, so this is feasible on modest compute.
- **Deliverable:** a benchmark + the drift curves = evidence the phenomenon is real and the testbed for RQ2.

## What a result looks like
A drift-vs-context curve showing (1) the failure is systematic, (2) bigger context / naive summary don't solve it, and (3) structured creative memory bounds it at fixed budget. That's a publishable diagnostic on its own; the memory method is the natural phase-2 conference paper.

## Risks & mitigations
- *"Just use a bigger context / RAG."* → the diagnostic is designed to show these underperform at fixed budget; **Lost-in-the-Middle** predicts they will.
- *Judge unreliability contaminates the metric.* → include the reliability check as a first-class result, not an afterthought.
- *Company-specific setup limits generality.* → define tasks on a public generator so results reproduce outside DirectorLabs' stack.

## Stretch / phase 2
The structured-memory mechanism itself: learned what-to-store policies, visual-state compression that preserves identity tokens, and consistency-aware retrieval whose objective is tied directly to the drift metric above.
