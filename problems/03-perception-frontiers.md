# 3. Perception Frontiers

What can multimodal models still not *see*? Fine detail, composition, space, and structured documents remain where capable models fail in ways that cap everything downstream.

> **Verification note:** "CHAOS (CHart Analysis with Outlier Samples)" — flagged as possibly fabricated in the original draft — is **REAL**: a chart-robustness benchmark, ✅ [arXiv:2505.17235](https://arxiv.org/abs/2505.17235) (Moured et al., May 2025). It applies 5 textual + 10 visual perturbations at 3 severity levels to measure MLLM robustness drop on ChartQA / Chart-to-Text. (The original draft placed it under *agents*; it belongs here.)

---

## 3.1 Compositional Reasoning (attribute binding, word order, relations)
**Why it's hard:** VLMs behave like bag-of-words matchers — CLIP-style contrastive training collapses word order, so "grass in the mug" vs "mug in the grass" get near-identical embeddings. Correctly binding attributes/relations to the right object needs structured representations current models lack.

**Key papers:** *Winoground*, CVPR 2022 — ✅ [arXiv:2204.03162](https://arxiv.org/abs/2204.03162) (SOTA VLMs near chance on minimal-pair swaps).
**Benchmarks:** Winoground (400 image-caption minimal pairs; text/image/group matching under identical word sets). See also SugarCrepe / ARO in [cluster 1](01-representation-fusion-alignment.md#15-binding--cross-modal-grounding).
**Directions:** scene-graph supervision, hard-negative training, generative-feature distillation, inference-time structural reasoning.

---

## 3.2 Fine-grained & High-resolution Perception ("CLIP-blind pairs")
**Why it's hard:** CLIP encoders map visually distinct images (orientation, count, feature presence) to near-identical vectors; MLLMs inherit these blind spots and hallucinate on tiny text or dense scenes.

**Key papers:** *Eyes Wide Shut?* (MMVP), CVPR 2024 — ✅ [arXiv:2401.06209](https://arxiv.org/abs/2401.06209); *BLINK*, ECCV 2024 — ✅ [arXiv:2404.12390](https://arxiv.org/abs/2404.12390).
**Benchmarks:** MMVP (CLIP-blind pairs, 9 visual patterns); BLINK (14 classic-CV tasks → 3,807 MCQs; GPT-4V 51% vs human 96%).
**Directions:** Mixture-of-Features (vision SSL + CLIP), higher native resolution / tiling, non-contrastive vision encoders.

---

## 3.3 Spatial / Geometric / 3D Reasoning & Counting from 2D
**Why it's hard:** 2D inputs underdetermine depth/geometry; language-mediated reasoning can't recover relative depth, correspondence, or multi-view structure, and counting fails in dense scenes.

**Key papers:** BLINK ✅ [2404.12390](https://arxiv.org/abs/2404.12390) (relative depth, correspondence, multi-view); MathVista ✅ [2310.02255](https://arxiv.org/abs/2310.02255) (geometric/quantitative visual reasoning).
**Benchmarks:** BLINK; MathVista (6,141 examples).
**Directions:** explicit 3D priors, depth-aware pretraining, tool-augmented counting, CoT over spatial primitives.

---

## 3.4 Chart, Document, Table & Diagram Understanding (OCR-free)
**Why it's hard:** Requires jointly reading dense small text *and* reasoning over structured layout (axes, legends, cells) without an OCR crutch — and robustness collapses under mild perturbations.

**Key papers:** CharXiv, NeurIPS 2024 — ✅ [arXiv:2406.18521](https://arxiv.org/abs/2406.18521); ChartQA, ACL Findings 2022 — ✅ [arXiv:2203.10244](https://arxiv.org/abs/2203.10244); CHAOS — ✅ [arXiv:2505.17235](https://arxiv.org/abs/2505.17235).
**Benchmarks:** ChartQA (9.6K human + 23.1K generated Q); CharXiv (2,323 real arXiv charts; GPT-4o 47% vs human 80.5%); CHAOS (robustness under perturbation); DocVQA (50K Q / 12K docs); TextVQA (scene-text, 45K Q).
**Directions:** OCR-free pixel-to-structure decoders, layout-aware pretraining, robustness-focused augmentation.

---

## 3.5 Visual Grounding / Referring / Pixel-level Precision
**Why it's hard:** Mapping language to precise pixel regions demands localization far finer than image-level captioning; MLLMs often "know" an object exists but cannot point to it accurately.

**Key papers:** MMVP ✅ [2401.06209](https://arxiv.org/abs/2401.06209) (grounding gains from vision-SSL features); MMMU, CVPR 2024 — ✅ [arXiv:2311.16502](https://arxiv.org/abs/2311.16502) (expert-level grounded reasoning, 30 image types).
**Benchmarks:** MMMU (11.5K college-level Q); MMVP.
**Directions:** segmentation-aware decoders, referring-expression RL, region-token architectures.

---

## Unverified / candidates
- ChartBench and PlotQA (WACV 2020) exist but were only confirmed via secondary sources; verify before citing.
- All eight core benchmarks (Winoground, MMVP, BLINK, MMMU, MathVista, DocVQA, TextVQA, CHAOS) were primary-source verified.
