# LLM Fundamentals: Complete Study + Novel Research Directions

**Created:** 2026-06-14/15
**Author:** Bilawal (bilawalriaz)
**GitHub Pages:** https://bilawalriaz.github.io/llm-fundamentals/
**Synthesis:** https://bilawalriaz.github.io/llm-fundamentals/synthesis.html
**Repo:** https://github.com/bilawalriaz/llm-fundamentals
**Local:** ~/work/llm-fundamentals/

---

## Background

I have practical experience with Unsloth, SFT, and DPO but lacked deep fundamental understanding of how LLMs work. I know about embeddings, tensors, and backprop at a surface level. This study was designed to build that foundation from 18 key papers, then extract novel insights from reading them together.

**Hardware constraints:**
- RTX 2070 Super (8GB VRAM) — daily driver for 3-4B models
- M3 MacBook Pro (36GB) — larger experiments (7-14B)

---

## The 18 Papers

### Tier 1: Foundations
1. **The Illustrated Transformer** — Jay Alammar, 2018/2025 — [Blog](https://jalammar.github.io/illustrated-transformer/) — Self-attention, Q/K/V, multi-head attention, positional encoding
2. **TinyStories** — Eldan & Li, 2023 — [arXiv:2305.07759](https://arxiv.org/abs/2305.07759) — <10M param models, capability emergence hierarchy (grammar→consistency→creativity)
3. **Textbooks Are All You Need (Phi-1)** — Gunasekar et al., 2023 — [arXiv:2306.11644](https://arxiv.org/abs/2306.11644) — 1.3B beats 15.5B, data quality > scale, 7B tokens
4. **SmolLM2** — HuggingFace, 2025 — [arXiv:2502.02737](https://arxiv.org/abs/2502.02737) — 1.7B model, 11T tokens, 4-stage training with online data rebalancing
5. **The Flan Collection** — Longpre et al., 2023 — [arXiv:2301.13688](https://arxiv.org/abs/2301.13688) — 1800+ tasks, mixed prompt training, input inversion, task balancing

### Tier 2: Parameter-Efficient Fine-Tuning
6. **LoRA** — Hu et al. (Microsoft), 2021 — [arXiv:2106.09685](https://arxiv.org/abs/2106.09685) — ΔW=BA, rank 4-8 sufficient, 10,000x fewer params, zero inference latency
7. **QLoRA** — Dettmers et al., 2023 — [arXiv:2305.14314](https://arxiv.org/abs/2305.14314) — 4-bit NF4 + double quantization + paged optimizers, 65B on one GPU

### Tier 3: Alignment
8. **LIMA** — Zhou et al. (Meta), 2023 — [arXiv:2305.11206](https://arxiv.org/abs/2305.11206) — Superficial alignment hypothesis, 1,000 examples = 43% parity with GPT-4
9. **SELF-INSTRUCT** — Wang et al., 2022 — [arXiv:2212.10560](https://arxiv.org/abs/2212.10560) — Bootstrapping instruction data from 175 seeds
10. **DPO** — Rafailov et al. (Stanford), 2023 — [arXiv:2305.18290](https://arxiv.org/abs/2305.18290) — RLHF as classification loss, Z(x) cancels, dynamic gradient weighting
11. **ZEPHYR** — Tunstall et al. (HuggingFace), 2023 — [arXiv:2310.16944](https://arxiv.org/abs/2310.16944) — dSFT + AI Feedback + dDPO, 7B surpasses Llama2-Chat-70B
12. **ORPO** — Hong et al., 2024 — [arXiv:2403.07691](https://arxiv.org/abs/2403.07691) — Monolithic SFT+alignment, odds ratio, no reference model

### Tier 4: Advanced RL & Modern Pipelines
13. **SFT-DPO Interaction** — Harry et al., 2026 — [arXiv:2603.20100](https://arxiv.org/abs/2603.20100) — FFT beats LoRA by more than SFT beats DPO (12:1 ratio)
14. **DeepSeekMath & GRPO** — Shao et al. (DeepSeek), 2024 — [arXiv:2402.03300](https://arxiv.org/abs/2402.03300) — PPO without value function, group-relative advantages, RL improves Maj@K not Pass@K
15. **SFT-GRPO Data Overlap** — 2026 — [arXiv:2604.13515](https://arxiv.org/abs/2604.13515) — Disjoint data pools = free +10.4pp, reward saturation
16. **Phi-3** — Microsoft, 2024 — [arXiv:2404.14219](https://arxiv.org/abs/2404.14219) — 3.8B on iPhone, LongRope, blocksparse attention, data-optimal regime
17. **Qwen3** — Alibaba, 2025 — [arXiv:2505.09388](https://arxiv.org/abs/2505.09388) — Thinking/non-thinking modes, 4-stage post-training, strong-to-weak distillation
18. **Unsloth DPO/ORPO/KTO Guide** — Unsloth Docs, 2025 — [Docs](https://unsloth.ai/docs/get-started/reinforcement-learning-rl-guide/preference-dpo-orpo-and-kto) — Practical implementation, code examples

---

## 18 Novel Cross-Paper Insights (from synthesis)

### Core Insights (1-9)

**1. Alignment is two different problems.** Style alignment (LIMA: 1,000 examples, format matching, cheap) vs reasoning alignment (Qwen3/DeepSeekMath: multi-stage RL, capability building, expensive). The field calls both "alignment" but they're fundamentally different operations.

**2. Training is curriculum design.** Phi-1 (textbook→exercise), SmolLM2 (rebalance mid-flight), Qwen3 (cold start→RL→fusion→RL). The optimal data at each stage is different. The optimal objective at each stage is different.

**3. Knowledge is 99% scaffolding.** LoRA rank 4-8, TinyStories 10M params, Phi-1 7B tokens, QLoRA 4-bit, LIMA 1,000 examples — all suggest actual knowledge is far lower-dimensional than parameter count implies.

**4. The evaluation crisis is the real bottleneck.** GPT-4 as judge: τ=0.43 with humans. Paper 13's 12:1 finding was only discovered through careful ablations. Most papers don't run them.

**5. The alignment tax paradox.** If alignment is superficial (LIMA), why does it degrade factual knowledge? Because format-matching overwrites weights that were storing knowledge.

**6. Hardware shapes research.** QLoRA, Phi-3, Unsloth — the "optimal" model is the best one that fits in your VRAM. This constraint creates the entire quantization/PEFT/distillation research direction.

**7. Human annotation extinction.** SELF-INSTRUCT → ZEPHYR (AI feedback) → Qwen3 (distillation). The end state: large model teaches small model, no humans. The ceiling question: student can never exceed teacher.

**8. Five contradictions nobody resolves.** LoRA placement (attention-only vs all-layers), DPO value (61% vs 0.18 points), SFT quantity (1K vs 11-point drop), reference model (required→eliminated→shared), online vs offline RL.

**9. The implicit 2026 recipe.** Pretrain curated → SFT 1K-50K high-quality → GRPO for verifiable tasks, DPO for subjective → Distill strong-to-weak → Deploy INT4 → Evaluate honestly across parameterizations.

### Advanced Insights (10-18)

**10. All post-training is distribution shaping, not knowledge addition.** LoRA learns orthogonal deltas. GRPO improves Maj@K not Pass@K. DPO reweights existing outputs. Every method concentrates probability mass on desired regions. Knowledge is surfaced, not added.

**11. Small models have qualitatively different failure modes.** Phi-1 counting failures are structural. SmolLM2 math ceiling at 32%. Small models can't explore via RL. They need distillation, not scaled-down recipes.

**12. DPO and GRPO are mathematically incommensurable.** DPO assumes transitive preferences (A > B). GRPO assumes cardinal rewards (A=0.7, B=0.3). Different axioms about what "better" means. You can't directly compare papers using different frameworks.

**13. No technique reduces total compute — they shift it.** QLoRA: memory→compute. GRPO: critic→64× samples. LoRA: trainable→frozen. Blocksparse: KV cache→pattern overhead. Each trades one resource for another.

**14. Teacher quality ceiling is unavoidable.** GPT-4 bounds Phi-1 data. GPT-4 bounds ZEPHYR alignment. 235B bounds Qwen3 student. GPT-4 bounds evaluation (τ=0.43). No demonstrated path to super-LLM through AI feedback alone.

**15. Negative data blind spot.** Phi-1 removed 40% — barely hurt. SFT-GRPO overlap: exclusion *improved* by +10pp. What you exclude matters as much as what you include. No theory of training data toxicity.

**16. Context extension: 3 papers, 3 methods, 0 comparisons.** Phi-3 (LongRope), Qwen3 (ABF+YARN), SmolLM2 (RoPE scaling). Nobody has compared head-to-head.

**17. Seven questions the field should be asking.** Information-theoretic lower bound on data. Why code→math transfer is one-way. When online RL becomes net beneficial. Can we factor models into knowledge + scaffolding. Optimal synthetic/real data ratio. What SFT-GRPO overlap really means. What defines "done."

**18. Creative Writing GRPO: Reward-Modelled, Not Verified.** (Corrected after peer review — see below.)

---

## The Creative Writing GRPO Architecture

### Original idea
Use human + SOTA LLM collaboration to define writing style → train a classifier on that style → use classifier as reward model for GRPO on 0.5B model.

### Peer review corrections (important)

The original framing overclaimed. Here's what was corrected:

**Overclaims:**
- ~~"Every domain becomes verifiable"~~ → "many subjective domains become cheaply rankable enough for RL"
- ~~"Classifier replaces compiler"~~ → "classifier replaces the LLM judge. Learned aesthetic filter, not hard validity gate"
- ~~"GRPO wins over DPO"~~ → "depends on reward model quality. DPO is boring but robust"
- ~~"No teacher ceiling after classifier is trained"~~ → "ceiling becomes classifier-recognisable quality, not true quality"
- ~~"Train once, use forever"~~ → "reward models rot under optimization. Need periodic relabelling + adversarial examples"

**What survives:**
- The architecture is real and testable
- The corrected thesis: "can a small calibrated reward model act as a practical verifier proxy for subjective generation, enabling GRPO-style online improvement in tiny LMs?"
- The key line: "Creative writing has no compiler, but it can have a calibrated reward model. GRPO does not require objective truth; it requires a reward signal with enough local consistency that group-relative ranking improves the policy faster than it exploits the reward model."

### Corrected architecture

```
Stage 1: Human + SOTA LLM define style preferences
         → 200-500 human-rated creative outputs (calibration set)
         → SOTA LLM rates 50K outputs on same axes
         → Calibrate LLM ratings against human baseline

Stage 2: Train reward classifier (0.5B-1B model)
         → Input: creative text → Output: quality score
         → This is a learned aesthetic filter, NOT a compiler

Stage 3: GRPO on 0.5B model using classifier as reward
         → 0.5B generates G=64 creative completions
         → Classifier scores each one
         → GRPO updates policy based on group-relative advantages
         → Disjoint data from SFT per Paper 15

Stage 4: Periodic recalibration
         → Re-sample human evaluations every N iterations
         → Check for Goodhart's Law effects (style collapse, purple prose)
         → Update classifier with adversarial examples
```

### Tighter experimental design (from peer review)

Train four models from the same 0.5B base:
1. SFT only
2. DPO from pairwise preferences
3. GRPO using SOTA LLM judge directly
4. GRPO using trained reward classifier

Evaluate with **blind human ranking on prompts never seen by SFT, reward model, or GRPO.** Also measure classifier-human correlation before and after GRPO.

**Key result is NOT:** "classifier score goes up" (trivial, maybe Goodhart)
**Key result IS:** "classifier score goes up AND blind human preference also goes up without style collapse"

**Adversarial checks needed:** Reward models often prefer purple prose, emotional overstatement, weird "literary" cadence, verbosity, fake profundity, or rubric-gaming. The model may learn to write like a contest entrant trying to seduce the judge rather than like a better writer.

---

## The Self-Improving Loop for Tiny Models

### Architecture (tool-augmented, 0.5B model)

```
Model output schema:
  {
    "thought": "I need to calculate X",
    "action": "calculate",
    "args": {"expression": "..."},
    "result": null
  }

Tool returns verified result → model continues reasoning
```

### Self-improving loop (for verifiable domains)

```
Phase 1: Teacher generates 5K harnessed reasoning traces → SFT 0.5B
Phase 2: 0.5B tackles 10K novel problems with tool access
Phase 3: Real-world verifiers score outputs (code tests, math correctness)
Phase 4: Filter successful traces → retrain (disjoint data)
Phase 5: Repeat from Phase 2
```

No teacher ceiling after Phase 1. Verifiers ARE ground truth.

### For creative domains (with trained classifier)

Same loop, but replace "real-world verifiers" with "trained reward classifier" + periodic human recalibration.

---

## Key Practical Takeaways for Unsloth Work

1. **All post-training is distribution shaping.** When you run DPO, you're concentrating probability mass — not teaching new knowledge.
2. **Data quality > algorithm > model size.** Paper 13's 12:1 finding: parameterization choice matters 12× more than objective choice.
3. **What you exclude matters as much as what you include.** Curate by exclusion first — remove noise, repetition, style inconsistency.
4. **Small models need different strategies.** Below ~3B: distillation beats online RL. Above ~7B: GRPO with exploration.
5. **LoRA is masking your methods' potential.** If you have VRAM, try full fine-tuning. The 12:1 gap means every LoRA comparison understates algorithm potential.
6. **Don't reuse SFT data for GRPO.** +10.4pp penalty from overlap. Partition data pools.
7. **You're bounded by your teacher.** No demonstrated path to super-LLM through AI feedback alone — unless you interface with verifiable reality.

---

## Files on Disk

```
~/work/llm-fundamentals/
├── LEARNING-GUIDE.md          # 6-module learning path
├── synthesis.md               # Structured synthesis (raw)
├── html/
│   ├── index.html             # Landing page with 4-tier reading order
│   ├── synthesis.html         # 18 insights + 7 open questions (peer-reviewed)
│   ├── 01-illustrated-transformer.html
│   ├── 02-tinystories.html
│   ├── 03-phi-1-textbooks.html
│   ├── 04-smollm2.html
│   ├── 05-flan-collection.html
│   ├── 06-lora.html
│   ├── 07-qlora.html
│   ├── 08-lima.html
│   ├── 09-self-instruct.html
│   ├── 10-dpo.html
│   ├── 11-zephyr.html
│   ├── 12-orpo.html
│   ├── 13-sft-dpo-interaction.html
│   ├── 14-deepseekmath.html
│   ├── 15-sft-grpo-overlap.html
│   ├── 16-phi-3.html
│   ├── 17-qwen3.html
│   └── 18-unsloth-dpo-guide.html
└── papers/                    # Original paper summaries (markdown)
    ├── 00-INDEX.md
    └── 01-18 individual files
```

---

## Next Steps to Explore

1. **Train a creative writing quality classifier** — Human + GPT-4 calibration → small model reward scorer
2. **Run GRPO on 0.5B model** — Using the trained classifier as reward signal
3. **Test the 4-model experimental design** — SFT-only vs DPO vs GRPO-LLM-judge vs GRPO-classifier
4. **Measure Goodhart's Law effects** — Does the 0.5B model learn to game the classifier?
5. **Iterate with periodic recalibration** — Fresh human evaluations, adversarial examples

**Research question:** "Can a small calibrated reward model act as a practical verifier proxy for subjective generation, enabling GRPO-style online improvement in tiny LMs?"

---

## Hardware Notes

- **RTX 2070 Super (8GB):** Qwen3.5-4B in INT4, LoRA r=16, 32K context. Fine-tune + test + iterate fast.
- **M3 MacBook Pro (36GB):** Qwen3.5-7B or 14B in INT4. GRPO experiments, complex tool use, multi-domain composition.
- **GRPO compute cost:** G=64 samples × 0.5B model = cheap. G=64 × 7B = expensive. Start small.
