---
title: "any-gpu — Proof of Artifacts"
layout: default
---

# Proof of Artifacts — any-gpu

*Concrete evidence that this project works on real hardware.*

[← Back to docs index](../) | [Timeline of Invention](timeline) | [GitHub repo](https://github.com/cochranblock/any-gpu)

---

## Why This Exists

CUDA only runs on NVIDIA. Metal only runs on Apple. any-gpu fills the gap for AMD and Intel GPUs — one wgpu/WGSL backend that works on all vendors.

If you have an NVIDIA GPU and want peak performance, use CUDA. any-gpu is for heterogeneous fleets.

## GPU Benchmark Results (292 tests verified on bt — AMD RX 5700 XT, RADV NAVI10, Mesa 25.0.7)

| Op | Perf | Notes |
|----|------|-------|
| Matmul 512² | **168 GFLOPS** | Wave64 4×4 register blocking (P1) |
| Matmul 1024² | **146 GFLOPS** | +24% over naive shader |
| Conv2d UNet forward (32x32) | ~7.9ms (127 fps) | Full diffusion UNet forward pass |

## CUDA/Metal Comparison (honest)

| GPU | any-gpu | CUDA/Metal | Ratio |
|-----|---------|-----------|-------|
| RTX 3070 (CUDA) | baseline | 4-22x faster | CUDA wins |
| RTX 3050 Ti (CUDA) | baseline | 17-50x faster | CUDA wins |
| Apple M4 (MPS) | baseline | 6-9x faster | Metal wins |
| **AMD RX 5700 XT** | **works** | CUDA: N/A | **any-gpu is the only option** |

## Shader Inventory

56+ WGSL compute shaders across:
- Elementwise ops (relu, sigmoid, gelu, swish, tanh)
- Backward ops (all differentiable)
- Convolution (tiled wave64, batch, transpose, depthwise)
- Normalization (group, layer, RMS)
- Attention (softmax, SDPA, causal, RoPE, KVCache, fused online-softmax)
- Transformer ops (embedding_lookup, argmax)
- Spatial (upsample)
- Loss (mse_loss)
- Optimizer (AdamW in-place)

## Inference Stack (Sprint 7)

Full LLM inference pipeline built on the GPU ops:
- `t544 = Tokenizer` (HuggingFace tokenizers wrapper)
- `t548 = CausalLM` (LLaMA-compatible forward pass)
- `any-gpu-serve` binary: `POST /generate`, `GET /health`
- KVCache for autoregressive decoding
- Safetensors loader (NanoSign-aware, bf16/f16 dequant)

## Test Coverage

| Date | Tests | Hardware |
|------|-------|---------|
| 2026-04-02 | 54 (all correctness, CPU-cross-validated) | bt, lf, gd, Apple M4 |
| 2026-05-16 | 221 (+ hardcoded IEEE/PyTorch reference tests) | bt (5700 XT) |
| 2026-05-17 | 256 (Sprint 7 complete) | bt |
| 2026-05-18 | **292** (full op coverage) | bt |

*[Full PROOF_OF_ARTIFACTS.md on GitHub](https://github.com/cochranblock/any-gpu/blob/main/PROOF_OF_ARTIFACTS.md)*
