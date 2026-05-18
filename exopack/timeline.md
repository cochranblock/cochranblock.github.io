---
title: "exopack — Timeline of Invention"
layout: default
---

# Timeline of Invention — exopack

*Dated, commit-level record of what was built, when, and why.*

[← Back to docs index](../) | [Proof of Artifacts](artifacts) | [GitHub repo](https://github.com/cochranblock/exopack)

---

## Human Revelations — Invented Techniques

### Triple Sims — Run 3x, All Must Pass (March 2026)

**Invention:** Run the entire test suite 3 times in sequence. All 3 runs must pass. Catches flaky tests, race conditions, non-deterministic behavior.

**Origin:** Military simulation ranges run the same scenario 3 times before certifying a system. Three hits is a pattern. Three misses is a failure.

**Commit:** `a4989c1`

---

### Test Binary Augmentation Pattern (March 2026)

**Invention:** The same Rust binary serves as both production app and test runner. A `{project}-test` binary imports the library and runs it through Triple Sims. No external test framework.

**Result:** Zero external test frameworks across 16 repositories.

**Commit:** `a4989c1`

---

### Visual Regression Orchestrator (March 2026)

**Invention:** First run auto-creates baselines. Subsequent runs compare pixel-by-pixel with configurable tolerance. Failures produce red-highlight diff PNGs.

**Commit:** `8724b66`

---

## Entries

### 2026-05-18 — v0.3.0: Focusing Release

**What:** 6 changes:
1. Canonical public API names alongside P13 aliases
2. Sim 4 baseline staging — screenshots go to `baselines_pending/`, not auto-promoted
3. Two-binary release tripwire — `exopack::deny_release_with_tests!()` proc macro
4. Workspace-aware `standards_check` — descends into workspace member crates
5. CLI surface hardened — `exopack standards`, `exopack baselines accept`, etc.
6. `ats_fixtures` pulled into core — 5-vendor mock ATS HTML generator

**Commits:** `88bdc69`, `5c329cc`, `8b6a575`, `7a14bb4`

---

### 2026-05-13 — Integration Test Surface Refresh

**What:** `tests/ats_fixtures.rs` (8 cases) pins cross-vendor ATS contract. `tests/harvest.rs` (7 cases) covers harvest module with zero prior coverage. POA refreshed: 49→71 unit, 7→24 integration.

---

### 2026-04-09 — MSRV Bump to 1.85 + Human Revelations Documentation Pass

**Commits:** `33ceb9e`, `ef0eb32`

---

### 2026-04-02 — Standards Check: Rust Industry Standards Quality Gate

**What:** `standards_check` module (f100–f116, t70–t72). 14 checks per project. Portfolio integration test across 10 projects (140 total checks). First run: 72/140 passed.

---

### 2026-03-31 — Truth Audit: Supply Chain Security + File Cleanup

**What:** `cargo audit` (1 non-exploitable vuln), `cargo outdated` (all current), removed 277KB unused font. Fixed stale metrics.

**Commit:** `521af17`

---

### 2026-03-29 — Multi-Arch: macOS ARM + Linux x86_64

**What:** macOS ARM (362 KB), Linux x86_64 (384 KB). Both on GitHub Release v0.1.0.

**Commit:** `b3817f3`

---

### 2026-03-28 — Crates.io Metadata + Govdocs CLI

**What:** `cargo publish --dry-run` passes. `exopack govdocs` subcommand prints 11 compliance docs from binary. `exopack --sbom` outputs SPDX 2.3.

**Commits:** `542611b`, `036bd11`

---

### 2026-03-27 — Sim 4 Visual Regression Orchestrator

**What:** `f73` captures screenshots, auto-creates baselines on first run, pixel-level comparison, red-highlight diff PNGs.

**Commit:** `8724b66`

---

### 2026-03-27 — P13 Tokenization + Binary Size

**What:** 28 functions → f60–f95, 8 types → t60–t67. Release profile: opt-level='z', lto, panic='abort', strip. Binary: 314 KB.

**Commit:** `ba1ff82`

---

### 2026-03-11 — Initial Release: Full Testing Framework

**What:** 8 modules in one sprint: TRIPLE SIMS, screenshot, DevTools (headless Chromium), mock (WireMock-style), interface (random port harness), video (xcap), demo (record/replay), baked_demo (zero-input automation). 2,286-line architecture guide.

**Commit:** `a4989c1`

---

*[Full history on GitHub](https://github.com/cochranblock/exopack/commits/main)*
