---
title: "pixel-forge — Proof of Artifacts"
layout: default
---

# Proof of Artifacts — pixel-forge

*Concrete evidence that this project builds, trains, and generates output.*

[← Back to docs index](../) | [Timeline of Invention](timeline) | [GitHub repo](https://github.com/cochranblock/pixel-forge)

---

## Build Output

| Metric | Value |
|--------|-------|
| Lines of Rust | ~17,311 across 39 .rs files |
| Binary size (macOS ARM) | 9.2 MB (opt-level=z, LTO, strip) |
| Binary size (macOS x86) | 7.6 MB |
| Binary size (Linux x86) | 11.3 MB |
| Model: Cinder | 1.09M params, 4.2 MB |
| Model: Quench | 5.83M params, 22 MB |
| Model: Anvil | 16.9M params, 64.5 MB |
| Training data (balanced) | 19,876 tiles, 68 active classes (capped 2K/class) |
| Class conditioning | 10 super-categories + 12 binary tags |
| ML framework | Candle 0.8 (Metal, CUDA, CPU) |
| Diffusion recipe | EDM (Karras 2022), Min-SNR γ=13 |
| BCE recipe | sigmoid + stable BCE-with-logits (no diffusion) |
| Data normalization | Per-channel z-score with mandatory `.normalize.json` sidecar |
| Model integrity | NanoSign: BLAKE3 sign on save, verify on load |

## Training Loss (Anvil v6, 200 epochs on lf RTX 3070)

| Epoch | Loss | LR | Time/epoch |
|-------|------|----|------------|
| 1 | 0.1927 | 1.0e-5 | 753s |
| 6 | 0.0799 | 1.1e-4 | 769s |
| 51 | 0.0654 | 1.8e-4 | 761s |
| 101 | 0.0653 | 1.1e-4 | 750s |
| 200 | 0.0542 | 1.0e-5 | 756s |

Total: ~42 hours on RTX 3070.

## Anvil-BCE Silhouette (in progress, 2026-05-18)

Shape-Diffusion Signal Split — stage 1. BCE loss, no diffusion, binary silhouette masks.

| Epoch | Loss | Notes |
|-------|------|-------|
| 1 | 0.314 | ln(2) baseline |
| 11 | 0.160 | No plateau (vs EDM at 8.5) |
| 21 | 0.122 | EDM would have died here |
| 46 | 0.093 | Still descending |

## GPU Backends

| Backend | Hardware | Status |
|---------|---------|--------|
| Metal | Apple Silicon | Primary dev path |
| CUDA | NVIDIA (lf RTX 3070, gd RTX 3050 Ti) | Training |
| Vulkan via any-gpu | AMD (bt RX 5700 XT) | Training + inference |
| CPU | Any | Fallback |
| WebGPU WASM | Browser | Beta inference |

*[Full PROOF_OF_ARTIFACTS.md on GitHub](https://github.com/cochranblock/pixel-forge/blob/main/PROOF_OF_ARTIFACTS.md)*
