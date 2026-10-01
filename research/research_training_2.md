# Research Training 2: The Scavenger King Training Protocol & Asymmetric Token Refinery
**High-Entropy Training Topology: Beating Brute-Force GPU Clusters with Signal-Processing Rigor**  
*Document Version: 1.0 — Pilot Phase*  
*Location: `d:/k_AI_n/research/research_training_2.md`*  
*Status: IN PROGRESS — Working idea and living operational protocol; subject to ongoing refinement, empirical validation, and tactical tweaking.*  
*Companion Document: `d:/k_AI_n/research/research_pilot_1.md`*

> **OPERATIONAL NOTICE:**  
> This document details the data engineering, distillation pipeline, and hardware allocation strategy for training the Alien Greenfield Engine without multi-thousand-dollar H100 GPU clusters. All throughput models, dataset ratios, and distillation schedules are active working targets subject to empirical benchmarking on local and cloud silicon.

---

## 1. The Scavenger King Philosophy: Signal Over Thermal Noise

Corporate artificial intelligence research has replaced engineering rigor with raw capital expenditure:
* Massive labs rent 10,000 H100 clusters to ingest 15 Trillion tokens of raw, unparsed web crawl (Common Crawl).
* **The Reality:** 90% of that data is semantic thermal noise—repetitive boilerplate, automated SEO spam, cookie banners, minified JS, flame wars, and grammar noise.
* Millions of GPU hours are wasted computing gradient updates on tokens that have near-zero Kolmogorov complexity.

**The Scavenger Stance:**  
Scavenging does not mean compromising quality; it means **flipping the problem on its head and out-engineering the brute-forcers.**

In the sibling repository next door (**TurboKain**), a 26-instrument digital signal engine processes hundreds of gigabytes of telescope baseband data to extract coherent non-linear signals buried under thermal noise. 

If we apply that exact same signal-processing doctrine to training data:
$$\mathbf{20\text{ Billion tokens of high-entropy, AST-verified logic}} \gg \mathbf{2\text{ Trillion tokens of raw web noise}}$$

A 14B model trained exclusively on pure signal out-reasons a 70B model drowned in Internet sludge.

---

## 2. The Asymmetric Three-Tier Hardware Topology

Rather than trying to rent a monolithic, expensive GPU cluster, we decouple data mining, AST compilation, synthetic execution, and matrix backpropagation across an asymmetric hardware pipeline:

```
┌────────────────────────────────────────────────────────────────────────┐
│  TIER 1: THE INGEST CITADEL (Production VPS — 1TB+ NVMe, High-Speed IO)│
│  • aria2c 16-connection accelerated mass mirroring of apex code repos  │
│  • TurboKain signal-processing entropy screener (LZW / AST filters)    │
│  • Continuous packager generating contiguous binary .kain_bin streams  │
└──────────────────────────────────┬─────────────────────────────────────┘
                                   │ (Verified Clean Code & Docs)
                                   ▼
┌────────────────────────────────────────────────────────────────────────┐
│  TIER 2: THE THOUGHT FACTORY (Free-Trial Azure VPS CPU Swarm)          │
│  • Multi-core CPU nodes running continuous AST & symbol parsing        │
│  • Sandboxed symbolic execution: generating causal execution traces    │
│    (Input State -> Register Mutations -> Output State)                │
│  • CPU-quantized teacher models generating synthetic reasoning proofs  │
└──────────────────────────────────┬─────────────────────────────────────┘
                                   │ (Memory-Mapped Packed Binary Tensors)
                                   ▼
┌────────────────────────────────────────────────────────────────────────┐
│  TIER 3: THE FORGE & CANARY (Local RTX 4090 + Quadro RTX 4000)         │
│  • RTX 4090: 100% duty-cycle matrix backpropagation & 1.58-bit QAT     │
│    ↳ Consumes pre-tokenized .kain_bin tensors at memory bandwidth      │
│  • Quadro RTX 4000 (8GB): The Ground-Truth Deployment Canary           │
│    ↳ Validates native Kain core.exe execution, shaders, and arenas     │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Tier 1: The TurboKain Data Refinery (Shannon Entropy Filtering)

We repurpose TurboKain’s complexity and anomaly detection instruments (`perm_entropy.kn`, `xvm_sandbox.kn`, `bispectrum.kn`) into an automated code screener:

1. **The Ingest Target (Apex Technical Corpus):**
   * Linux kernel core subsystems, `musl` libc, and `glibc`.
   * Compiler backends: LLVM IR generators, SPIR-V, PTX, and Cranelift.
   * Native graphics engines: Direct3D 12, Vulkan, and metal shader libraries.
   * Formal verification code: Lean4 math libraries, Coq proofs, Z3 SMT-LIB2 packs.
   * TurboKain’s 26 DSP instruments (proven native signal mathematics).
   * High-signal GitHub repositories (filtered by star count, commit depth, and test coverage).

2. **The 3-Gate Refinery:**
   * **Gate 1 (Syntax / Compiler Verification):** Every snippet must pass through a native compiler/tree-sitter pass. If it does not parse into a valid AST, it is instantly discarded.
   * **Gate 2 (Kolmogorov Complexity / LZW Screening):** Repetitive boilerplate, autogenerated code, and duplicate templates compress heavily under LZW. We reject code with low permutation entropy ($H_{PE} < 0.60$).
   * **Gate 3 (Semantic Density Screener):** Code with high cyclomatic complexity, SIMD vectorization, explicit memory bounds, and pointer manipulation is promoted to the highest priority tier.

---

## 4. The 3 Scavenger Super-Hacks

### Hack #1: "Vocabulary Slicing" (Zero-Cost Weight Surgery)
Existing open-weights models (Qwen-2.5-Coder-1.5B/7B, SmolLM2-1.7B) carry 128,000 to 150,000 dictionary entries.
* **The Technique:** Scan the tokenizer vocabulary against our target English + Code + Math corpus. Identify the top 16,384 tokens.
* **The Operation:** Physically truncate the embedding table and final projection layer (LM-head) from 128k down to 16k.
* **The Win:** Immediately reclaims **~60% of the parameter overhead** in the input/output layers without losing any coding competence or English grammar.

### Hack #2: Distillation into 1.58-Bit (Not From Scratch)
* Pre-training a model from random Gaussian weights takes trillions of tokens.
* **Distillation-Aware Quantization (QAT):** We initialize the student model’s structural weights directly from the sliced open-weights foundation, then train the ternary projection layers ($\{-1, 0, +1\}$) using knowledge distillation.
* **Tokens Required:** Only **2 to 5 Billion tokens** instead of 2 Trillion!
* **Timeline on a single RTX 4090:**
  * 4090 Training Throughput: ~45,000–50,000 tokens/sec (sequence length 2,048, packed INT8/FP8).
  * 3 Billion tokens $\div$ 45,000 tokens/sec $\approx$ **66,000 seconds = ~18.5 hours!**
  * A full 1.5B ternary distillation run completes over a single weekend for **$0 in cloud compute**.

### Hack #3: The Azure CPU "Synthetic Teacher Swarm"
* Raw code shows *what* was written, but not *how the machine reasoned through it*.
* We deploy CPU-quantized teacher models (Qwen-2.5-32B or Llama-3.3-70B via quantized CPU inference) across our free Azure VPS instances.
* The swarm runs 24/7 generating:
  1. Step-by-step bug triage and invariant repairs.
  2. Execution traces: variable values after each loop iteration.
  3. Formal mathematical proofs explaining why an algorithm terminates.
* Output is written directly into packed `.kain_bin` files.

---

## 5. Memory-Mapped Binary Streaming (`.kain_bin`)

Traditional PyTorch training suffers from Python data-loader bottlenecks: unpickling tensors, shuffling on CPU, and copying over PCIe while the GPU sits at 60% utilization.

In the Kain Scavenger pipeline:
* Data is pre-tokenized on the VPS into raw binary files (`.kain_bin`) aligned to 64-byte cache lines with `+32` byte SIMD safety buffers.
* The local training harness memory-maps these files directly.
* Utilizing Kain’s **77x faster `file_read`** and pinned DMA buffers, batches stream directly into GPU register SRAM.
* **GPU Utilization Target:** **> 98% continuous Tensor Core saturation.**

---

## 6. Invariant-Weighted Loss Function

In human pre-training, every token receives identical loss weighting. A comma is penalized the same as a critical pointer offset.

Under the Alien Scavenger Protocol, loss is scaled by semantic criticality:

$$\mathcal{L}_{\text{total}} = \sum_{t=1}^{T} w(x_t) \cdot \mathcal{L}_{\text{CE}}(y_t, \hat{y}_t)$$

| Token Classification | Weight ($w$) | Examples |
|---|---|---|
| **Structural Invariants** | **$5.0\times$** | Pointer dereferences, array bounds, memory offsets, types |
| **Control Flow & Invariants** | **$3.0\times$** | `while`, `converge`, `law`, `stage`, loop boundary conditions |
| **Logic & Math Operators** | **$2.0\times$** | Bitwise shifts, popcounts, arithmetic formulas, modulus |
| **Syntactic Glue / Whitespace**| **$0.2\times$** | Indentation, trailing commas, closing delimiters |

This forces the ternary weights to prioritize the logical backbone that prevents undefined behavior, segfaults, and logical fallacies.

---

## 7. The Stepping-Stone Ladder

```
[ Step 0: 150M Proof of Alien Life ]
  ↳ Parameter Count: ~150 Million (8 layers, 1.58-bit)
  ↳ Training Time: ~18 hours on Quadro RTX 4000 or single 4090
  ↳ VRAM Footprint: ~38 MB (.kain_weights)
  ↳ Purpose: Proves Kain compute shader GEMM, arenas, and binary runtime end-to-end.

[ Step 1: 1.5B Scavenger Coder ]
  ↳ Parameter Count: 1.5 Billion (Mamba-2 / SWA Hybrid, 16k Vocab)
  ↳ Distillation: 3–5B high-entropy tokens on 1x RTX 4090 (~2–3 days)
  ↳ VRAM Footprint: ~380 MB in VRAM (runs effortlessly on Quadro RTX 4000)
  ↳ Purpose: High-speed, flawless systems coder (Kain, C, Rust, Shaders, Math).

[ Step 2: 7B / 14B Flagship Engine ]
  ↳ Parameter Count: 7B to 14B (Full Multimodal Latent Flow Matching)
  ↳ Training: Scaled asymmetric distillation over 15–20B tokens using pooled 4090s
  ↳ VRAM Footprint: 3.50 GB (Cleanly fits inside 6GB VRAM target)
  ↳ Purpose: Complete autonomous alien engine (Text, Code, 0.8s Images, 10s Video).
```

---

## 8. Summary of Tactical Moves

1. **VPS Ingestion:** Spin up background scrapers on the 1TB+ VPS using TurboKain’s `aria2c` configuration.
2. **Refinery Implementation:** Build a fast Kain script (`refinery.kn`) to run LZW/entropy checks and AST validation over mirrored repositories.
3. **Binary Packaging:** Stream verified code directly into `.kain_bin` format.
4. **Local Matrix Forge:** Point the local 4090 at the binary token stream for zero-friction distillation.

*(This protocol is active and in development. Metrics and schedules will be updated as empirical benchmarks land.)*
