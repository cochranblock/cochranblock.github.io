---
title: "oakilydokily — Timeline of Invention"
layout: default
---

# Timeline of Invention — oakilydokily

*Dated, commit-level record of what was built, when, and why.*

[← Back to docs index](../) | [Proof of Artifacts](artifacts) | [GitHub repo](https://github.com/cochranblock/oakilydokily)

---

## Human Revelations — Invented Techniques

### ESIGN-Compliant Waiver in Single Binary (March 2026)

**Invention:** Legally compliant electronic signature waiver system (ESIGN Act, 15 U.S.C. 7001-7031) with 7-year retention, audit trail, and multi-auth — all in a single Rust binary with SQLite WAL, no external services.

ESIGN compliance requires: (1) intent to sign (separate `signature` field from `full_name`), (2) consent to electronic records, (3) 7-year retention. Zero cloud services. No DocuSign, no Adobe Sign.

**Commit:** `79d816d`

---

### Zero-JavaScript Architecture (March 2026)

**Invention:** A production web application with 2,496 lines of Rust and 0 lines of JavaScript. Server-rendered HTML, CSS animations, HTML forms — no WASM, no build step, no client-side framework.

**Commit:** `cb0cbf6`

---

## Entries

### 2026-04-09 — Canonical Template Conformance + Docs Refresh

**What:** Standardized TOI and POA to canonical templates. Backfilled 2026-04-03 security/test burst. Updated metrics: binary 8.7→8.8 MB, Rust LOC 2,748→3,211, integration checks 25→59, unit tests 0→15.

**Commit:** `f5e6881`

---

### 2026-04-03 — Forge RCE Close + Backlog Sprint

**What:** Closed critical RCE in `/api/forge`: added auth gate (unauthenticated POSTs → 401), replaced shell-string interpolation with compile-time-constant `remote_cmd()` + stdin-only JSON. Also: SESSION_SECRET fail-fast, waiver bypass env, rate-limiter pruning, 12 unit tests, forge injection integration tests. **Triple sims: 3/3 pass, 38 checks.**

**Commits:** `640e5ae`, `d8a0663`, `d0fa1a1`, `58168a5`, `8655e26`

---

### 2026-04-02 — Async I/O, Rate Limiting, Dead Code Cleanup

**What:** Fixed 8 instances of `reqwest::blocking::Client` in async handlers. Added IP-based rate limiting (10/60s) on login/signup. Removed all `#![allow(dead_code)]`. Full docs truth audit.

**Commits:** `7575531`

---

### 2026-03-30 — Zero Downtime Hot Reload

**What:** SO_REUSEPORT socket binding, PID lockfile management, SIGTERM/SIGKILL old instance. New binary binds port before killing old.

---

### 2026-03-30 — Supply Chain Security Audit

**What:** EO 14028 audit: `cargo audit` (fixed RUSTSEC-2026-0049), duplicate dep analysis, deep code review. Removed 42 MB dead ONNX model.

**Commit:** `db89576`

---

### 2026-03-29 — iOS + PWA + Multi-Arch (12 Targets)

**Commit:** `e56fc16`

---

### 2026-03-29 — Android AAB: 4.6 MB

**What:** Full Gradle project, cargo-ndk, JNI bridge, launcher icon. Pocket Server architecture.

**Commits:** `0df7ec6`, `db89576`

---

### 2026-03-28 — Zero JavaScript: 2,496 Rust / 0 JS

**What:** Removed all JavaScript (67 KB gl.js, mural-bridge.js, mural-wasm.wasm). Replaced WASM canvas with static mural + CSS gradient. Added /govdocs routes.

**Commit:** `cb0cbf6`

---

### 2026-03-27 — Binary Optimization: 42 MB → 9.1 MB

**What:** Removed rust-gmail dep (killed openssl chain), added strip/LTO/codegen-units=1, removed 25 MB of unused source PNGs.

**Commit:** `19f481b`

---

### 2026-03-27 — Waiver: Signature Field + 7-Year Retention

**What:** Added `signature` input separate from `full_name` (ESIGN requirement). Fixed `archive_prune` from 365 → 2,557 days. Wired /auth/signup.

**Commit:** `79d816d`

---

### 2026-03-22 — Pixel Forge Mural Integration

**What:** `/api/forge` — SSH dispatches to GPU node for AI sprite generation.

**Commit:** `b066b52`

---

### 2026-03-14 — Waiver System + Multi-Auth

**What:** ESIGN-compliant liability waiver with audit trail. Google/Facebook/Apple/manual OAuth. First paying partnership.

---

*[Full history on GitHub](https://github.com/cochranblock/oakilydokily/commits/main)*
