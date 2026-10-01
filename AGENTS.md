# AGENTS.md — k_AI_n

Onboarding + operating rules for any AI agent working in this repository.
Read this before touching code.

> **START HERE: `research/spec.txt` & `research/research_pilot_1.md`** — the mission.
> This file describes *how to work in this repo* and *how the Kain language works*.
> The research blueprints describe *what we are building and why*: a bare-metal,
> native-compiled multimodal intelligence engine in pure Kain that squeezes
> **14B–70B parameter reasoning (with 120B+ effective cognitive depth)** into
> **6GB VRAM + 16–32GB System RAM**, with zero framework tax and sub-second
> generation.
>
> **TurboKain reference path: `D:/TurboKain`** ← edit this one line per machine/build if it moves.
> TurboKain next door is our production reference code ground truth: 30,000+ lines of real, shipped Kain code
> across 26 instruments proving whole-program amalgamation (`kain amalgamate --raw`), AVX2 SIMD `converge` fast-lanes,
> `alloc_zeroed(n+32, "Byte")` arena memory management, and 77x faster `kernel32` DMA IO.
> When in doubt about syntax, system patterns, or real-world Kain architecture, inspect TurboKain code before guessing.
> As codebases move or bounce around across drives, **this single line is the only path that ever needs updating**.
>
> **Sibling repos:**
> - **TurboKain (`D:/TurboKain`):** The reference production code truth (above).
> - **Kain Compiler (`../kain/`):** The language compiler repo. Source of truth for
>   the language runtime, stdlib, and benchmarks. Read-only reference — never edit it.

---

## 1. What `k_AI_n` Is (The Goal)

`k_AI_n` is a **greenfield native-compiled multimodal intelligence engine** written
entirely in **Kain**. It throws out the decadent corporate AI stack (Python runtime,
PyTorch, CUDA wrappers, $O(N^2)$ attention caches, 250,000-token multilingual matrices)
to build an unlobotomized, sovereign, bare-metal binary (`core.exe` < 5MB).

### The Alien N64 Mindset
Corporate AI burns millions of dollars running 10,000-node H100 clusters to ingest
sludge and re-read quadratic KV-caches on every token.

**The Alien Stance:** If an advanced civilization crash-landed with consumer silicon
(an N64 / a 6GB laptop GPU), they would not write Python or spin up 400MB runtime wrappers.
They would treat silicon as pure physics: **memory bandwidth, cache lines, register
SRAM, and raw bitwise operations.**

### The 4 Core Architectural Primitives

1. **1.58-Bit Ternary Compute ($\{-1, 0, +1\}$) & Zero-MatMul FWHT:**
   - Replaces floating-point matrix multiplications with **Fast Walsh-Hadamard Transform
     (FWHT) butterfly recursive additions** ($O(N \log N)$ complexity) and GPU subgroup
     bitwise popcounts.
   - **Zero floating-point multiplications** in the projection layers. Executes at raw
     L1/L2 SRAM bandwidth.

2. **Sub-Bit Fractional Weight Entanglement (0.65 Bits/Parameter):**
   - Weights are stored as **interleaved bit-planes** using Z3-synthesized Hadamard
     projection matrices and Sparse Distributed Memory (SDM).
   - A single physical bit participates across multiple intersecting projections,
     yielding **46.7 Billion effective parameters inside 3.8 GB VRAM**.

3. **$O(1)$ Continuous Liquid Memory (SSM Recurrence):**
   - Replaces quadratic $O(N^2)$ Transformer Attention KV-caches with Linear State-Space
     Model (SSM) recurrence ($h_t = A h_{t-1} + B x_t$).
   - History compresses into a fixed ~128MB hidden state matrix, providing **infinite
     context length with zero VRAM bloat over long runs**.
   - Paired with Local Sliding-Window Attention (SWA, window = 1,024) for exact
     associative recall of syntax tokens, variable names, and closing braces.

4. **Deep Equilibrium Universal Depth (DEQ Fixed Points):**
   - Instead of 80 distinct static physical layers eating gigabytes, a high-density 7B
     structural core iterates until mathematical equilibrium: $x^* = f_\theta(x^*, u)$.
   - Easy tokens converge in 2 iterations; complex logic, compiler passes, and mathematical
     proofs iterate 16–32 times directly inside GPU register SRAM.
   - Footprint of a 7B model; cognitive depth of a **120B-parameter titan**.

### Beyond Text: Unified Multimodal Latent Flow
Text, code, and vision share the **identical semantic trunk** — no lossy CLIP bottlenecks:
- **32x Deep Compression Spatial Autoencoder (DC-AE):** Compresses $512 \times 512 \times 3$
  images into a compact $16 \times 16 \times 64$ latent grid (256 spatial tokens vs 4,096 in standard diffusion).
- **Rectified Flow (Straight-Line Transport):** Solves the ODE along straight trajectories
  in **4 Euler steps**, generating photorealistic images in **0.6 to 0.9 seconds**.
- **Temporal Motion Wavelets:** Predicts spatio-temporal velocity vectors ($\Delta x, \Delta y, \Delta z$)
  to render 2–4 second video clips in **8 to 12 seconds**.

### Hot-Swappable 15MB Delta Cartridges (`.kain_cartridge`)
Zero catastrophic forgetting:
- The cognitive core is immutable.
- Domain capabilities (C++ Systems, Kain, Vulkan Shaders, Spoken Languages) mount as
  lightweight linear delta matrices in SRAM: $W_{\text{active}} = W_{\text{core}} + \sum \Delta W_{\text{cartridge}}$.
- Hot-swap latency is **< 1 millisecond**. Stackable and decoupled from the base model.

---

## 2. What Kain Is (The Language Substrate)

Kain is a real, shipped systems language — **not** a scripting toy, a DSL, or a research
sketch. It compiles through **LLVM to native `.exe`** (also `.dll`, `.so`, `.obj`, `.a`),
has a full REPL/TUI, and is paranoid by design: **Z3 theorem provers and CBMC formal
assertions are integrated throughout the compiler and runtime** — 500+ Z3 proof packs,
380+ SMT-LIB2 files, 10,000+ CBMC assertions ship with it. The assumption is *all code
is fundamentally broken until mathematically proven otherwise.*

### The Compiler-Owned Semantic Stack (111 Keywords, 8 Layers)

Kain is not Rust; do not write Rust-with-Kain-syntax. The language provides high-level
constructs where the *compiler*, not the programmer, owns state, mutation, dispatch,
timing, coupling, layout, and handoff.

The decision ladder, top-down (stop at the first rung that fits):

```
L7 systems    actor · collapse/observe/decay · spawn/send/on
L6 stones     axiom · shatter · teleport
L5 temporal   pulse · resonate · dampen · every · jitter
L4 stage      orchestrate · stage · deps · policy · residency · transfer
L3 dispatch   converge · spec · fast · capability · verify
L2 integrity  patch · law
L1 authority  world · entangle · surface
L0 plain      fn · struct · let · mut · enum · trait · impl
```

### The Effects Lattice
Functions declare explicit effect signatures:
`with Pure`, `with IO`, `with Async`, `with GPU`, `with Reactive`, `with Unsafe`.
An un-annotated function cannot execute hidden side-effects.

### Dual GPU & Host Architecture
Kain compiles both CPU host code and GPU compute kernels from the **same `.kn` file**:
- `shader compute MyKernel(id: UVec3) -> Void:` — General-purpose GPU compute kernel.
- Uniforms declared via `@N uniform name: Type`.
- The compiler emits SPIR-V / PTX / HLSL bytecode artifacts.
- The CPU host invokes the kernel via `dispatch "MyKernel" [gx, gy, gz]`.

### The Vendored Baseline & Lean TSV Lookups (`docs/kain/`)
Agents do not guess syntax; learn from the organized vendor baseline under `docs/kain/`:

- **`docs/kain/tsv/` — THE LEAN TRUTH TABLES (Grep here first!):**
  36 specialized TSV tables mapping the entire language without prose bloat. Grep these before guessing syntax or stdlib signatures:
  - `docs/kain/tsv/cli_commands.tsv` — Complete CLI commands, options, and flags.
  - `docs/kain/tsv/keywords.tsv` — All 111 keywords, lexical groups, and layers.
  - `docs/kain/tsv/stdlib.tsv` — All standard library functions, arguments, and module paths (`module · symbol · kind · signature · purpose`).
  - `docs/kain/tsv/decision_ladder.tsv` — The architectural decision ladder from L0 to L7.
  - `docs/kain/tsv/shaders.tsv` — Compute shader stages, uniform annotations, and dispatch rules.
  - `docs/kain/tsv/converge.tsv` / `orchestrate.tsv` — Dispatch and stage graph grammar.
  - `docs/kain/tsv/ownership.tsv` — `collapse`, `observe`, `decay`, and arena lifecycles.
  - `docs/kain/tsv/error_codes.tsv` — Compiler error codes, parser, and typechecker diagnostics.
- **Reference Docs & Guides:**
  - `docs/kain/KAIN_BY_EXAMPLE.md` — The language manual with compilable snippets for every feature.
  - `docs/kain/KEYWORDS.MD` — The 111-keyword dictionary and semantic layers in depth.
  - `docs/kain/SHADER_GPU.MD` — Full reference on compute shaders, uniform bindings, and GPU dispatch.
  - `docs/kain/SYSTEMS_PROGRAMMING.MD` — **The Metal Guide**: 100KB bible on raw CPU/OS control — inline asm (`asm("pause")`, `clflush`), hardware fences (`lfence`/`sfence`/`mfence`), cache prefetching (`prefetch_read`), huge pages (`vm_allocate_huge`), NUMA pinning (`set_current_thread_affinity`), `comptime` lookup tables, and the full CRUSHER semantic fusion pattern. TurboKain only scratched the surface; `k_AI_n` taps this metal depth.
- **Code Exemplars & Training:**
  - `docs/kain/examples/CRUSHER.kn` — The semantic singularity benchmark fusing all 8 layers: `shatter struct`, `teleport`, `orchestrate`, `converge`, `world`/`entangle`, `std::actor`, `collapse`/`decay`, and hardware fences (`lfence`/`sfence`).
  - `docs/kain/examples/metal.kn` — 12 raw metal benchmark cases: inline assembly (`asm("pause")`, `clflush`), CPUID/RDTSC intrinsics, virtual memory torture, thread affinity, and callconv dispatch (`@callconv("vectorcall")`).
  - `docs/kain/examples/core_os.kn` — Direct OS syscalls, `std::os::mmap`, page protection, RAM locking (`mlock`), and file IO without C runtime bloat.
  - `docs/kain/training/kain_omni.kn` — One compilable file exercising all layers L0–L7, every effect, stdlib, actors, and telemetry.
  - `docs/kain/examples/fusion_chain.kn` — The definitive multi-layer causal chain voice.
  - `docs/kain/examples/sieve-pattern.kn` — Fast AVX2 `converge` spec pattern over memory buffers.
  - `docs/kain/examples/keyword_crucible.kn` — Exhaustive keyword compiler test harness.
  - `docs/kain/_llm_proto_examples/` — Working Kain prototype implementations of tokenizers, transformers, search kernels, training routines, and config parsers.
  - `docs/kain/_gpu_cpu_examples/` — Real-world pipelines combining `orchestrate`, `converge`, `world`, `entangle`, and compute shaders.
  - `docs/kain/_shader_examples/` — Real-time compute and ray-marching shaders written natively in Kain.
- **`docs/kain/stdlib.kn` — The Amalgamated Stdlib (Search truth here!):**
  148 files / 146 modules packed into a single 1.54 MB raw amalgamation (~39,300 lines).
  Need to see the exact implementation, signature, or return type of any stdlib function?
  Grep this file directly: `grep -n "pub fn os_mlock" docs/kain/stdlib.kn` or `grep -n "pub fn popcount32" docs/kain/stdlib.kn`.
- **`docs/kain/THE_MESSIAH.KN` — The Ultra File:**
  6,029 modules, ~795,000 lines, ~36 MB packed into a single searchable corpus.
  *Never `read` the whole file.* Grep it for production patterns (`grep -n "converge " docs/kain/THE_MESSIAH.KN`).

---

## 3. The "Connect Like Glue" Architecture (Why `k_AI_n` Differs from TurboKain)

In **TurboKain**, 21 digital signal instruments (`slice.kn`, `fam_god.kn`, `boxcar_bank.kn`,
`waterfall.kn`) operate largely as **independent tools**. They read an input file (`.f32`
or `.raw`), execute a DSP pass, and emit a CSV, TSV, or PNG. Their pipeline coupling is
loose, mediated by filesystem artifacts or simple command lines.

In **`k_AI_n`**, this loose coupling is fatal. An artificial intelligence engine is an
interconnected computational organism where **files and modules must connect like glue**:

```
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                                   THE CONNECTIVE GLUE MESH                                  │
├─────────────────────────┬──────────────────────────┬────────────────────────────────────────┤
│ SUBSYSTEM               │ MODULES                  │ IN-MEMORY GLUE HANDOFF                 │
├─────────────────────────┼──────────────────────────┼────────────────────────────────────────┤
│ 1. Runtime & Memory     │ _common.kn               │ Arena memory allocation (+32 SIMD),    │
│                         │ config.kn, dispatch.kn   │ zero-copy OS DMA streaming handles,    │
│                         │ mmap_loader.kn           │ hardware capability arbitration        │
├─────────────────────────┼──────────────────────────┼────────────────────────────────────────┤
│ 2. Linguistic           │ tokenizer_16k.kn         │ Direct UTF-8 byte stream → token IDs   │
│    Transceiver          │ transceiver_pack.kn      │ → high-dimensional semantic vectors    │
│                         │ prompt_steward.kn        │ (carved directly into arena buffers)   │
├─────────────────────────┼──────────────────────────┼────────────────────────────────────────┤
│ 3. Tensor Math &        │ fwht_butterfly.kn        │ Zero-multiplication FWHT butterflies,  │
│    Zero-MatMul Suite    │ hadamard_sdm.kn          │ interleaved bit-plane decoders,        │
│                         │ gemm_158b.kn, act.kn     │ fused RMSNorm & SwiGLU in SRAM         │
├─────────────────────────┼──────────────────────────┼────────────────────────────────────────┤
│ 4. Recurrent Hybrid &   │ ssm_liquid.kn            │ O(1) continuous state updates (128MB), │
│    Equilibrium Engine   │ swa_attention.kn         │ sliding-window attention matrices,     │
│                         │ deq_equilibrium.kn       │ dynamic fixed-point SRAM loop,         │
│                         │ moe_micro_basis.kn       │ 512 micro-shard SRAM superposition,    │
│                         │ cartridge_engine.kn      │ 15MB delta cartridge in-memory fusion  │
├─────────────────────────┼──────────────────────────┼────────────────────────────────────────┤
│ 5. Multimodal Latent    │ dcae_autoencoder.kn      │ Shared semantic trunk vectors →        │
│    Flow Engine          │ rectified_flow.kn        │ 16x16x64 latent grid → 4-step ODE      │
│                         │ motion_wavelet.kn        │ → temporal velocity fields →           │
│                         │ render_png.kn            │ direct in-memory PNG rasterization     │
├─────────────────────────┼──────────────────────────┼────────────────────────────────────────┤
│ 6. Scavenger Refinery   │ entropy_screener.kn      │ Repurposed Shannon/LZW screener →      │
│    & Distillation       │ ast_verifier.kn          │ AST compiler gates → contiguous        │
│                         │ binary_packer.kn         │ 64-byte aligned .kain_bin tensors      │
│                         │ distill_loss.kn          │ → invariant-weighted backpropagation   │
└─────────────────────────┴──────────────────────────┴────────────────────────────────────────┘
```

### The 5 Laws of Glue in `k_AI_n`

1. **Zero-Serialization In-Memory Handoffs:**
   No JSON, no disk caching, no intermediate files between layers. When `tokenizer_16k.kn`
   converts a prompt, the token IDs live in a contiguous arena. `hadamard_sdm.kn` projects
   those tokens into semantic vectors *in the same arena*. `ssm_liquid.kn` updates the
   recurrent state in-place. `deq_equilibrium.kn` iterates the hidden vector in GPU register
   SRAM. Finally, `rectified_flow.kn` or the LM-head reads that exact memory slice.

2. **The `_common.kn` Source of Truth (ASCII Sort Rule):**
   When modules are amalgamated via `kain amalgamate --raw`, files are concatenated in
   ASCII alphabetical order. `_common.kn` begins with an underscore `_` specifically so
   it sorts before all other files (`_` < `a`).
   - All shared arena allocators (`alloc_zeroed(n + 32, "Byte")`), memory load/store
     helpers (`mem_load`, `mem_store`), tensor view structs, hardware capability probes,
     and fatal assertion handlers live here.
   - Every module relies on these primitives; nothing compiles if this foundation shifts.

3. **Unified Memory Arena Contracts (`collapse`, `observe`, `decay`):**
   - Memory is never allocated via loose heap churn (`malloc`/`free` or garbage collection).
   - Tensors and intermediate states carve from bounded memory arenas.
   - Arenas observe strict ownership regions: `collapse` freezes/packs state, `observe`
     creates safe read-only windows across subsystems, and `decay` reclaims the arena
     at the lifecycle boundary. Exactly one `decay` per arena on the success path.

4. **Host-GPU Heterogeneous Glue:**
   - The CPU host (`_common.kn`, `mmap_loader.kn`, `orchestrate`) owns the 32GB System RAM
     warehouse and manages double-buffered asynchronous PCIe DMA streaming.
   - Compute shaders (`shader compute` in `gemm_158b.kn`, `fwht_butterfly.kn`, `rectified_flow.kn`)
     own the 6GB VRAM execution scratchpad.
   - The two communicate through pinned buffer handles and `dispatch` synchronization.
     The GPU never stalls waiting for RAM offloads; prefetching of Layer $N+1$ runs in
     parallel with GPU execution of Layer $N$.

5. **Formal Structural Glue (`law` and `patch`):**
   - Tensor dimensions, alignment strides, quantization ranges, and equilibrium tolerances
     are not polite comments — they are enforced via Kain `law` contracts and CBMC assertions.
   - If `ssm_liquid.kn` outputs a state vector that violates dimension invariants, the
     runtime aborts immediately with a formal proof receipt.

---

## 4. The Flat Core Doctrine (`kain/core/`)

> **WHY FLAT BEATS MICRO-FOLDER SPRAWL:**  
> Deep nested folder trees (`src/a/b/c/d/`) are the enemy of systems engineering: context gets fragmented
> across dozens of editor tabs, relative imports drift, and developers lose track of zero-copy buffer lifecycles.  
> Following the battle-tested TurboKain architecture, **all core neural modules live in a single, flat directory: `kain/core/`**.
>
> 1. **Zero navigation friction:** Every module is an immediate peer in `kain/core/`.
> 2. **Deterministic ASCII Amalgamation:** `_common.kn` sorts first (`_` < `a`), guaranteeing all shared
>    memory allocators, tensor descriptors, and assertions are declared before any module references them.
> 3. **Clean One-Line Build:**
>    ```bash
>    kain amalgamate --raw kain/core -o kain/core.kn
>    kain build kain/core.kn --target llvm -o kain/core.exe
>    ```

```
k_AI_n/ (repo root)
├── docs/                         # Language baseline & vendor guides
│   └── kain/                     # Vendored Kain documentation corpus
│       ├── tsv/                  # 36 LEAN lookup tables (keywords, stdlib, cli, shaders)
│       ├── training/             # kain_omni.kn (all layers L0-L7), HoloGEHv2.md
│       ├── examples/             # CRUSHER.kn, metal.kn, core_os.kn, fusion_chain.kn, sieve-pattern.kn
│       ├── _llm_proto_examples/  # LLM tokenizers, embeddings, transformers, search kernels
│       ├── _gpu_cpu_examples/    # Heterogeneous GPU+CPU pipeline examples
│       ├── _shader_examples/     # Standalone compute/graphics shaders
│       ├── KAIN_BY_EXAMPLE.md    # Language tutorial & compilable snippets
│       ├── KEYWORDS.MD           # 111 keywords & layer mappings
│       ├── SHADER_GPU.MD         # Compute shader & GPU dispatch reference
│       ├── SYSTEMS_PROGRAMMING.MD# Metal guide: asm, fences, prefetch, NUMA, huge pages, CRUSHER
│       ├── stdlib.kn             # Single-file amalgamation of all 148 stdlib modules (grep target)
│       └── THE_MESSIAH.KN        # 795k-line ultra reference corpus (grep only)
│
├── research/                     # Architectural specifications & blueprints
│   ├── spec.txt                  # Original greenfield vision
│   ├── research_pilot_1.md       # Maximum ceiling architecture (14B-70B in 6GB VRAM)
│   ├── research_training_2.md    # Scavenger King training protocol & token refinery
│   ├── research_3_structure.md   # 32k-line codebase engineering layout
│   └── research_4_kain_semantics.md # The Go-To Neural Semantic Stack & Keyword Taxonomy
│
├── kain/                         # Pure Kain Source (NOT src/)
│   ├── core/                     # FLAT Core Suite: All neural modules live here as peers
│   │   ├── _common.kn            # Foundation: Arenas (+32 SIMD), memory windows, assertions
│   │   ├── config.kn             # Hardware probe (VRAM, SIMD), architecture hyperparameters
│   │   ├── dispatch.kn           # Multi-call CLI entry point (core chat/image/video/cartridge)
│   │   ├── mmap_loader.kn        # Zero-copy kernel32 DMA file streaming (77x faster IO)
│   │   ├── tokenizer_16k.kn      # 16,384-token English + Code byte-pair BPE tokenizer
│   │   ├── transceiver_pack.kn   # Spoken language adapter & projection matrices
│   │   ├── prompt_steward.kn     # Sliding window context & AST special marker injector
│   │   ├── fwht_butterfly.kn     # O(N log N) Fast Walsh-Hadamard butterfly network
│   │   ├── hadamard_sdm.kn       # 0.65-bit fractional weight entanglement decoder
│   │   ├── gemm_158b.kn          # 1.58-bit ternary {-1, 0, +1} dot-product kernels
│   │   ├── activations.kn        # Fused RMSNorm, SwiGLU, RoPE, top-p/top-k nucleus sampler
│   │   ├── ssm_liquid.kn         # O(1) continuous state-space recurrence (128MB context)
│   │   ├── swa_attention.kn      # Local sliding-window attention (window = 1,024)
│   │   ├── deq_equilibrium.kn    # Monotone Operator fixed-point solver (2-32 iterations)
│   │   ├── moe_micro_basis.kn    # Combinatorial micro-expert router (C(512,8) states)
│   │   ├── cartridge_engine.kn   # Hot-swappable 15MB domain cartridge mounting (<1ms)
│   │   ├── dcae_autoencoder.kn   # 32x Deep Compression Spatial Autoencoder (16x16x64 grid)
│   │   ├── rectified_flow.kn     # 4-step straight-line ODE Euler integrator (0.8s images)
│   │   ├── motion_wavelet.kn     # Temporal motion velocity vectors (10s video loops)
│   │   ├── render_png.kn         # Native zero-dependency PNG encoder & rasterizer
│   │   ├── entropy_screener.kn   # Shannon entropy / LZW complexity filter
│   │   ├── ast_verifier.kn       # Compiler AST syntax validation gate
│   │   ├── binary_packer.kn      # Contiguous 64-byte aligned .kain_bin tensor stream
│   │   └── distill_loss.kn       # Invariant-weighted loss calculator (5x on pointers/bounds)
│   │
│   ├── spike/                    # Prototyping scratchpad for rapid kernel validation
│   ├── core.kn                   # Unified whole-program raw amalgamation
│   └── core.exe                  # Single compiled native binary (<5MB)
│
├── python/                       # Python scripting & orchestration layer (clean separation)
├── cartridges/                   # 15MB Hot-Swappable Domain Cartridges (.kain_cartridge)
├── memory.tsv                    # Append-mostly change log — EVERY file change gets a row
├── catalog.tsv                   # Module, kernel, and artifact status ledger
└── AGENTS.md                     # This file
```

---

## 5. CLI Commands & Build Workflow

### The Build Commands (Run from Project Root)

```bash
# Typecheck without emitting binary:
kain check kain/core/_common.kn

# Compile a standalone spike or test kernel:
kain build kain/spike/kernel_test.kn --target llvm -o kernel_test.exe

# Step 1: Pack the entire flat core suite into a single whole-program translation unit:
kain amalgamate --raw kain/core -o kain/core.kn

# Step 2: Compile via LLVM Whole-Program Optimization (WPO):
kain build kain/core.kn --target llvm -o kain/core.exe

# Run the single binary:
./kain/core.exe chat
./kain/core.exe code --prompt "fn quickselect..."
./kain/core.exe image "a hyper-detailed retrofuturistic terminal" --out render.png
./kain/core.exe video "fluid vortex in zero gravity" --out loop.png
./kain/core.exe cartridge mount cartridges/cpp_systems.kain_cartridge
./kain/core.exe prove
```

### Two Commands — Do NOT Confuse Them

| Command | What it is | Use it? |
|---|---|---|
| **`kain`** | The **fast** native compiler binary (launches instantly) | ✅ **Always, for all work in this repo** |
| `kaindev` | The Bazel dev auto-sync shim (rebuilds the compiler from source) | ❌ **Never**, unless actively modifying the compiler itself |

**Agents:** Never prepend `.kain/bin` or Bazel directories to PATH. `kain` is on PATH
and works out of the box. If you see `failed to start bazel`, you ran `kaindev` by mistake.

### Stdlib and Runtime Discovery & The Lean Whitelist

Kain resolves standard library modules via `KAIN_STDLIB_PATH` and `$KAIN_HOME/stdlib` (e.g. `D:\kain\stdlib`).
If an imported stdlib module reports `Unknown identifier`, it is an environment discovery issue, not missing syntax. Check `kain doctor`.

#### The Lean Stdlib Whitelist (Don't Overwhelm with 71 Modules!)
Kain ships with 71 standard library modules. **Do NOT drown agents in this ocean.**
For building `k_AI_n`, **only 11 modules actually matter** — led by the GPU/CUDA accelerators:

| Module | Why It Matters for `k_AI_n` | Key Symbols to Use |
|---|---|---|
| **`std::gpu`** | Portable compute shader pipeline, residency & buffer policies | `gpu_device_local_memory_policy`, `gpu_storage_buffer_binding`, `GPU_RESIDENCY_DEVICE_LOCAL` |
| **`std::cuda`** | NVIDIA driver bridge, tensor/neural dispatch & warp intrinsics | `cuda_dispatch`, `CudaDispatchStats`, `CudaBindingLocator`, `cuda_tensor_binding` |
| **`std::machine`** | CPU cache prefetching, hardware fences, NUMA, huge pages, cycles | `prefetch_read`, `lfence`, `sfence`, `vm_allocate_huge`, `rdtsc` |
| **`std::os`** | Memory-mapping `.kain_bin` files, locking RAM, probing hardware | `os_mmap`, `os_mlock`, `os_ram_total`, `os_cpu_count` |
| **`std::bits`** | Hardware bit manipulation for 1.58-bit ternary popcounts | `popcount32`, `clz32`, `ctz32`, `bswap32` |
| **`std::simd`** | 256-bit SIMD vector types and lane operations | `I64x4`, lane add/sub/mask |
| **`std::memory`** | Volatile access, pointer offset manipulation, memory fences | `volatile_load_int`, `load_fence`, `store_fence` |
| **`std::atomic`** | Lockless thread synchronization, atomic flags | `atomic_load_int`, `atomic_store_int`, `atomic_compare_exchange` |
| **`std::math`** | Fast transcendentals, vectors, matrices, float clamp | `sin`, `cos`, `sqrt`, `clamp`, `Vector4` |
| **`std::bytes`** | Raw byte slice handling, binary packing | `ByteSlice`, `BytesBuilder` |
| **`std::fs` / `std::path`** | Small file and path checks (manifests, cartridges) | `fs_exists`, `path_join`, `path_extension` |

#### What to IGNORE (The Noise Filter):
- ❌ **`std::no_std`**: Designed for bare-metal microcontroller kernels / RTOS. We are building a hosted 64-bit Windows binary with Vulkan/DirectX and kernel32 DMA. Do not import `no_std`.
- ❌ **`std::mmio`**: Designed for device-driver register memory. 1.58-bit ternary bitplane unpacking is faster and cleaner with `std::bits` (`popcount32`) and raw shifts.
- ❌ **`std::ui` / `std::graphics`**: Neural inference is headless. Leave UI out of the compute engine (`std::gpu` handles compute, `std::graphics` handles windows/renderpasses).
- ❌ **`std::net` / `std::http` / `std::tls`**: The core binary is zero-network, sovereign local AI.
- ❌ **`std::tar` / `std::zip` / `std::semver`**: Unnecessary build artifact formats.
- ❌ **`std::tar` / `std::zip` / `std::semver`**: Unnecessary build artifact formats.

---

## 6. Ledgers — `memory.tsv` & `catalog.tsv`

We maintain append-mostly ledgers to guarantee absolute provenance, continuity across agents,
and prevent silent drift.

### The Agent Ledger Workflow: CHECK FIRST, LOG AFTER

> **CRITICAL RULE FOR ALL AGENTS:**  
> 1. **CHECK FIRST:** Before touching code or planning an implementation, **inspect `memory.tsv`**
>    (`read` or `grep`). See what previous agents built, tested, changed, or broke.
>    Do not guess the state of the repo — the ground truth is in the ledger.
> 2. **LOG AFTER:** Every file change (created, edited, moved, deleted) **must be logged immediately**
>    using the native Kain helper `scripts/memlog.exe`. No silent edits.

---

### `memory.tsv` — The Change Ledger
Columns: `date  area  type  description  file` (tab-separated, append-only).

- `area`: Subsystem (`core`, `tokenizer`, `tensor`, `model`, `flow`, `refinery`, `docs`, `scripts`, `repo`, `build`)
- `type`: `add`, `update`, `fix`, `build`, `verify`, `scaffold`, `refactor`
- `description`: What changed and **why**, including receipts or benchmark evidence.
- `file`: Affected paths, comma-separated.

#### How to Log: `scripts/memlog.exe`

`scripts/memlog.kn` is compiled to a fast native binary at `scripts/memlog.exe`.
It automatically stamps today's ISO date, sanitizes inputs so newlines or tabs can never corrupt the TSV, and appends to `memory.tsv`.

```bash
# General invocation (run from repo root):
./scripts/memlog.exe <area> <type> "<description>" "<file1,file2>"

# Examples:
./scripts/memlog.exe tensor build "FWHT butterfly passes 1024-vector sanity in 12us" "kain/tensor/fwht_butterfly.kn"
./scripts/memlog.exe tokenizer add "16k BPE vocab mapping with zero-copy arena buffers" "kain/tokenizer/tokenizer_16k.kn"
./scripts/memlog.exe core fix "Added +32 SIMD padding to avoid AVX2 store overrun" "kain/core/_common.kn"
```

If you ever need to rebuild the helper:
```bash
cd scripts && kain build memlog.kn --target llvm -o memlog.exe && cd ..
```

---

### `catalog.tsv` — The Module & Kernel Ledger
Columns: `module  source  target  status  receipt  notes  updated` (tab-separated).
- Tracks status of each of the 26 modules (`draft` → `builds` → `proven` → `fused`).
- **Check `catalog.tsv`** to see which kernels are drafted vs mathematically proven.
- `status=proven` requires an actual test receipt (e.g. `108/108 checks green in 4ms`).
  An empty receipt is unacceptable. Update this ledger whenever a module graduates.

---

## 7. Non-Negotiable Rules

1. **You write programs in Kain; you do NOT modify the Kain compiler.**
   The compiler lives outside this repo at `../kain/`. If code fails to compile, assume your
   Kain code is incorrect first. Blame the compiler only with a minimal isolated repro.
2. **Zero framework tax in the execution path.**
   No Python runtime, no PyTorch wrappers, no CUDA DLL dependencies in `core.exe`.
   The entire engine compiles to a standalone native binary under 5MB.
3. **Files connect like glue.**
   Never create decoupled, orphan modules with incompatible buffer layouts or heap allocations.
   Tensors and states pass in-memory through contiguous arenas defined in `_common.kn`.
4. **Prove before fuse.**
   Every mathematical kernel (FWHT, 1.58b GEMM, SSM recurrence, DEQ fixed point) must have
   an executable test battery in `kain/spike/` with verified numerical receipts before being
   fused into `kain/core.kn`.
5. **Thresholds and dimensions are configuration data.**
   No magic hardcoded numbers scattered in logic loops. Hidden dimensions, layer counts,
   SWA window sizes, and equilibrium tolerances belong in `config.kn`.
6. **No junk web tokens; Shannon entropy & AST validation are mandatory.**
   Training and distillation data must pass through the Tier 1 & 2 refinery. Only high-entropy,
   compilable code with verified ASTs enters `.kain_bin` tensor streams.
7. **`build` is the gate.**
   `kain check` cannot synthesize pointers to verify `converge` blocks over `ptr` parameters.
   Equivalence and compilation verification must pass through `kain build`.
8. **No hardcoded drive letters or absolute paths.**
   Never hardcode `D:/` or `C:/` in committed source. Use relative paths, command-line flags,
   or environment variables (`--data-dir`, `--models-dir`).
9. **`and` and `or` do not short-circuit.**
   Guard argument counts and array bounds with nested `if` statements. Never write
   `if len(arr) > 0 and arr[0] == x:`, which will crash on empty arrays.
10. **Receipts for everything.**
    "It works" is not a status. A status is: "FWHT butterfly passes 1024-vector sanity check;
    0 diff against mathematical identity; executes in 12 microseconds."
11. **Check memory.tsv before work; log with memlog.exe after.**
    Never touch code without reading recent history in `memory.tsv`. Never finish a turn
    without logging your file modifications with `./scripts/memlog.exe`. No silent edits.

---

## 8. Common Pitfalls (Bled-For Knowledge — Do Not Rediscover)

- **`failed to start bazel`:** You invoked the dev shim (`kaindev`). Use plain `kain`.
- **`and` / `or` do not short-circuit:**
  ```kn
  // CRASHES if args is empty:
  if len(args) > 0 and args[0] == "--help": ...

  // CORRECT:
  if len(args) > 0:
      if args[0] == "--help": ...
  ```
- **Bulk arenas require `+32` SIMD safety padding:**
  Bulk memory loops auto-vectorize into AVX2 32-byte stores and will overrun unaligned or
  exact-size buffers by up to 31 bytes, triggering exit code 127.
  **Always allocate with `+32` safety bytes:**
  ```kn
  let buf = alloc_zeroed(nbytes + 32, "Byte")
  ```
- **High-throughput file IO via `kernel32`:**
  Do not use `std::fs` for multi-gigabyte weight or tensor streaming (`fs_read_bytes_range`
  is 1000x slower than memcpy). Use `@extern` `kernel32` handles (`CreateFileA`, `ReadFile`,
  `SetFilePointer`) directly into `ptr<Byte>` arenas (**77x faster `file_read`**).
- **Exact Byte load/store semantics:**
  - Read byte `i` as `Int`: `mem_load(ptr_offset(buf, i, "Byte"), "Int") & 255`
    (Do NOT do `mem_load "Byte" as Int` — that reinterprets an 8-byte word).
  - Store byte `i`: `mem_store(ptr_offset(buf, i, "Byte"), v as Byte, "Byte")` where `v` is 0..255.
- **One `decay` per arena per function:**
  The Kain borrow checker joins branch states. Emitting `decay my_arena` in two branches
  of the same function fails `check` even with early returns. Ensure exactly one lexical
  `decay` site per arena on the success path.
- **Never shadow an arena pointer with a local variable:**
  Declaring `var state: Int` inside a loop when an arena pointer `let state: ptr<Byte>`
  is in scope corrupts ownership tracking, passing `check` but crashing with exit 127 on `decay`.
- **Reserved words that bite:**
  `out`, `share`, `match`, `policy`, `deps`, `fast`, `spec` are reserved keywords.
  Do not use them as parameter names, field names, or local variables. Use `out_val`,
  `share_mode`, `matched`, `policy_cfg`.
- **String interpolation quirks:**
  `"{var}"` can print literally in certain compiler paths. Prefer string concatenation
  or explicit string formatting functions (`str(x)`).
- **Trig probes via `kain -c`:**
  `kain -c 'sin(1.57)'` can return 0 due to repl harness stubbing. Real trig functions
  work accurately when compiled from source files. Always verify mathematical kernels
  via file compilation.

---

## 9. Current Phase & Immediate Roadmap

We are executing the **Step 0 & Step 1 Pilot**:
1. **Scaffold the 6 Subsystems in `kain/`:** Establish `kain/core/`, `kain/tokenizer/`,
   `kain/tensor/`, `kain/model/`, `kain/flow/`, and `kain/refinery/`.
2. **Solidify the Glue (`kain/core/_common.kn`):** Pin down the shared arena allocators,
   memory windows, SIMD safety padding, and tensor view structs that bind all modules together.
3. **Build the 16k Tokenizer (`kain/tokenizer/tokenizer_16k.kn`):** Byte-pair BPE tokenizer
   mapping English and code bytes directly to compact integer vectors.
4. **Validate Tensor Math Kernels (`kain/tensor/`):** Implement and prove the FWHT butterfly
   addition network and 1.58-bit ternary dot products in `kain/spike/`.
5. **Amalgamation & LLVM Build Verification:** Prove that modular source files cleanly
   amalgamate into `kain/core.kn` and compile to native `kain/core.exe` with zero errors.

We build with precision. Every file connects like glue.
