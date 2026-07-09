# 1. Representation, Fusion & Alignment

How do we embed heterogeneous modalities into a shared space where they actually cooperate rather than compete? These are the substrate problems — get them wrong and everything downstream inherits the damage.

---

## 1.1 The Modality Gap
**Why it's hard:** Even after contrastive training, image and text embeddings occupy separate, narrow cones in the shared space rather than interleaving. The gap arises from encoder initialization (the "cone effect") plus the contrastive temperature, and it is *not* understood whether closing it helps or hurts — its relationship to zero-shot accuracy and fairness is non-monotonic, so there is no principled target gap.

**Key papers**
- Liang et al., *Mind the Gap: Understanding the Modality Gap in Multi-modal Contrastive Representation Learning*, NeurIPS 2022 — ✅ [arXiv:2203.02053](https://arxiv.org/abs/2203.02053)
- *Explaining and Mitigating the Modality Gap in Contrastive Multimodal Learning*, 2024 — ✅ [arXiv:2412.07909](https://arxiv.org/abs/2412.07909)
- *Decipher the Modality Gap: From Convergent Representations to Pairwise Alignment*, 2025 — ⚠️ [arXiv:2510.03268](https://arxiv.org/abs/2510.03268)

**Directions:** temperature/geometry scheduling, post-hoc gap shifting, understanding gap vs. transfer trade-off.

---

## 1.2 Modality Imbalance / Dominance
**Why it's hard:** Under a single shared objective, the easiest-to-fit modality dominates the gradient and the other encoders stay under-optimized ("greedy learning") — so a multimodal model can underperform its *best unimodal* counterpart. Balancing methods (gradient modulation, Shapley re-weighting) are heuristic and dataset-specific.

**Key papers**
- Peng et al., *Balanced Multimodal Learning via On-the-fly Gradient Modulation* (OGM-GE), CVPR 2022 — ✅ [arXiv:2203.15332](https://arxiv.org/abs/2203.15332)
- *On-the-fly Modulation for Balanced Multimodal Learning*, TPAMI 2024 — ✅ [arXiv:2410.11582](https://arxiv.org/abs/2410.11582)
- *BalanceBenchmark* (survey + benchmark), 2025 — ⚠️ [arXiv:2502.10816](https://arxiv.org/abs/2502.10816)

**Benchmarks / datasets:** CREMA-D, Kinetics-Sounds, AV-MNIST.
**Directions:** adaptive per-modality optimization, gradient-guided distillation ([arXiv:2506.21514](https://arxiv.org/abs/2506.21514) ⚠️), SAM-based modulation.

---

## 1.3 Fusion Architecture & Connector Trade-offs
**Why it's hard:** Early vs. late vs. cross-attention fusion has no universal winner, and for VLMs the connector (Q-Former compression vs. MLP projector) trades token efficiency against alignment quality. Recent evidence favors simple MLP projectors for alignment — contradicting Q-Former's compression rationale.

**Key papers**
- Li et al., *BLIP-2* (Q-Former), ICML 2023 — ✅ venue confirmed / [arXiv:2301.12597](https://arxiv.org/abs/2301.12597)
- Liu et al., *Visual Instruction Tuning* (LLaVA, linear/MLP connector), NeurIPS 2023 — ✅ venue confirmed / [arXiv:2304.08485](https://arxiv.org/abs/2304.08485)
- *Deciphering Cross-Modal Alignment with the Modality Integration Rate (MIR)*, 2024 — ✅ [arXiv:2410.07167](https://arxiv.org/abs/2410.07167)

**Directions:** MIR-guided connector selection, learned/automated fusion policies.

---

## 1.4 Missing / Incomplete Modalities at Inference
**Why it's hard:** Multimodal transformers degrade sharply when a modality is absent at test time, and the best recovery strategy is dataset-dependent (early fusion wins on Hateful Memes, late on MM-IMDb). Dropout-invariant representations remain elusive.

**Key papers**
- Ma et al., *Are Multimodal Transformers Robust to Missing Modality?*, CVPR 2022 — ✅ [arXiv:2204.05454](https://arxiv.org/abs/2204.05454)
- *Multimodal Prompting with Missing Modalities*, CVPR 2023 — ✅ [arXiv:2303.03369](https://arxiv.org/abs/2303.03369)
- *MMP: Masked Modality Projection*, 2024 — ✅ [arXiv:2410.03010](https://arxiv.org/abs/2410.03010)

**Benchmarks:** MM-IMDb, Hateful Memes, CMU-MOSEI.
**Directions:** proxy tokens, uncertainty-aware fusion.

---

## 1.5 Binding / Cross-Modal Grounding
**Why it's hard:** VLMs are object-centric and fail to bind attributes/relations to the right entity (e.g., swapping colors between two objects) — contrastive encoders behave like bags-of-words. Diagnosing and fixing binding at the *representation* level is open. (Closely related to §3.1 compositional reasoning.)

**Key papers**
- Thrush et al., *Winoground*, CVPR 2022 — ✅ [arXiv:2204.03162](https://arxiv.org/abs/2204.03162)
- Hsieh et al., *SugarCrepe*, NeurIPS 2023 — ✅ [arXiv:2306.14610](https://arxiv.org/abs/2306.14610)
- Yuksekgonul et al., *ARO* (When and why VLMs behave like bags-of-words), ICLR 2023 — ✅ [arXiv:2210.01936](https://arxiv.org/abs/2210.01936)

**Directions:** hard-negative training, dense/aligned captions, mechanistic attention analysis.

---

## 1.6 Scaling Laws & the "More Modalities" Question
**Why it's hard:** Mixed-modal scaling shows a *competition barrier*: at small scale modalities compete for capacity (interference), transitioning to synergy only past a threshold — so adding modalities can *hurt* below it. Whether early- vs. late-fusion advantages persist across scale is unsettled.

**Key papers**
- Aghajanyan et al., *Scaling Laws for Generative Mixed-Modal Language Models*, ICML 2023 — ✅ [arXiv:2301.03728](https://arxiv.org/abs/2301.03728)
- Shukor et al., *Scaling Laws for Native Multimodal Models*, 2025 — ⚠️ [arXiv:2504.07951](https://arxiv.org/abs/2504.07951)
- Girdhar et al., *ImageBind*, CVPR 2023 — ✅ [arXiv:2305.05665](https://arxiv.org/abs/2305.05665)

**Directions:** capacity routing to reduce interference, modality-count / data-mix scaling laws.

---

## Unverified / candidates
- Exact arXiv IDs for Winoground (2204.03162), ARO (2210.01936), BLIP-2 (2301.12597), LLaVA (2304.08485) were partly cited from memory during research; **venues are confirmed**, re-fetch the IDs if using them formally.
- *Dense and Aligned Captions* (2305.19595) surfaced but its venue was not confirmed.
