---
title: "exopack — Proof of Artifacts"
layout: default
---

# Proof of Artifacts — exopack

*Test augmentation framework. Zero external test frameworks. The binary tests itself.*

[← Back to docs index](../) | [Timeline of Invention](timeline) | [GitHub repo](https://github.com/cochranblock/exopack)

---

## What It Is

Test augmentation library for the CochranBlock portfolio. Implements the two-binary model (prod binary + test binary, same crate), Triple Sims gate, visual regression, screenshot/video capture, mock servers, and standards quality gate.

## Build Output

| Metric | Value |
|--------|-------|
| Binary size | 362 KB (aarch64-apple-darwin) |
| Lines of Rust | ~1,781 |
| Modules | 8 (triple_sims, screenshot, devtools, mock, interface, video, demo, baked_demo) |
| crates.io | Published v0.3.0 |

## Features

| Feature | Description |
|---------|-------------|
| Triple Sims | Run test suite 3x — all must pass. Catches race conditions. |
| Two-binary model | `{project}-test` binary spawns server, runs tests, tears down |
| Sim 4 Visual Regression | Screenshot capture + pixel diff with red-highlight output |
| Baseline staging | First-run screenshots go to `baselines_pending/` (not auto-promoted) |
| Two-binary tripwire | `exopack::deny_release_with_tests!()` — compile error if tests in release |
| Standards check | 14-point quality gate per project |
| ATS fixtures | 5-vendor mock ATS HTML generator (Greenhouse, Lever, Workday, iCIMS, Ashby) |
| Mock server | WireMock-style GET/POST mocks |

## Standards Gate (14 checks)

clippy, fmt, audit, deny, MSRV, unsafe, docs, changelog, license, test binary (P16), allow(unused), error handling, secrets, Cargo.toml metadata.

## Test Coverage

| Date | Unit Tests | Integration Tests |
|------|-----------|------------------|
| v0.1.0 | 17 | 0 |
| v0.2.1 | 49 | 7 |
| v0.3.0 | 71 | 24 |

*[Full PROOF_OF_ARTIFACTS.md on GitHub](https://github.com/cochranblock/exopack/blob/main/PROOF_OF_ARTIFACTS.md)*
