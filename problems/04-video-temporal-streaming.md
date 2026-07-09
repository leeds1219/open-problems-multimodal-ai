# 4. Video, Temporal & Streaming Understanding

How do we reason over *time* — at length, in the right order, and in real time — instead of collapsing video to a bag of frames?

---

## 4.1 Long-form Video Understanding (hours-long)
**Why it's hard:** An hour of video is millions of pixels, but a fixed context/token budget forces aggressive subsampling or compression — trading coverage against detail. "Needle-in-haystack" questions require retrieving sparse evidence from long spans, yet dense encoding is computationally infeasible.

**Key papers:** *MovieChat* (dense-to-sparse memory) ✅ [arXiv:2307.16449](https://arxiv.org/abs/2307.16449); *LongVU* (spatiotemporal adaptive compression) ✅ [arXiv:2410.17434](https://arxiv.org/abs/2410.17434); *STORM* (token-efficient long video) ✅ [arXiv:2503.04130](https://arxiv.org/abs/2503.04130).
**Benchmarks:** **Video-MME**, CVPR 2025 — ✅ [arXiv:2405.21075](https://arxiv.org/abs/2405.21075) (900 videos, 11s–1hr, 2,700 QA); **MLVU** — ✅ [arXiv:2406.04264](https://arxiv.org/abs/2406.04264) (1,730 videos, 3min–2hr, 9 tasks); **LongVideoBench**, NeurIPS 2024 — ✅ [arXiv:2407.15754](https://arxiv.org/abs/2407.15754) (3,763 videos, 6,678 MCQs).
**Directions:** learned memory/retrieval over KV-cache; query-conditioned token pruning; hierarchical/agentic frame selection; length extrapolation.

---

## 4.2 Temporal Reasoning & Grounding vs. Static-frame Shortcuts
**Why it's hard:** Many "video" benchmarks are solvable from a single frame or the text alone, so high scores can reflect spatial/language priors rather than reasoning about order, causality, or event localization. Building tasks that *provably* require the frame sequence — and models that use it — is unsolved.

**Key papers:** *TemporalBench* ✅ [arXiv:2410.10818](https://arxiv.org/abs/2410.10818) (GPT-4o ~38.5%, ~30% below human); *Temporal Reasoning Transfer from Text to Video* ✅ [arXiv:2410.06166](https://arxiv.org/abs/2410.06166).
**Benchmarks:** **TempCompass** ✅ [arXiv:2403.00476](https://arxiv.org/abs/2403.00476) (order/speed/direction); **NExT-QA**, CVPR 2021 — ✅ [arXiv:2105.08276](https://arxiv.org/abs/2105.08276) (causal/temporal/descriptive QA); TemporalBench.
**Directions:** shortcut-resistant / counterfactual pair construction; explicit event-graph & causal representations; text-to-video temporal skill transfer.

---

## 4.3 Online / Streaming Video Understanding
**Why it's hard:** Real-time agents must perceive, decide *when* to respond, and answer causally without seeing the future — all while memory grows unbounded. This is fundamentally different from offline models that batch-process every frame first.

**Key papers:** *VideoLLM-online*, CVPR 2024 — ✅ [arXiv:2406.11816](https://arxiv.org/abs/2406.11816) (>10 FPS on A100); *Dispider* (disentangled perception/decision/reaction) ✅ [arXiv:2501.03218](https://arxiv.org/abs/2501.03218).
**Benchmarks:** **StreamingBench** ✅ [arXiv:2411.03628](https://arxiv.org/abs/2411.03628) (900 videos, 4,500 timestamped QA, 18 tasks).
**Directions:** proactive response timing; bounded streaming memory/token compression; aligning training objective with causal streaming inference.

---

## 4.4 Audio-Visual Joint Understanding & Synchronization
**Why it's hard:** Models often lean on one dominant modality; genuine joint reasoning requires temporally aligning sound events with visuals and detecting desynchronization. Separating *temporal* sync from *semantic* correspondence is under-measured.

**Key papers/benchmarks:** *DAVE* (diagnostic audio-visual eval) ✅ [arXiv:2503.09321](https://arxiv.org/abs/2503.09321); *AV-SyncBench: Decoupled Benchmarking of Temporal and Semantic Audio-Visual Synchronization*, Interspeech 2026 — ✅ [arXiv:2607.00726](https://arxiv.org/abs/2607.00726).
**Directions:** forcing cross-modal dependency (unanswerable from one modality); fine-grained audio-visual alignment; robustness to modality dissonance.

---

## 4.5 Egocentric Video Understanding
**Why it's hard:** First-person video has severe camera motion, occlusion, and hand-object interaction, plus long-horizon intent/procedural structure — distinct from third-person web video.

**Key papers/benchmarks:** **Ego4D**, CVPR 2022 — ✅ [arXiv:2110.07058](https://arxiv.org/abs/2110.07058) (3,670 hrs, 931 wearers); **EgoSchema** ✅ [arXiv:2308.09126](https://arxiv.org/abs/2308.09126) (3-min clips, long-certificate diagnostic MCQ).
**Directions:** long-horizon procedural/intent reasoning; gaze/3D-grounded models; egocentric streaming assistants.

---

## Unverified / candidates
- **EgoSchema** ID (2308.09126) and **TempCompass** ID (2403.00476) came via secondary sources — benchmarks are well-attested, re-confirm the IDs.
- AVQA and OmniBench appeared only in summaries — do not cite without direct verification.
- *AV-SyncBench* (2607.00726) was **verified** against the arXiv API on 2026-07-09 and promoted to the main list above.
