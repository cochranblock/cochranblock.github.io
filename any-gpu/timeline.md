---
title: "any-gpu — Timeline of Invention"
layout: default
---

# Timeline of Invention — any-gpu

*Dated, commit-level record of what was built, when, and why. Proves human-piloted AI development.*

> Every entry maps to real commits. Run `git log --oneline` to verify.

[← Back to docs index](../) | [Proof of Artifacts](artifacts) | [GitHub repo](https://github.com/cochranblock/any-gpu)

---

### 2026-05-18 — P7: Two-Pass GEMV for M=1 Decode

**What:** New two-pass GEMV shader: pass 1 accumulates partial dot products into VGPR; pass 2 reduces via LDS. Avoids 32x32 tile machinery for the M=1 decode case.
**Commit:** [`93321779`](https://github.com/cochranblock/any-gpu/commit/93321779) | **Verified:** 292/292 pass on bt (AMD RX 5700 XT, RADV NAVI10, Mesa 25.0.7)

---

### 2026-05-18 — P6: Wave64 Fused SDPA for Decode

**What:** One wavefront of 64 threads per Q row, each thread handles K/V slice with `subgroupAdd`. Eliminates inter-thread-group barrier overhead for single-row attention.
**Commits:** [`69c1ed02`](https://github.com/cochranblock/any-gpu/commit/69c1ed02), [`f3fb023b`](https://github.com/cochranblock/any-gpu/commit/f3fb023b) | **Verified:** 292/292 pass on bt

---

### 2026-05-18 — P5: SDPA VGPR Accumulator + LayerNorm 2-Barrier Fuse

**What:** SDPA running max/sum moved from LDS to VGPR. LayerNorm fused to 2 barriers. `ops_bench` binary measuring dispatch time at representative sizes.
**Commit:** [`0c204d86`](https://github.com/cochranblock/any-gpu/commit/0c204d86) | **Verified:** 292/292 pass on bt

---

### 2026-05-17 — Full Op Coverage: 256→292 Tests

**Round 1 (262→275):** `f628`, `f642`, `f645`, `f661`, `f580` tiled matmul, `f621` multi-batch SDPA.
**Round 2 (275→292):** `f643`, `f644`, `f646`, 2D dispatch, depthwise conv, stride-2 conv, `f601` backward, multi-position RoPE.
**Commits:** [`edb09113`](https://github.com/cochranblock/any-gpu/commit/edb09113), [`1fe65f76`](https://github.com/cochranblock/any-gpu/commit/1fe65f76) | **Verified:** 292/292 pass on bt

---

### 2026-05-17 — P4: batch_matmul 4×4/wave64 Register Blocking

**What:** 4×4 register blocking + wave64 workgroup for batched GEMM. 16 FMAs per thread per k-step, 32×32 LDS tiles. 260→262 tests.
**Commit:** [`8ea2b50a`](https://github.com/cochranblock/any-gpu/commit/8ea2b50a) | **Verified:** 262/262 pass on bt

---

### 2026-05-17 — Matmul 4×4/wave64 + subgroup-fused softmax

**What:** Wave64 single-wavefront matmul (barrier = free intra-wavefront fence). 4×4 register blocking (16 FMAs/thread/k-step). **512² +42% (118→168 GFLOPS), 1024² +24% (118→146 GFLOPS).** Subgroup-fused softmax: one 256-thread workgroup per row, `subgroupMax`/`subgroupAdd` replace LDS tree reduction.
**Commit:** [`6216f68`](https://github.com/cochranblock/any-gpu/commit/6216f68) | **Verified:** 256/256 pass on bt

---

### 2026-05-17 — Sprint 7 complete: fused SDPA + tokenizer + LLM inference stack + serve runtime

**What:** `f626` = online-softmax fused causal SDPA (O(N) VRAM). `f627/f628/f629` = split_heads/merge_heads/repeat_kv. `t544` = Tokenizer, `t545/546` = Module/Linear, `t547/548` = LmConfig/CausalLM (LLaMA-compatible). `any-gpu-serve` binary: `POST /generate`, `GET /health`. **239→256 tests.**
**Commits:** [`ae8a688`](https://github.com/cochranblock/any-gpu/commit/ae8a688), [`522a63a`](https://github.com/cochranblock/any-gpu/commit/522a63a) | **Verified:** 256/256 pass on bt

---

### 2026-05-17 — S7.4: LayerPager + S7.5: GpuBufferF16

**What:** `t539 = LayerPager` — pinned-RAM staging buffer for streaming model weights VRAM one layer at a time (default 256 MB). `t540 = GpuBufferF16` — packed f16 storage via `unpack2x16float`. **228→233 tests.**

---

### 2026-05-16 — Self-Licking Test Audit + Hardcoded-Reference Backstops

**What:** Found 5 ops with CPU helper = shader formula (self-licking cone). Added hardcoded reference tests from PyTorch/IEEE spec: sigmoid, SiLU, RMSNorm, softmax, SDPA. **221/221 pass.**

---

### 2026-05-16 — Safetensors Loader (Sprint 7, step 3)

**What:** `t538 = SafetensorsModel` — NanoSign-aware safetensors loader with bf16/f16 dequant. `f766 = bf16_to_f32`, `f767 = f16_to_f32`. **195→214 tests.**

---

### 2026-05-15 — Causal SDPA + RoPE + KV Cache (Sprint 7, step 2)

**What:** `f624` = apply_causal_mask (asymmetric Q/KV), `f623` = scaled_dot_product_attention_causal, `f625` = rope (Llama/Mistral/Qwen/Gemma convention), `t534` = KVCache (append-only). **3 WGSL shaders, 21 tests → 195 total.**

---

### 2026-05-15 — Transformer Inference Primitives (Sprint 7, step 1)

**What:** LayerNorm, RMSNorm, GELU (tanh-approx, PyTorch-validated), embedding_lookup, argmax. **7 WGSL shaders, 29 tests → 174 total.** GELU validated against PyTorch reference values — explicit anti-self-licking discipline.

---

### 2026-04-09 — Autograd, Training Loop, Pipeline Caching

**What:** Tiled matmul, Tensor type (6 dims inline, no heap), reverse-mode autograd (flat tape, 13 differentiable ops), 7 backward shaders, AdamW optimizer, `train_step()`. Pipeline caching via `HashMap<u64, Arc<ComputePipeline>>`. **62→145 tests.**
**Commits:** [`0ca243d`](https://github.com/cochranblock/any-gpu/commit/0ca243d), [`dd55772`](https://github.com/cochranblock/any-gpu/commit/dd55772), [`5137d40`](https://github.com/cochranblock/any-gpu/commit/5137d40), [`c09b0ee`](https://github.com/cochranblock/any-gpu/commit/c09b0ee), [`9511a61`](https://github.com/cochranblock/any-gpu/commit/9511a61), [`9f0b567`](https://github.com/cochranblock/any-gpu/commit/9f0b567), [`6d93866`](https://github.com/cochranblock/any-gpu/commit/6d93866), [`a905cd1`](https://github.com/cochranblock/any-gpu/commit/a905cd1)

---

### 2026-04-03 — NanoSign Integration + Full Doc Update

**What:** NanoSign BLAKE3 model signing. Every model file signed on write, verified on load.
**Commits:** [`5e58eb3`](https://github.com/cochranblock/any-gpu/commit/5e58eb3)

---

### 2026-04-02 — CPU-Validated Test Suite

**What:** 27 smoke tests replaced with 54 correctness tests. Every op cross-validated against CPU reference. Edge cases: odd sizes, 1x1 tensors, non-square matmul, multi-channel batch. **Verified on bt (5700 XT), lf (RTX 3070), gd (RTX 3050 Ti), Apple M4.**
**Commit:** [`801c4de`](https://github.com/cochranblock/any-gpu/commit/801c4de)

---

### 2026-04-02 — 15 Diffusion Training Ops

**What:** relu, sigmoid, swish/silu, tanh, conv2d, conv_transpose2d, batch_matmul, group_norm, concat, transpose, upsample_nearest2d, softmax, scaled_dot_product_attention, scale, sub, mse_loss. Conv2d benchmark: full UNet forward pass ~7.9ms (127 fps) on 5700 XT.
**Commits:** [`8aa9fc1`](https://github.com/cochranblock/any-gpu/commit/8aa9fc1), [`56976a7`](https://github.com/cochranblock/any-gpu/commit/56976a7)

---

### 2026-04-02 — CUDA/Metal Comparison Benchmarks

**What:** Head-to-head matmul comparison — RTX 3070 (CUDA 4-22x faster), RTX 3050 Ti (CUDA 17-50x faster), Apple M4 (MPS 6-9x faster). Documented honestly.
**Commit:** [`d6ab4ec`](https://github.com/cochranblock/any-gpu/commit/d6ab4ec)

---

### 2026-04-02 — AMD RADV Segfault Fix

**What:** Three SIGSEGV fixes for AMD RADV/RDNA1 (Navi 10): adapter.limits() instead of defaults, removed enumerate_adapters(), replaced arrayLength() with uniform params, shared device via LazyLock.
**Commits:** [`35c75ef`](https://github.com/cochranblock/any-gpu/commit/35c75ef), [`e124fbb`](https://github.com/cochranblock/any-gpu/commit/e124fbb), [`56976a7`](https://github.com/cochranblock/any-gpu/commit/56976a7)

---

### 2026-04-02 — 4-GPU Benchmark Matrix

**What:** Matmul benchmarks 64x64 through 1024x1024. AMD RX 5700 XT: 69 GFLOPS (181x vs CPU). RTX 3070: 151 GFLOPS. M4: 122 GFLOPS. Naive WGSL shader.
**Commit:** [`1a93e7f`](https://github.com/cochranblock/any-gpu/commit/1a93e7f)

---

### 2026-04-02 — Sprint 1: wgpu Compute Backend

**What:** GpuDevice, GPU auto-discovery, upload/alloc/read, 3 WGSL shaders (matmul 16x16, add, mul), bench.
**Commit:** [`e1a6d96`](https://github.com/cochranblock/any-gpu/commit/e1a6d96)

---

*[Full history on GitHub](https://github.com/cochranblock/any-gpu/commits/main)*
