---
title: "kova — Proof of Artifacts"
layout: default
---

# Proof of Artifacts — kova

*Concrete evidence that this project works, ships, and is real.*

[← Back to docs index](../) | [Timeline of Invention](timeline) | [GitHub repo](https://github.com/cochranblock/kova)

---

## What It Is

Augment engine. Local-first agentic tool loop with dual-mode inference (local GGUF or Anthropic SSE), 5-node SSH swarm orchestration, and tokenized-everything compression. One binary. No cloud.

## Build Output

| Metric | Value |
|--------|-------|
| Binary size (release) | 27 MB (opt-z, LTO, strip, codegen-units=1, panic=abort) |
| Android APK | 17 MB (signed release) |
| Android AAB | 6.6 MB (signed, for Play Store) |
| Lines of Rust | 41,629 across 103 files |
| Tokenized functions | f0–f402 |
| Tokenized types | t0–t217 |
| User surfaces | 7 (REPL, TUI, GUI, HTTP+WASM, Android, iOS scaffold, PWA) |
| Agent tools | 13 |
| Worker nodes | 5 (SSH orchestrated) |
| Unit/integration tests | 314 passing |
| LLMs evaluated | 42 (Micro Olympics) |

## QA Results (2026-05-17)

| Gate | Result |
|------|--------|
| cargo check --features serve | PASS |
| cargo test --lib | PASS (254 tests) |
| cargo test --release | PASS (314 tests) |
| TRIPLE SIMS | 101 pass |

## Architecture

```
User → REPL / TUI / GUI / HTTP+WASM
         ↓
    Agent Loop
         ↓
    ┌────────────────────────────┐
    │ Context Compaction (f380)  │
    │ 13 Tools (src/tools.rs)    │
    │ Dual-Mode Inference (f382) │
    │   → Anthropic SSE (f381)  │
    │   → IRONHIVE Cluster      │
    │ Checkpoint/Undo (f383/384) │
    └────────────────────────────┘
         ↓
    C2 → n0/lf (RTX 3070) · n1/gd (RTX 3050 Ti) · n2/bt (RX 5700 XT) · n3/st (CPU) · n4/mm (Mac Mini)
```

## Subatomic Models (starter.nanobyte)

| Model | Params | Accuracy | Dim |
|-------|--------|----------|-----|
| slop_detector | 16,386 | 88.5% | 8192 |
| code_vs_english | 16,386 | 97.1% | 8192 |
| lang_detector | 40,965 | 93.3% | 8192 |
| intent_classifier | 315,469 | 80.36% (banking77) | 4096 |

*[Full PROOF_OF_ARTIFACTS.md on GitHub](https://github.com/cochranblock/kova/blob/main/PROOF_OF_ARTIFACTS.md)*
