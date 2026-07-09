# 7. Agents & Cross-Modal Security

Why do multimodal agents (GUI / computer-use / web / embodied) break over long horizons — and how are they attacked through their own eyes and ears?

> This cluster incorporates and corrects the original `open_problems_multimodal_agents.md` draft: the broken LaTeX is fixed there, and the **CHAOS** benchmark it cited is real but was miscategorized — it is a *chart-robustness* benchmark and now lives in [cluster 3](03-perception-frontiers.md).

---

## Benchmark reference

| Name | What it actually is | Cite |
|---|---|---|
| **OSWorld** | 369 execution-graded tasks in real Ubuntu/Windows/macOS; best model ~12% vs human 72% | ✅ [arXiv:2404.07972](https://arxiv.org/abs/2404.07972), NeurIPS 2024 |
| **VisualWebArena** | 910 visually-grounded tasks over self-hosted Classifieds/Shopping/Reddit | ✅ [arXiv:2401.13649](https://arxiv.org/abs/2401.13649), ACL 2024 |
| **WebArena** | Reproducible self-hosted web env, execution-based eval (base for VWA) | ✅ [arXiv:2307.13854](https://arxiv.org/abs/2307.13854), ICLR 2024 |
| **WebVoyager** | End-to-end LMM agent on 15 live sites; GPT-4V auto-eval (85% human agreement) | ✅ [arXiv:2401.13919](https://arxiv.org/abs/2401.13919), ACL 2024 |
| **AndroidWorld** | Live Android emulator, 116 parameterized tasks / 20 apps | ✅ [arXiv:2405.14573](https://arxiv.org/abs/2405.14573) |
| **Mind2Web** | 2,000+ tasks over 137 real websites (offline action prediction) | ✅ [arXiv:2306.06070](https://arxiv.org/abs/2306.06070), NeurIPS 2023 |
| **ScreenSpot** | First mobile/desktop/web GUI-grounding benchmark, from SeeClick | ✅ [arXiv:2401.10935](https://arxiv.org/abs/2401.10935), ACL 2024 |
| **Agent Smith** | "Infectious jailbreak" — one adversarial image spreads exponentially to ~1M LLaVA agents | ✅ [arXiv:2402.08567](https://arxiv.org/abs/2402.08567), ICML 2024 |

---

## 7.1 Visual Grounding of UI Elements
**Why it's hard:** Agents must map an instruction to precise pixel coordinates across heterogeneous mobile/desktop/web UIs, dense icons, and high-res professional apps. Grounding failure is the dominant OSWorld error mode and directly caps task success.

**Key papers:** SeeClick ✅ [arXiv:2401.10935](https://arxiv.org/abs/2401.10935); ScreenSpot-Pro ⚠️ [arXiv:2504.07981](https://arxiv.org/abs/2504.07981).
**Benchmarks:** ScreenSpot, ScreenSpot-Pro, OSWorld.
**Directions:** grounding-specific pretraining, set-of-marks / accessibility-tree fusion, high-res adaptive-focus perception.

---

## 7.2 Long-horizon Reliability & Compounding Errors
**Why it's hard:** Even a small per-step error rate compounds multiplicatively across dependent steps, collapsing reliable short-task performance into near-systematic failure. pass@1 poorly captures variance across episodes.

**Key papers:** *Measuring Long Horizon Execution in LLMs* ⚠️ [arXiv:2509.09677](https://arxiv.org/abs/2509.09677); *Beyond pass@1: A Reliability Science Framework* ⚠️ [arXiv:2603.29231](https://arxiv.org/abs/2603.29231) (2026 preprint — re-check).
**Benchmarks:** OSWorld, AndroidWorld, Mind2Web.
**Directions:** error-recovery/backtracking, self-audit, reliability metrics beyond mean success.

---

## 7.3 Episodic Memory Over Long Multimodal Histories & State Tracking
**Why it's hard:** Screenshot/video histories are huge; lossy compression and similarity retrieval introduce errors that *themselves* compound, and agents must track evolving entity state across many steps.

**Key papers:** *A Multimodal Agent with Long-Term Memory* ⚠️ [arXiv:2508.09736](https://arxiv.org/abs/2508.09736); *AMA-Bench* ⚠️ [arXiv:2602.22769](https://arxiv.org/abs/2602.22769) (2026 preprint — re-check).
**Directions:** structured spatiotemporal memory, verifiable retrieval, long-context vs. memory-augmentation trade-off studies.

---

## 7.4 Reward Specification / LMM-as-Judge Reliability
**Why it's hard:** Non-deterministic environments have no single gold trajectory, so evaluation leans on LMM judges — which show position/length/self-preference bias and, critically, *perceptual judgment bias*: rewarding plausible text over what the image actually shows.

**Key papers:** *MLLM-as-a-Judge* ✅ [arXiv:2402.04788](https://arxiv.org/abs/2402.04788); *Mitigating Perceptual Judgment Bias* ⚠️ [arXiv:2606.02578](https://arxiv.org/abs/2606.02578) (2026 preprint — re-check).
**Benchmarks:** MLLM-as-a-Judge; WebVoyager's GPT-4V auto-eval protocol.
**Directions:** perceptual-perturbation reward modeling, execution-based over judge-based eval, bias-mitigation pipelines.

---

## 7.5 Real-Time Latency & Asynchrony
**Why it's hard:** The world changes during inference, so a reasoned action can be stale on arrival; synchronous agents that pause to think fail badly under delay (ReAct GPT-4o: 47% sync → 11% async on Robotouille).

**Key papers:** *Robotouille* ✅ [arXiv:2502.05227](https://arxiv.org/abs/2502.05227), ICLR 2025; *RRARA* (async reflect) ⚠️ [arXiv:2506.07223](https://arxiv.org/abs/2506.07223).
**Benchmarks:** Robotouille.
**Directions:** async reflect/act loops, speculative tool-calling, "learning to wait" temporal synchronization.

---

## 7.6 Cross-Modal Security (visual injection, image jailbreaks, propagation)
**Why it's hard:** Instructions injected into the *visual* environment (pop-ups, chat, on-screen text) hijack agents that treat screen content as trusted; in multi-agent memory-sharing systems a single adversarial image can propagate exponentially. The core open question: how to *provably* isolate the trusted intent channel from the untrusted observation channel.

**Key papers:** *Attacking Vision-Language Computer Agents via Pop-ups* ✅ [arXiv:2411.02391](https://arxiv.org/abs/2411.02391) (≈92.7%/73.1% attack-hit on OSWorld/VWA); Agent Smith ✅ [arXiv:2402.08567](https://arxiv.org/abs/2402.08567).
**Benchmarks:** VPI-Bench ⚠️ [arXiv:2506.02456](https://arxiv.org/abs/2506.02456); attacks measured on OSWorld / VisualWebArena.
**Directions:** provable containment of infectious jailbreaks (open per Agent Smith), trusted-vs-untrusted screen-content separation, red-team environment-injection suites.

---

## Unverified / candidates
- Several citations carry early-2026 arXiv IDs (2602.*, 2603.*, 2606.*) that predate compilation but were **not fetched to primary-source depth** — marked ⚠️, re-check before formal use.
- EMemBench, MIRAGE, EVA appeared only in recent listings — excluded from the core lists.
