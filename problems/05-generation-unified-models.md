# 5. Generation & Unified Models

Can one model both *perceive* and *generate* across modalities — and how do we even score the output when the target isn't a single ground-truth?

---

## 5.1 Unified Understanding + Generation
**Why it's hard:** A single backbone must serve two conflicting objectives — understanding wants high-level semantic features, generation wants fine-grained reconstructable detail. Continuous signals resist clean tokenization, and the field is split between autoregressive (discrete tokens) and diffusion (continuous) paradigms with no clear winner. Decoupling encoders helps but complicates unification.

**Key papers:** *Chameleon* (early-fusion, token-based) ✅ [arXiv:2405.09818](https://arxiv.org/abs/2405.09818); *Transfusion* (next-token + diffusion loss in one transformer) ✅ [arXiv:2408.11039](https://arxiv.org/abs/2408.11039); *Emu3* (pure next-token over text/image/video) ✅ [arXiv:2409.18869](https://arxiv.org/abs/2409.18869); *Janus / Janus-Pro* (decoupled visual encoding) ✅ [arXiv:2410.13848](https://arxiv.org/abs/2410.13848), [arXiv:2501.17811](https://arxiv.org/abs/2501.17811).
**Directions:** better continuous tokenizers/VAEs, hybrid AR+diffusion training, resolving the understanding-vs-generation granularity conflict.

---

## 5.2 Any-to-Any / Omni Models
**Why it's hard:** Aligning ≥4 modalities into one sequence space is hard — each has its own tokenizer (EnCodec, SpeechTokenizer, image VQ), interleaved multimodal instruction data is scarce, and cross-modal generation quality lags specialist models.

**Key papers:** *AnyGPT* (discrete-token unified LLM over speech/text/image/music) ✅ [arXiv:2402.12226](https://arxiv.org/abs/2402.12226); *Unified Multimodal Understanding and Generation Models* (survey) ✅ [arXiv:2505.02567](https://arxiv.org/abs/2505.02567).
**Directions:** scalable any-to-any instruction data, streaming generation, disaggregated serving.

---

## 5.3 Compositional & Instruction-Following Failures (T2I / T2V)
**Why it's hard:** Diffusion cross-attention leaks attributes across objects ("catastrophic neglect," mis-binding), and numeracy/spatial relations stay weak because the text encoder loses compositional structure. Legible text rendering is still fragile.

**Key papers:** *Attend-and-Excite* (diagnoses neglect + binding, inference-time fix) ✅ [arXiv:2301.13826](https://arxiv.org/abs/2301.13826); *ELLA* (LLM adapter for dense prompts) ✅ [arXiv:2403.05135](https://arxiv.org/abs/2403.05135).
**Benchmarks:** **GenEval** (co-occurrence/position/count/color via detection) ✅ [arXiv:2310.11513](https://arxiv.org/abs/2310.11513); **T2I-CompBench(++)** (6k–8k prompts: binding, relations, numeracy) ✅ [arXiv:2307.06350](https://arxiv.org/abs/2307.06350); **DPG-Bench** (1k dense prompts, from ELLA).
**Directions:** LLM-grounded planning, layout/region control, RL from compositional rewards.

---

## 5.4 Evaluation of Generative Output (why FID / CLIPScore / IS fail)
**Why it's hard:** FID/IS give only holistic distributional quality; CLIPScore gives coarse alignment. None do instance-level compositional checks or match human judgment on aesthetics, reasoning, bias, or safety.

**Benchmarks:** **HEIM** — 12 aspects (alignment, quality, aesthetics, reasoning, bias, toxicity, fairness, robustness, multilinguality, efficiency), 62 scenarios, human eval — ✅ [arXiv:2311.04287](https://arxiv.org/abs/2311.04287); GenEval & T2I-CompBench (above) for fine-grained alignment.
**Directions:** human-aligned learned metrics, MLLM-as-judge calibration, reference-free scoring.

---

## 5.5 Controllability, Consistency & World-Model Coherence in Video
**Why it's hard:** Video adds temporal flickering, identity/subject drift, and physics violations; models act as unreliable "world simulators" lacking persistent state and physical commonsense.

**Benchmarks:** **VBench / VBench++** — 16 disentangled dimensions incl. subject consistency, motion smoothness, temporal flickering — ✅ [arXiv:2311.17982](https://arxiv.org/abs/2311.17982), [arXiv:2411.13503](https://arxiv.org/abs/2411.13503); **VideoPhy-2** — action-centric physical commonsense, 3,940 prompts + auto-evaluator — ✅ [arXiv:2503.06800](https://arxiv.org/abs/2503.06800).
**Directions:** persistent-state world models, physics-grounded objectives, long-horizon identity/temporal consistency.

---

## Unverified / candidates
- *PhyWorldBench, NExT-OMNI, AR-Omni, M2-omni, OmniFlow* surfaced in search but IDs/venues not individually verified — candidates only.
- Several search hits carried implausible future-dated IDs (2601.*–2606.*) and were **excluded** as unverifiable.
