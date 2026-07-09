# 6. Reasoning & Test-Time Scaling

How do we get vision-language models to reason *with* images rather than about them — and to scale inference-time compute without the cost of dense visual tokens exploding?

---

## 6.1 Multimodal CoT That Actually Uses the Image (grounding drift)
**Why it's hard:** As reasoning chains lengthen, models lean harder on language priors and hallucinate visual details, "drifting" into text-only reasoning — ablations show CoT-prompted MLLMs answer nearly as well with the image *removed*. Text is a lossy bottleneck for continuous perceptual content.

**Key papers:** *Multimodal Chain-of-Thought Reasoning in Language Models* ✅ [arXiv:2302.00923](https://arxiv.org/abs/2302.00923); *Take-along Visual Conditioning (TVC)* ✅ [arXiv:2503.13360](https://arxiv.org/abs/2503.13360); *Thinking with Images* (survey) ✅ [arXiv:2506.23918](https://arxiv.org/abs/2506.23918).
**Benchmarks:** MMMU-Pro vision-only setting ✅ [arXiv:2409.02813](https://arxiv.org/abs/2409.02813); BLINK ✅ [arXiv:2404.12390](https://arxiv.org/abs/2404.12390).
**Directions:** no-image ablations as default eval; interleaved vision-text traces; periodic visual re-conditioning; attention-to-image regularizers.

---

## 6.2 Test-Time Scaling Under Dense Visual Tokens
**Why it's hard:** o1/R1-style long reasoning, self-correction, and tree search (MCTS) each re-encode hundreds–thousands of visual tokens per node/rollout, so search compute multiplies against an already-expensive visual context — far worse than text-only inference scaling.

**Key papers:** *Mulberry* (collective MCTS) ✅ [arXiv:2412.18319](https://arxiv.org/abs/2412.18319); *LLaVA-CoT* (stage-wise retracing search) ✅ [arXiv:2411.10440](https://arxiv.org/abs/2411.10440); *Vision-R1* (RL for MLLM reasoning) ✅ [arXiv:2503.06749](https://arxiv.org/abs/2503.06749).
**Benchmarks:** MathVista ✅ [2310.02255](https://arxiv.org/abs/2310.02255); MATH-Vision ✅ [2402.14804](https://arxiv.org/abs/2402.14804).
**Directions:** visual-KV caching across search nodes; per-node token pruning / adaptive resolution; budget-aware verifier-guided search.

---

## 6.3 Thinking *With* Images: Tool-use, Cropping/Zoom, Sketching
**Why it's hard:** Fine-grained and high-res tasks require actively re-examining the image (zoom, crop, draw auxiliary lines), but pure-text CoT can't express these operations; integrating grounded visual actions into the reasoning loop — and learning *when* to invoke them — is unsolved.

**Key papers:** *V\*: Guided Visual Search* (SEAL, V\*Bench) ✅ [arXiv:2312.14135](https://arxiv.org/abs/2312.14135); *Visual Sketchpad* ✅ [arXiv:2406.09403](https://arxiv.org/abs/2406.09403); *DeepEyes* (RL-incentivized thinking with images) ✅ [arXiv:2505.14362](https://arxiv.org/abs/2505.14362).
**Benchmarks:** V\*Bench (high-res needle search); BLINK.
**Directions:** RL-induced zoom/crop policies without SFT cold-start; validated tool suites (OCR, segmentation, drawing); intrinsic "visual imagination" vs. external tools.

---

## 6.4 Reward Modeling & Verification With Vision (process reward models)
**Why it's hard:** Step-level correctness in multimodal chains depends on whether each step is *visually grounded*, not just logically valid; automated process supervision and human step-annotation are far harder to obtain than for text math.

**Key papers:** *VisualPRM* (VisualPRM400K, VisualProcessBench) ✅ [arXiv:2503.10291](https://arxiv.org/abs/2503.10291).
**Benchmarks:** VisualProcessBench (step-wise correctness); Best-of-N gains on MathVista, MMMU-Pro.
**Directions:** verifiers that check visual *premises* per step; generative/self-verifying reward models; process rewards that penalize ungrounded steps.

---

## 6.5 Math / Diagram / Scientific Reasoning
**Why it's hard:** Requires jointly parsing dense diagrams (geometry, graphs, charts) *and* executing multi-step symbolic reasoning; small perceptual misreads cascade into wrong answers, and many "visual" items are secretly text-solvable.

**Key papers:** *MATH-Vision* ✅ [arXiv:2402.14804](https://arxiv.org/abs/2402.14804); *MathVista* ✅ [arXiv:2310.02255](https://arxiv.org/abs/2310.02255); *Vision-R1* (73.5% MathVista @7B) ✅ [arXiv:2503.06749](https://arxiv.org/abs/2503.06749).
**Benchmarks:** MathVista, MATH-Vision, MMMU-Pro.
**Directions:** explicit-visual-dependency filtering; diagram-faithful perception pretraining; tool-augmented geometric construction.

---

## Unverified / candidates
- Search surfaced implausible future-dated IDs (e.g. "2604.16060", "2606.08231") — **excluded** as hallucinated.
- OpenAI **o3** "thinking with images" (crop/zoom/rotate in CoT) is a product capability described secondhand — cite as a *system*, not a paper.
