# Research 3: Theoretical Codebase Architecture & Modular Inventory
**Engineering Specification: A 32,000-Line Zero-Dependency Single-Binary Engine**  
*Document Version: 1.0 — Pilot Phase*  
*Location: `d:/k_AI_n/research/research_3_structure.md`*  
*Status: IN PROGRESS — Working codebase layout and module inventory; modeled directly on the TurboKain whole-program amalgamation doctrine.*  
*Companion Documents: `research_pilot_1.md`, `research_training_2.md`*

> **OPERATIONAL NOTICE:**  
> This specification maps out every module, file, interface boundary, and estimated line count required to build **`k_AI_n`** from zero to a single compiled native binary (`core.exe` < 5MB). Like TurboKain, the architecture separates development into clean, domain-isolated modules, then amalgamates into a single translation unit for whole-program LLVM optimization.

---

## 1. The Amalgamation Blueprint (The TurboKain Doctrine)

TurboKain proved that **30,000+ lines of high-performance Kain code** across 26 instruments can be written cleanly in isolated modules, fused via `kain amalgamate --raw`, and compiled into a single **1.97 MB native `.exe` with zero runtime dependencies.**

We apply the identical build doctrine to **`k_AI_n`**:

```
kain/core/*.kn  ──►  kain amalgamate --raw kain/core -o kain/core.kn  ──►  kain build kain/core.kn  ──►  core.exe (~3.5 MB)
 (6 Subsystems, 26 Modules, ~32k LOC)                                      (Whole-Program LLVM)          │
                                                                                                        ├── core chat
                                                                                                        ├── core code
                                                                                                        ├── core image <in> [prompt]
                                                                                                        ├── core video <in> [prompt]
                                                                                                        ├── core cartridge <mount>
                                                                                                        ├── core distill <data>
                                                                                                        └── core prove
```

### Architectural Principles:
1. **Zero Runtime Dependencies:** Direct OS boundary linking (`kernel32` / OS handles). No Python, no MSVC redistributables, no CUDA toolkit DLLs.
2. **Whole-Program Optimization (WPO):** Amalgamating into `core.kn` exposes the entire neural call-graph to LLVM for inter-procedural inlining, loop vectorization, and dead-code elimination.
3. **Contiguous Padded Arenas:** Tensors carve from statically bounded `Byte` arenas with `+32` byte SIMD padding (`alloc_zeroed(n + 32, "Byte")`), using `collapse`, `observe`, and `decay`.
4. **Multi-Call Dispatch:** The binary can be invoked as subcommands (`core chat`) or hardlinked directly to tool stems (`chat.exe`, `image.exe`).

---

## 2. Comprehensive Module Inventory & Line-Count Estimates

```
================================================================================
k_AI_n CODEBASE ESTIMATE: 26 MODULES / ~32,000 LINES OF PURE KAIN
================================================================================
```

### Subsystem 1: Core Host Runtime, Memory & CLI (~5,000 LOC)
* **`_common.kn` (~1,200 LOC):**
  * Contiguous arena memory allocation (`alloc_zeroed` with $+32$ SIMD safety).
  * High-speed pointer window loaders (`mem_load`, `mem_store`), byte-endian packing.
  * Formal contract assertions, Z3 invariant verification hooks, fatal abort handlers.
* **`config.kn` (~1,000 LOC):**
  * Hardware capability resolution (VRAM capacity, Tensor Core support, SIMD width).
  * Architecture hyper-parameters (hidden dim, layer count, equilibrium tolerance, SWA window).
  * Command-line argument parsing and configuration merging.
* **`dispatch.kn` (~1,600 LOC):**
  * Entry-point routing (`GetCommandLineA()` hook).
  * Multi-call execution router and command help dispatcher.
* **`mmap_loader.kn` (~1,200 LOC):**
  * Direct OS kernel file handles for high-throughput weight ingestion (**77x faster `file_read`**).
  * Zero-copy DMA streaming of pre-tokenized binary files (`.kain_bin`).

---

### Subsystem 2: Linguistic Transceiver & Tokenization (~4,000 LOC)
* **`tokenizer_16k.kn` (~1,500 LOC):**
  * Specialized 16,384-token English + Code + Math byte-pair BPE tokenizer.
  * Direct UTF-8 byte stream to packed integer array conversion with zero heap allocations.
* **`transceiver_pack.kn` (~1,200 LOC):**
  * Modular spoken language adapter loader (`lang_english`, `lang_japanese`).
  * In-SRAM projection matrices converting text tokens to high-dimensional semantic vectors.
* **`prompt_steward.kn` (~1,300 LOC):**
  * Context window formatting, special marker injection (`fn`, `struct`, `shader`, `proof`).
  * Rolling sliding-window history buffer management.

---

### Subsystem 3: Tensor Math & Zero-MatMul Kernel Suite (~6,500 LOC)
* **`fwht_butterfly.kn` (~1,800 LOC):**
  * Fast Walsh-Hadamard Transform butterfly recursive addition network ($O(N \log N)$ complexity).
  * Zero-multiplication forward pass in GPU register SRAM.
* **`hadamard_sdm.kn` (~2,000 LOC):**
  * Sub-bit fractional weight entanglement (0.65 bits/parameter) decoding.
  * Interleaved bit-plane reconstruction and Z3-synthesized projection matrices.
* **`gemm_158b.kn` (~1,500 LOC):**
  * Packed ternary $\{-1, 0, +1\}$ matrix dot-product kernels with Tensor Core (`DP4A` / `MMA`) acceleration.
  * `converge` spec + AVX2 / GPU fast lanes.
* **`activations.kn` (~1,200 LOC):**
  * Fused RMSNorm, SwiGLU / GeGLU non-linearities, RoPE positional rotary embeddings.
  * High-performance temperature-scaled top-$p$ / top-$k$ nucleus sampler.

---

### Subsystem 4: Recurrent Hybrid & Equilibrium Model (~8,000 LOC)
* **`ssm_liquid.kn` (~2,200 LOC):**
  * Linear State-Space Model recurrent update ($h_t = A h_{t-1} + B x_t$) in $O(1)$ constant memory.
  * Cache-line optimized streaming state recurrence (128k+ token context in ~150 MB).
* **`swa_attention.kn` (~1,800 LOC):**
  * Local Sliding-Window Attention kernel (window size = 1,024).
  * Exact associative recall for syntax symbols, variable names, and closing braces.
* **`deq_equilibrium.kn` (~1,500 LOC):**
  * Monotone Operator fixed-point equilibrium solver ($x^* = f_\theta(x^*, u)$).
  * Dynamic adaptive iteration depth (2 iterations for simple tokens, up to 32 for complex proofs).
* **`moe_micro_basis.kn` (~1,500 LOC):**
  * Combinatorial micro-expert router ($\binom{512}{8} \approx 4.8 \times 10^{16}$ super-expert states).
  * Micro-shard superposition and dynamic SRAM fusion.
* **`cartridge_engine.kn` (~1,000 LOC):**
  * Hot-swappable 15MB domain cartridge mounting ($W_{\text{active}} = W_{\text{core}} + \Delta W_{\text{cartridge}}$) in $< 1\text{ ms}$.
  * In-memory stacking of multiple domain adapters (e.g. C++ + Vulkan).

---

### Subsystem 5: Unified Multimodal Latent Flow Engine (~5,500 LOC)
* **`dcae_autoencoder.kn` (~1,800 LOC):**
  * 32x Deep Compression Spatial Autoencoder (512×512 $\to$ 16×16×64 latent grid).
  * Slashes spatial token count from 4,096 down to 256.
* **`rectified_flow.kn` (~1,500 LOC):**
  * 4-step straight-line ODE Euler integrator for photorealistic image generation in ~0.8 seconds.
* **`motion_wavelet.kn` (~1,200 LOC):**
  * Temporal motion velocity vector field generator ($\Delta x, \Delta y, \Delta z$) for 16-frame video clips.
* **`render_png.kn` (~1,000 LOC):**
  * Native, zero-dependency PNG encoder (derived from TurboKain’s `waterfall.kn`).
  * Direct in-memory rasterization to disk with zero external image libraries.

---

### Subsystem 6: The Scavenger Refinery & Distillation Suite (~3,000 LOC)
* **`entropy_screener.kn` (~1,000 LOC):**
  * Repurposed TurboKain `perm_entropy` / LZW complexity filters.
  * Discards low-entropy boilerplate, HTML, minified JS, and repetitive templates.
* **`ast_verifier.kn` (~800 LOC):**
  * Syntax tree compiler validation gates (ensuring only compilable code enters training).
* **`binary_packer.kn` (~600 LOC):**
  * Formats clean code into contiguous, 64-byte aligned `.kain_bin` memory-mapped streams.
* **`distill_loss.kn` (~600 LOC):**
  * Invariant-weighted loss calculator ($5.0\times$ weight on pointer arithmetic, bounds, types).

---

## 3. Physical Directory Layout

```
d:/k_AI_n/
├── research/
│   ├── spec.txt                  # Original greenfield vision
│   ├── research_pilot_1.md       # Maximum ceiling architecture (v2.0)
│   ├── research_training_2.md    # Scavenger training protocol & VPS refinery
│   └── research_3_structure.md   # This specification (32k LOC codebase layout)
│
├── src/
│   ├── core/                     # Subsystem 1: Runtime, Memory, CLI
│   │   ├── _common.kn
│   │   ├── config.kn
│   │   ├── dispatch.kn
│   │   └── mmap_loader.kn
│   │
│   ├── tokenizer/                # Subsystem 2: 16k Vocab & Transceivers
│   │   ├── tokenizer_16k.kn
│   │   ├── transceiver_pack.kn
│   │   └── prompt_steward.kn
│   │
│   ├── tensor/                   # Subsystem 3: FWHT, 1.58b GEMM & Act
│   │   ├── fwht_butterfly.kn
│   │   ├── hadamard_sdm.kn
│   │   ├── gemm_158b.kn
│   │   └── activations.kn
│   │
│   ├── model/                    # Subsystem 4: SSM, SWA, DEQ & Cartridges
│   │   ├── ssm_liquid.kn
│   │   ├── swa_attention.kn
│   │   ├── deq_equilibrium.kn
│   │   ├── moe_micro_basis.kn
│   │   └── cartridge_engine.kn
│   │
│   ├── flow/                     # Subsystem 5: 32x Latent Flow & PNG
│   │   ├── dcae_autoencoder.kn
│   │   ├── rectified_flow.kn
│   │   ├── motion_wavelet.kn
│   │   └── render_png.kn
│   │
│   └── refinery/                 # Subsystem 6: Entropy Mining & Packaging
│       ├── entropy_screener.kn
│       ├── ast_verifier.kn
│       ├── binary_packer.kn
│       └── distill_loss.kn
│
├── cartridges/                   # 15MB Hot-Swappable Domain Packs
│   ├── lang_english.kain_cartridge
│   ├── lang_japanese.kain_cartridge
│   ├── cpp_systems.kain_cartridge
│   └── vulkan_shaders.kain_cartridge
│
├── build.kn                      # Authority build pipeline & amalgamation task
└── Makefile / build.cmd          # One-line compiler invocation
```

---

## 4. Build & Compilation Flow

To compile the entire intelligence engine into a single binary:

```bat
:: Step 1: Amalgamate all 26 modules into one whole-program translation unit
kain amalgamate --raw src/core src/tokenizer src/tensor src/model src/flow -o src/core.kn

:: Step 2: Compile via LLVM Whole-Program Optimization (WPO)
kain build src/core.kn --target llvm -o core.exe

:: Result: Standalone core.exe (~3.5 MB), zero dependencies, ready to run.
```

---

## 5. Development Velocity Estimate

Based on TurboKain’s proven velocity (30,000+ lines in ~4 days using clean modular `.kn` design + amalgamation):
* **Week 1:** Subsystems 1, 2, and 3 (Runtime, 16k Tokenizer, FWHT/GEMM Kernels).
* **Week 2:** Subsystem 4 (SSM Liquid Recurrence + Sliding-Window Attention + Cartridge Engine).
* **Week 3:** Subsystem 5 (32x Latent Flow Engine + PNG Exporter).
* **Week 4:** Subsystem 6 (Refinery scripts running on VPS) + First 150M End-to-End Distillation.

The mold is complete. We build.
