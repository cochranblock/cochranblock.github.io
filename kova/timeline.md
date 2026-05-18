---
title: "kova — Timeline of Invention"
layout: default
---

# Timeline of Invention — kova

*Dated, commit-level record of what was built, when, and why. Proves human-piloted AI development.*

> Every entry maps to real commits. Run `git log --oneline` to verify.

[← Back to docs index](../) | [Proof of Artifacts](artifacts) | [GitHub repo](https://github.com/cochranblock/kova)

---

## Human Revelations — Invented Techniques

### P13 Compression Mapping (March 2026)

**Invention:** A human-designed tokenization scheme that compresses all public symbols in a Rust codebase to short tokens (f0-fN for functions, t0-tN for types), reducing AI context consumption by 40-60% per session.

**Origin:** Military communications brevity codes (WILCO, SITREP, CASEVAC). Michael Cochran's 13 years in Army signals/cyber (17C). Applied to AI-human collaboration as a token economy measure.

**Named:** P13 Compression Mapping | **Commit:** `1012a05` (first kova tokenization)

---

### Agentic Tool Loop with Tokenized Commands (March 2026)

**Invention:** An AI agent loop where the LLM calls tools iteratively until the task is complete, with all system commands pre-compressed into tokenized aliases that produce compressed output.

**Named:** Tokenized Agentic Loop | **Commit:** See `agent_loop.rs`, `cargo_cmd.rs`, `git_cmd.rs`

---

### C2 Swarm Orchestration (March 2026)

**Invention:** A command-and-control system treating 4 heterogeneous worker nodes as a single distributed compute fabric, orchestrated over SSH with circuit breakers and job deduplication.

**Named:** IRONHIVE C2 | **Commits:** `c1265c9`, `c986dbc`, `b4211c5`

---

### tmuxisfree Fleet Mesh (April 2026)

**Invention:** A tmux session manager that treats AI agent panes as a fleet of workers, dispatching tasks, broadcasting commands, detecting rate limits, and auto-retrying.

**Named:** Sponge Mesh Broadcast (tf3)

---

## Entries

### 2026-05-18 — Serve: Pure REST + OpenAPI JSON; Tele Pipeline; kova-test Fix

**What:** Three changes in `b5d8b64`:
1. Kill WASM client, pure REST. OpenAPI JSON at `/openapi.json`.
2. Tele pipeline wiring.
3. kova-test fix (WASM lock deadlock + bindgen mismatch resolved).

**Commit:** `b5d8b64`

---

### 2026-05-17 — P13 Swarm Tokenization + Static Corpus Carver

**What:** Two items:
1. P13 tokenize swarm module — `Example→t216`, `SubatomicConfig→t217`, `featurize→f394`, `train_starter→f395`, `predict→f396`.
2. Static corpus carver (`src/bin/carve_static.rs`) — walks 240,596-crate corpus, extracts .rs files, emits labeled JSONL for 15 subatomic models. Smoke test: 100 crates → 1,979 examples in <1s release.

**Commit:** *(this commit)*

---

### 2026-05-17 — sled → redb Migration + REPL Pyramid Wiring + Correctness Hardening

**What:** Three interlocking improvements:
1. sled → redb migration across all 7 storage consumers.
2. REPL subatomic T1 wiring — `code_vs_english` and `slop_detector` classifiers run from embedded `starter.nanobyte`.
3. User story analysis + master Rust engineer review (10 findings, 6 fixed).

**Commit:** *(this commit)*

---

### 2026-05-07 — Hash Dim Bump 256 → 8192 for Original 3 Starters

| Model | Old (256-dim) | New (8192-dim) | Δ |
|---|---|---|---|
| slop_detector | 89.4% | 88.5% | -0.9 |
| code_vs_english | 94.2% | **97.1%** | **+2.9** |
| lang_detector | 97.0% | 93.3% | -3.7 |

**Commit:** *(this commit)*

---

### 2026-05-06 — intent_classifier + Bench Harness on banking77

**What:** Trained banking77 intent classifier — **80.36% accuracy on test (3,080 examples)**, macro F1 81.10%. Hash dim 256 → 4096. 315,469 params. Packed into `starter.nanobyte`.

**Commit:** *(this commit)*

---

### 2026-05-06 — Nanobyte Inference + Embedded Starter + REPL Telemetry

**What:** `Nanobyte::infer(model, text)` parity-tested against on-disk path. `STARTER_NANOBYTE: &[u8]` baked into `.rodata`, zero file I/O at REPL startup.

**Commit:** *(this commit)*

---

### 2026-05-06 — Nanobyte Format + Starter Pack

**What:** 64-byte header + 80-byte/entry manifest + f32 weights + 36-byte NSIG trailer. `assets/starter.nanobyte` — 9,592 bytes, 3 models, 2,313 total params.

**Commit:** `44caf37`

---

### 2026-04-03 — Subatomic Models Trained + NanoSign + P23 + Blueprint

**What:** Trained first 3 subatomic models on bt AMD RX 5700 XT via any-gpu Vulkan. Built swarm training infrastructure. Harvested 240,596 crates from crates.io (34GB). Published NanoSign spec. Created P23 Triple Lens Research Protocol. Published 66-model subatomic catalog.

**Commits:** `a4da3f6`, `80965f6`, `2c6d647`, `f244483`, `5671ca3`, `c36064c`, `96f8245`, `329c7cd`

---

### 2026-04-02 — Pyramid Architecture + Docs Audit

**What:** Full Pyramid Architecture — subatomic/molecular/cellular model tiers with shared mmap'd nanobyte weight blob.

**Commits:** `43a7c4d`, `373edd9`, `a24106d`, `558da7f`, `07dbc4a`, `8ec4709`, `c2e2e92`, `cda7462`

---

### 2026-04-01 — Claude Code Architecture Fusion

**What:** Context compaction, dual-mode inference, checkpoint/undo, exec tool rename + permission gates. 314 tests passing.

**Commits:** `6efab67`, `3fa3080`, `6091dad`, `e8a0cb0`, `a79be81`

---

### 2026-03-29 — Multi-Architecture Release

**What:** macOS ARM64 (27 MB), macOS x86_64 (13 MB), Android AAB (6.6 MB), Android APK (17 MB). All uploaded to GitHub Release v0.7.0.

**Commit:** `c876698`

---

### 2026-03-28 — C2 CLI Tools + Govdocs Subcommand

**What:** 5 new c2 subcommands: dispatch, broadcast, status, monitor, fleet. Govdocs: 11 compliance docs baked into binary.

**Commit:** `0398ed4`

---

### 2026-03-27 — Federal Compliance Documentation

**What:** 11 govdocs: SBOM (EO 14028), SSDF (NIST 800-218), supply chain, security posture, Section 508, privacy impact, FIPS gap, FedRAMP, CMMC, ITAR/EAR, federal use cases.

**Commit:** `7c474da`

---

### 2026-03-27 — P13 Tokenization + Binary Size Reduction

**What:** Tokenized quantize.rs. Binary: 54 MB → 27 MB (48% reduction).

**Commit:** `1012a05`

---

*[Full history on GitHub](https://github.com/cochranblock/kova/commits/main)*
