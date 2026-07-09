# 2. Hallucination & Faithfulness

Why do vision-language models describe things that aren't in the image — and when they *do* reason correctly, is the reasoning actually grounded in what they saw?

---

## Benchmark reference

| Benchmark | What it actually is | Cite |
|---|---|---|
| **POPE** | Polling-based Object Probing Evaluation. Binary "Is there a {object}?" queries with random/popular/adversarial negative sampling; reports Acc/P/R/F1. Li et al., EMNLP 2023. | ✅ [arXiv:2305.10355](https://arxiv.org/abs/2305.10355) |
| **CHAIR** | Caption Hallucination Assessment with Image Relevance — a *metric*, not a dataset. CHAIR_i (object-level) and CHAIR_s (sentence-level). Rohrbach et al., EMNLP 2018. | ✅ [arXiv:1809.02156](https://arxiv.org/abs/1809.02156) |
| **HallusionBench** | 346 images / 1129 Qs, disentangles *language hallucination* (prior overrides vision) from *visual illusion* (perception failure) via control-group pairs. Guan et al., CVPR 2024. | ✅ [arXiv:2310.14566](https://arxiv.org/abs/2310.14566) |
| **MME** | 14 perception+cognition subtasks, yes/no pairs; existence/count subtasks used as a hallucination proxy. Fu et al., 2023. | ✅ [arXiv:2306.13394](https://arxiv.org/abs/2306.13394) |
| **MMHal-Bench** | 96 image-question pairs, 8 categories × 12 object topics, GPT-4 scored; penalizes hallucination. From LLaVA-RLHF. | ✅ [arXiv:2309.14525](https://arxiv.org/abs/2309.14525) |
| **Object HalBench** | Object hallucination in *detailed* descriptions; reports CHAIR_s/_i via GPT-4. Defined/popularized by Woodpecker. | ✅ [arXiv:2310.16045](https://arxiv.org/abs/2310.16045) |

---

## 2.1 Object / Attribute / Relation Hallucination
**Why it's hard:** VLMs describe objects, attributes, and relations absent from the image. Object hallucination is the best-measured; *attribute* (color/count/state) and *relation* (spatial/interaction) hallucination are harder to benchmark and persist even when the model perceives the scene correctly.

**Key papers:** POPE ✅ [2305.10355](https://arxiv.org/abs/2305.10355); CHAIR ✅ [1809.02156](https://arxiv.org/abs/1809.02156); *Survey of Hallucination in MLLMs* ✅ [2404.18930](https://arxiv.org/abs/2404.18930).
**Directions:** fine-grained attribute/relation benchmarks; context-aware object-similarity metrics beyond exact match.

---

## 2.2 Root Causes (language prior, weak grounding, biased data)
**Why it's hard:** Causes are entangled — an oversized LLM decoder overrides vision (language prior), weak vision-language alignment interfaces, and co-occurrence bias in training data all contribute. Attributing a given hallucination to one cause is an open diagnostic problem.

**Key papers:** Survey ✅ [2404.18930](https://arxiv.org/abs/2404.18930); *VCD* (statistical bias + unimodal priors) ✅ [2311.16922](https://arxiv.org/abs/2311.16922); *OPERA* (attention over-trust on summary tokens) ✅ [2311.17911](https://arxiv.org/abs/2311.17911).
**Directions:** mechanistic interpretability of where priors override vision; causal-intervention benchmarks.

---

## 2.3 Mitigation and Its Incompleteness
**Why it's hard:** Every family is partial. **Decoding** methods (VCD, OPERA) are training-free but only reduce hallucination and can hurt fluency. **RLHF/DPO** methods risk reward hacking and the *unconditional preference problem* (model ignores the image during preference learning). **Grounding/correction** (Woodpecker) leans on external detectors that themselves err.

**Key papers:** VCD ✅ [2311.16922](https://arxiv.org/abs/2311.16922); OPERA ✅ [2311.17911](https://arxiv.org/abs/2311.17911); LLaVA-RLHF ✅ [2309.14525](https://arxiv.org/abs/2309.14525); *mDPO* ✅ [2406.11839](https://arxiv.org/abs/2406.11839); Woodpecker ✅ [2310.16045](https://arxiv.org/abs/2310.16045).
**Directions:** image-conditioned preference optimization; combining decoding + grounding without external-detector dependence.

---

## 2.4 Measuring Hallucination Reliably
**Why it's hard:** Discriminative metrics (POPE yes/no) are gameable and diverge from generative behavior; CHAIR is locked to a fixed COCO vocabulary; GPT-4-as-judge (MMHal, ObjHal) is costly and non-reproducible. No single metric jointly covers open-vocabulary + attributes + relations robustly.

**Key papers:** POPE ✅ [2305.10355](https://arxiv.org/abs/2305.10355); HallusionBench ✅ [2310.14566](https://arxiv.org/abs/2310.14566); MME ✅ [2306.13394](https://arxiv.org/abs/2306.13394).
**Directions:** open-vocabulary hallucination metrics; control-group / consistency protocols (HallusionBench-style) to cut judge variance.

---

## 2.5 Faithfulness of Multimodal Reasoning Chains
**Why it's hard:** Multimodal chain-of-thought traces often ignore or fabricate visual evidence yet still land on the right answer — so the rationale is post-hoc rationalization, not the true decision process. Dangerous in high-stakes (e.g. medical) use, and RL reward design can reward *mentioning* visual details regardless of correctness.

**Key papers (recent):** *On the Faithfulness of Visual Thinking* ⚠️ [2510.23482](https://arxiv.org/abs/2510.23482); *Reasoning Faithfulness in Medical VLMs via Multimodal Perturbations* ⚠️ [2510.11196](https://arxiv.org/abs/2510.11196).
**Directions:** perturbation-based faithfulness tests; reward shaping that credits *correct* visual grounding, not mere mention.

---

## Unverified / candidates
- "Object HalBench" as a *standalone* paper could not be confirmed; treat Woodpecker ([2310.16045](https://arxiv.org/abs/2310.16045)) as its defining source.
- Faithfulness works (2510.23482, 2510.11196) are 2025 preprints; peer-review status not confirmed beyond arXiv.
