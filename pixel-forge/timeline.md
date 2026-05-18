---
title: "pixel-forge — Timeline of Invention"
layout: default
---

# Timeline of Invention — pixel-forge

*Dated, commit-level record of what was built, when, and why. Proves human-piloted AI development.*

[← Back to docs index](../) | [Proof of Artifacts](artifacts) | [GitHub repo](https://github.com/cochranblock/pixel-forge)

---

## Human Revelations — Invented Techniques

### Shape-Diffusion Signal Split (May 2026)

**Invention:** A generation cascade matching loss type to signal type: BCE for shape/silhouette (binary classification), EDM diffusion for color (continuous generation).

**Evidence:** Every EDM run on binary silhouette masks plateau'd at epoch 6-11 regardless of model size or γ. BCE converges without plateau. Root cause: diffusion adds Gaussian continuous noise — binary masks (0 or 1) have no "hard noise regime" for EDM to target. BCE treats each pixel as independent binary classification. Correct framing → convergence.

**Cascade:** Anvil-BCE → Quench-BCE → Quench-color (EDM + mask) → Cinder-detail

**Commits:** `2640e90e` (BCE training mode), `3f72254b` (γ=13 fix + diagnosis)

---

### NanoSign Model Integrity (April 2026)

**Invention:** 36-byte model signing (4-byte `NSIG` magic + 32-byte BLAKE3 hash) appended to any model file. Verified on every load. Tampered files rejected, unsigned files pass.

**Origin:** Defense systems verify firmware before execution. Model files are executable logic — they should have the same guarantee.

**Commit:** `92748094`

---

### MoE Cascade Pipeline — Cinder to Quench to Anvil (March 2026)

**Invention:** Tiered diffusion where each model uses the previous tier's output as its starting noise.
- **Cinder** (1.09M, 4.2MB): rough sprite from pure noise
- **Quench** (5.83M, 22MB): refines Cinder output
- **Anvil** (16.9M, 64MB): fine detail from Quench output
- Mobile: Cinder only. Desktop: full cascade. Device auto-detects.

---

### Hybrid Class Conditioning (March 2026)

**Invention:** 10 super-categories + 12 binary tags replace integer class labels. New classes = new tag combinations. Zero retraining needed.

**Result:** Scaled from 16 to 108 classes without retraining.

**Commit:** `8e72544c`

---

## Entries

### 2026-05-18 — BCE Training Fixes: Denoising Prior + Zero-Init Inference

**What:**
1. **Zero-init for inference (`0b8ab101`):** Training now initializes from zeros (not the target image). Teaches the model to generate from class conditioning alone, not to reconstruct.
2. **Target+noise denoising prior (`bd8e2bda`):** `x_input = target + noise * 0.3`. Creates a denoising prior — the BCE analog to diffusion's noise schedule.

**Commits:** `0b8ab101`, `bd8e2bda`

---

### 2026-05-17/18 — BCE Silhouette Pivot + γ=13 Diagnosis + Anvil-BCE Training

**What:** Diagnosed γ=5 was a no-op (1/σ_data² ≈ 6.23 > 5, so every sample was clamped). Fixed to γ=13. Still plateau'd — diagnosed EDM as wrong tool for binary data. Pivoted to Shape-Diffusion Signal Split architecture.

**Anvil-BCE loss trajectory:**

| Epoch | Loss | Notes |
|-------|------|-------|
| 1 | 0.314 | Random init baseline: ln(2) ≈ 0.693 |
| 11 | 0.160 | >50% reduction, no plateau |
| 21 | 0.122 | Where EDM permanently died |
| 46 | 0.093 | Still dropping |

**Commits:** `3f72254b`, `d250c1fb`, `50b92365`, `98b40840`, `2640e90e`, `eda7a0bb`

---

### 2026-05-15 — Quench EDM Training + Recipe Regression Diagnosis

**What:** Kicked off Quench training (5.83M, fp16, bs=16, 200 epochs) on lf RTX 3070. Diagnosed regression: Apr 19 Cinder model bricked — `701eea94` removed sidecar fallback, `a1f61fda` replaced DDPM with EDM Euler. Math fundamentally different, no compat path. Quench EDM is path forward.

---

### 2026-05-05/06 — EDM Preconditioning + Mandatory Z-Score Sidecar

**What:** Migrated from DDPM to Karras EDM (2022). `src/precond.rs` with EdmCoeffs, log-normal σ distribution. Z-score normalization mandatory (`.normalize.json` sidecar).

**Commits:** `9eaa2669`, `6b125b4a`, `ab9c6763`, `06ee6b50`, `701eea94`, `8d68ff82`, `6b2d32f1`, `a1f61fda`, `7cef7c9f`

---

### 2026-04-30 — WASM Browser Backend + Beta Landing Page

**What:** WASM crate runs Cinder inference in-browser via WebGPU through any-gpu. Real WebGPU runtime wired to beta panel.

**Commits:** `b649e42f`, `2888c8b1`, `73e417ba`, `1810517e`

---

### 2026-04-26 — Tiered Pipeline + Vulkan/any-gpu Backend for Cinder

**What:** 3-stage tiered pipeline: per-class silo → PaletteNet 8 colors → Cinder-detail. Vulkan/any-gpu backend enables training on AMD bt node (5700 XT, no CUDA).

**Commits:** `37220e05`, `5469b0dd`, `a68a6c0f`

---

### 2026-04-10/11/12 — Per-Class Silos + Sponge Mesh + any-gpu Fleet Training

**What:** 18 per-class MicroUNet silos (~97K params each). Sponge Mesh: self-healing training auto-retries on NaN loss or plateau (max 3 retries). `train-fleet` command for multi-GPU distributed training.

**Commits:** `ebafcb2b`, `2a3dc0f1`, `de887913`, `b90a710a`, `a68a6c0f`, `ae8e9de0`, `5905b86e`, `63aa0f70`

---

### 2026-04-03 — NanoSign Model Integrity + Documentation P23

**What:** NanoSign BLAKE3 signing. Covers all 6 save paths and all 10 load paths. Full P23 Triple Lens audit.

**Commits:** `92748094`, `d1e60bf6`, `2cfc5c0a`, `801bc218`

---

### 2026-04-01/02 — Inference Bug Fixes + Training Improvements + Anvil v7

**What:** Fixed 3 inference bugs: CFG scale 1.0→3.0, uniform noise→Gaussian, DDIM clamp. Added `--resume`, rotation augmentation. Anvil v7 training on lf.

**Commits:** `d4b28270`, `68f2183a`

---

### 2026-03-28/29/30 — Diffusion Debug + Gaussian Noise Fix + Anvil Training + Multi-Arch

**What:** Root cause: uniform [0,1] noise. Fixed to Gaussian N(0,1). Anvil v6 (16.9M) training on RTX 3070 — loss 0.08 at epoch 6, showing structured output. Multi-arch release: macOS ARM/Intel, Linux x86, Android AAB.

**Commits (14):** `b84b5b8b`→`541720cd`

---

### 2026-03-27 — Hybrid Class Conditioning + Play Store Pipeline

**What:** Replaced 16-class integer system with hybrid conditioning: 10 super-categories + 12 binary tags. 108 classes. Trained Cinder v2 (lf) and Quench v2 (gd) in parallel with 75,182 tiles.

**Commits (11):** `5c45b2a5`→`4c8a473c`

---

### 2026-03 — From-Scratch Diffusion Models

**What:** Three UNet tiers from scratch using Candle: Cinder (1.09M/4.2MB), Quench (5.83M/22MB), Anvil (16.9M/64MB). On-device training, zero cloud.

---

*[Full history on GitHub](https://github.com/cochranblock/pixel-forge/commits/main)*
