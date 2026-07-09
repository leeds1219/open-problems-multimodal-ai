# 8. Data, Efficiency & Evaluation (Cross-Cutting)

The substrate under every other cluster: where the data comes from, how continuous signals become tokens, what it all costs, and whether our benchmarks measure anything real. A good idea here can move all the others at once.

---

## 8.1 Paired/Aligned Data Scarcity & Synthetic-Data Risks
**Why it's hard:** High-quality aligned pairs (image/video/audio ↔ text) are scarce, expensive, and noisy at web scale. The obvious fix — generating synthetic pairs — risks compounding errors and eroding distribution tails; recursive training on model output causes irreversible *model collapse*, so synthetic augmentation cannot naively substitute for real coverage.

**Key papers:** *The Curse of Recursion: Training on Generated Data Makes Models Forget* ✅ [arXiv:2305.17493](https://arxiv.org/abs/2305.17493) (later in *Nature*, 2024).
**Directions:** provenance/watermark-aware curation, real-synthetic mixing ratios that provably avoid collapse, generated-text detection as a data filter.

---

## 8.2 Tokenization of Continuous Signals (VQ vs. continuous)
**Why it's hard:** Discrete visual/audio tokenizers (VQ) impose a hard information bottleneck — codebook collapse and repeated-patch artifacts cap fidelity — yet continuous representations complicate autoregressive/likelihood modeling. Choosing token count trades reconstruction fidelity against sequence length and compute.

**Key papers:** *MoVQ* (modulated quantized vectors) ✅ [arXiv:2209.09002](https://arxiv.org/abs/2209.09002), NeurIPS 2022; *Autoregressive Image Generation without Vector Quantization* (MAR / diffusion loss) ✅ [arXiv:2406.11838](https://arxiv.org/abs/2406.11838), NeurIPS 2024.
**Directions:** continuous-token AR modeling, adaptive/variable-rate tokenizers, unified image-audio-video codecs.

---

## 8.3 Efficiency: Long-Context Multimodal Is Expensive
**Why it's hard:** A single high-res image is hundreds of tokens; long video reaches millions — blowing up attention cost and KV-cache memory. Visual tokens are highly redundant, but pruning/merging must preserve task-relevant detail.

**Key papers:** *FastV* — *An Image is Worth 1/2 Tokens After Layer 2* ✅ [arXiv:2403.06764](https://arxiv.org/abs/2403.06764), ECCV 2024; *Token Merging* (ToMe) ✅ [arXiv:2210.09461](https://arxiv.org/abs/2210.09461), ICLR 2023; *LongVU* ✅ [arXiv:2410.17434](https://arxiv.org/abs/2410.17434).
**Directions:** query-aware / streaming KV-cache eviction, learned vs. training-free pruning, temporal-redundancy encoders.

---

## 8.4 Evaluation Methodology: Contamination, Saturation, Shortcuts, Judge Bias
**Why it's hard:** Many "multimodal" benchmark questions are answerable *without* the image (LLM world-knowledge/text shortcuts), and unintentional training leakage inflates scores — so leaderboards saturate without measuring true multimodal reasoning. Using LMMs as judges adds its own biases and modality neglect.

**Key papers:** *Are We on the Right Way for Evaluating LVLMs?* (**MMStar**) ✅ [arXiv:2403.20330](https://arxiv.org/abs/2403.20330), NeurIPS 2024; *MMBench* (CircularEval) ✅ [arXiv:2307.06281](https://arxiv.org/abs/2307.06281), ECCV 2024; *MLLM-as-a-Judge* ✅ [arXiv:2402.04788](https://arxiv.org/abs/2402.04788).
**Benchmarks:** MMStar (1,500 vision-indispensable samples), MMBench.
**Directions:** vision-necessity filtering, leakage/contamination detection, robust judge de-biasing, dynamic/held-out test sets.

---

## 8.5 Safety, Bias & Fairness Across Modalities
**Why it's hard:** Cross-modal attack surfaces let benign-looking images bypass text-only safety alignment (typographic / query-relevant image jailbreaks), and biases can enter through *any* modality independently.

**Key papers:** *MM-SafetyBench* ✅ [arXiv:2311.17600](https://arxiv.org/abs/2311.17600), ECCV 2024 (13 scenarios, 5,040 text-image pairs).
**Directions:** dual-modal safety subspaces, cross-modal red-teaming, multi-turn image-input harm evaluation. (See also [cluster 7 security](07-agents-and-security.md#76-cross-modal-security-visual-injection-image-jailbreaks-propagation).)

---

## Unverified / candidates
- *MM-JudgeBias, OmniSafeBench-MM* surfaced with 2026 IDs (2604.*/2512.*) that could not be authenticated — unconfirmed.
- *STORM, InfiniPot-V, StreamMem* (video KV-cache) appeared in search but were not individually verified.
- Original *VQGAN* ("Taming Transformers," CVPR 2021, arXiv:2012.09841) is well-known but not re-fetched this pass.
