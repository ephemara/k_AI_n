# RESEARCH PILOT 5: DSP / Signal Theory Post-Training Model Pruner & Harmonic Compressor ("TurboPrune" / "kain-prune")

**Status:** ARCHITECTURAL SPECIFICATION & RESEARCH BLUEPRINT  
**Target Substrate:** KAIN Systems Language (`core.exe` portable standalone binary < 3MB)  
**Parent Ecosystem:** `k_AI_n` / `TurboKain`  
**Location:** `D:/k_AI_n/research/research_5_pruner.md` (Mirror: `Y:/k_AI_n/research/` | Local: `T:/KainProjects/k_AI_n/research/`)

---

## 1. The Core Thesis: Neural Networks as Cascaded Digital Filter Networks

Modern machine learning treats Deep Transformers (LLMs like LLaMA/Qwen and DiTs like Wan 2.1 / Flux) as abstract computational graphs executing floating-point tensor contractions. Compression efforts in the open-source community are almost exclusively limited to **Scalar/Grouped Weight Quantization** (FP16 → INT8 → INT4 GGUF/AWQ/EXL2).

**The Quantization Flaw:**
Quantization reduces the memory footprint of individual numbers, but:
1. It does **not** reduce arithmetic complexity ($O(N^2)$ attention and dense GEMM FLOPs remain unchanged).
2. It does **not** reduce layer-to-layer execution latency.
3. On memory-constrained devices (e.g. 6GB VRAM running a 9GB Q4 Wan 14B checkpoint), it fails to prevent catastrophic PCIe paging stalls (~1.8s per step just shuttling weights across the bus).

**The DSP Breakthrough:**
A Transformer block with residual connections:
$$x_{l+1} = x_l + \text{MLP}(\text{Attention}(x_l))$$
is formally equivalent to a **Cascaded Multi-Channel IIR (Infinite Impulse Response) Filter Bank with Non-Linear Side-Chain Modulation**.

Instead of treating the model as a statistical black box requiring hundreds of brute-force GPU forward passes over arbitrary datasets, we analyze the **frequency response, transfer function ($H(\omega)$), information entropy velocity, and quadratic phase coupling** of each block directly.

---

## 2. Triangulation: The Three Converging Disciplines

This instrument exists at the exact intersection of three mature technical fields that have historically never spoken to each other:

```
                  [ Audio Engineering (10+ yrs) ]
                   - Transfer functions H(ω)
                   - All-pass / unitary gain detection
                   - Harmonic phase distortion & mixing
                                 \
                                  \
[ Radio Astronomy / SETI (20+ yrs) ] ─── [ Systems Programming / Demoscene (Kain) ]
 - Cyclostationary noise extraction        - Zero-GC Arena Allocations (+32 rule)
 - Bispectrum Quadratic Phase Coupling      - Win32 kernel32 DMA Direct IO
 - Permutation Entropy / LZ76 complexity    - AVX2 / SIMD `converge` register execution
 - Weak signal recovery in noise floors     - Standalone, zero-dependency 2MB binaries
```

---

## 3. The 4 Analytical Lenses (The Instrument Battery)

Following the proven architectural discipline of **TurboKain** (where 26 distinct signal instruments each evaluate raw baseband and emit formal proof receipts), the model pruner deploys a multi-lens battery against the unquantized or quantized weight matrices and layer activations:

### Lens 1: Spectral Transfer Function & Eigenvalue Attenuation ($H(\omega)$)
- **Concept:** In an audio processing chain, an EQ stage set to flat 0 dB with negligible phase shift is a no-op that wastes processing cycles.
- **Application:** For each Transformer block $l$, compute the singular value spectrum of the residual delta:
  $$\Delta W_l = W_{\text{out}} \cdot W_{\text{in}}$$
- **Verdict:** If the spectral radius $\rho(\Delta W_l) < \epsilon$ and condition number $\kappa \approx 1$, the block acts as an **All-Pass / Identity Filter** across the operational latent band. It is safely prunable with zero degradation of semantic geometry.

### Lens 2: Permutation Entropy Velocity ($dH/dl$ & LZ76 Algorithmic Complexity)
- **Concept:** Derived directly from TurboKain’s `perm_entropy` instrument. Distinguishes thermal Gaussian noise from structured information using ordinal patterns and Lempel-Ziv complexity.
- **Application:** Measure the rate of information entropy change as latent vectors pass through layers $l = 1 \dots L$:
  $$v_H(l) = \frac{d H_{\text{perm}}(x_l)}{d l}$$
- **Verdict:** Early layers exhibit rapid entropy collapse (structural geometric lock-in). Late layers exhibit fine-grained texture modulation. Middle layers often show $v_H(l) \approx 0$—indicating computational idling where weights merely recirculate data without adding entropy or rejecting noise.

### Lens 3: Bispectral Analysis & Quadratic Phase Coupling (QPC)
- **Concept:** Derived from TurboKain’s `bispectrum` (HOSA-QPC). Identifies non-linear phase locking between frequency components $(f_1, f_2 \to f_1 + f_2)$ while perfectly canceling Gaussian noise.
- **Application:** Critical for **Multi-Reference blending** (e.g. Klein Flux 9B combining Image 1 + Image 2) and **LoRA integration**. Cross-attention layers operate as modulators/mixers.
- **Verdict:** Distinguishes layers that achieve true harmonic phase coupling of reference features from layers suffering from destructive intermodulation distortion (the root cause of visual hallucinations, ghosting, and latent artifacts).

### Lens 4: Fractional Fourier Chirp Matched Filtering (FrFT)
- **Concept:** Derived from TurboKain’s `frft_hunt`. Evaluates non-stationary trajectories in the time-frequency $(t, f)$ plane.
- **Application:** Diffusion sampling trajectories are smooth, non-linear flow curves over timesteps $t \in [1 \dots T]$. High-order layers are only active at specific fractional rotation angles (timesteps).
- **Verdict:** Drives **Dynamic Step-Adaptive Layer Skipping**—proving that blocks $B_{\text{even}}$ can be bypassed during steps $t \in [2, 3, 4]$ while only executing on step 1.

---

## 4. Software Architecture: The Kain Implementation

The tool is implemented in **KAIN**, compiled to a standalone executable via LLVM (`turboprune.exe` / `core.exe`), completely decoupled from Python, PyTorch, CUDA runtime installers, or C++ build systems.

### A. Memory & IO Architecture (TurboKain DNA)
1. **Direct OS DMA (`kernel32`):**
   - Utilizes `k_CreateFileA`, `k_SetFilePointer`, and `k_ReadFile` to memory-map or stream raw GGUF / Safetensors structures.
   - Bypasses C runtime overhead; achieves measured read speeds of **2.75 GB/s to 5.4 GB/s**.
2. **Arena Allocation with the `+32` Byte Slack Rule:**
   - Zero garbage collection. Memory is managed via aligned arena allocations:
     `alloc_zeroed(n + 32, "Byte")`
   - Guarantees safe AVX2 256-bit SIMD over-reads in inner loops without page fault boundaries.
3. **Register-Resident Math (`converge` Blocks):**
   - Inner-loop dot products, cosine metrics, and QuickSelect medians execute inside dual-path `converge` blocks (spec table vs AVX2 vectorized hardware lanes).

### B. Execution Modes
```bash
# 1. Profile: Scan weights and output mathematical health & redundancy catalog
turboprune profile --model wan2.1-14b-Q4_K_M.gguf --gpu 0

# 2. Search: Pareto-frontier optimization for target VRAM / Latency budget
turboprune search --model wan2.1-14b-Q4_K_M.gguf --target-vram 5.5GB --metric dsp-entropy

# 3. Slice: Emit physically pruned, smaller GGUF for vanilla runtimes
turboprune slice --model wan2.1-14b.gguf --recipe prune_recipe.tsv --out wan2.1-11b-turbo.gguf

# 4. Schedule: Emit dynamic step-skipping JSON recipe for ComfyUI / vLLM
turboprune schedule --model wan2.1-14b.gguf --steps 4 --out step_recipe.json
```

---

## 5. Concrete Impact: The Wan 2.1 14B Target

Applying this architecture to Wan 2.1 14B on consumer hardware (Quadro RTX 3000, 6GB VRAM):

1. **Current State:**
   - 40 DiT blocks, 8.9GB Q4 weights.
   - Cannot fit in 6GB VRAM.
   - Thrashes PCIe 3.0 x16 on every sampling step (36GB total bus traffic).
   - Generates 5s video in 60–90 seconds.

2. **Target State via DSP Pruning:**
   - 8 provably redundant layers excised via Lens 1 & Lens 2 ($H(\omega)$ and $dH/dl$).
   - Model parameter count drops from 14B → **11.2B**; file size drops from 8.9GB → **~6.8GB**.
   - With dynamic step-skipping (Lens 4), steps 2–4 execute on a 20-block core.
   - Fits resident in VRAM / minimal PCIe paging.
   - **Generation time drops to 12–20 seconds on 6GB silicon with 98% visual fidelity.**

---

## 6. Strategic Role in the `k_AI_n` Roadmap

`TurboPrune` serves as the immediate tactical bridge between **TurboKain** (shipped radio-astronomy success) and **`k_AI_n`** (sovereign native intelligence engine):

- **Validation:** Proves Kain's tensor parsing, DMA IO, and mathematical SIMD kernels on industry-standard GGUF / Safetensors weights.
- **Precursor to FWHT:** The spectral filter analysis directly lays the mathematical foundation for `k_AI_n`'s zero-matMul Fast Walsh-Hadamard Transform engine.
- **Immediate Utility:** Solves the acute VRAM wall for millions of local AI users today while demonstrating the undeniable speed and elegance of the Kain language to the broader systems engineering community.
