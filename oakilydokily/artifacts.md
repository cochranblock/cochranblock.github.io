---
title: "oakilydokily — Proof of Artifacts"
layout: default
---

# Proof of Artifacts — oakilydokily

*Concrete evidence that this project builds, ships, and is legally compliant.*

[← Back to docs index](../) | [Timeline of Invention](timeline) | [GitHub repo](https://github.com/cochranblock/oakilydokily)

---

## What It Is

Veterinary professional services site. Multi-auth, ESIGN-compliant waivers, federal compliance docs, AI sprite generation via pixel-forge. First paying CochranBlock partnership.

## Build Output

| Metric | Value |
|--------|-------|
| Binary size | 9.1 MB (release, opt-level=z, LTO, strip) |
| Lines of Rust | 3,211 |
| Lines of JavaScript | **0** |
| Routes | 32 registered |
| Unit tests | 15 |
| Integration checks | 59 |
| Federal compliance docs | 13 (baked into binary via include_str!) |
| Auth providers | Google, Facebook, Apple, manual email/password |
| Database | SQLite WAL mode |
| Waiver retention | 2,557 days (7 years, ESIGN compliant) |

## Compliance

- **ESIGN Act (15 U.S.C. 7001-7031):** Separate `signature` field, consent field, 7-year retention, audit trail
- **EO 14028 Supply Chain:** cargo audit clean (RUSTSEC-2026-0049 fixed)
- **Zero JS supply chain risk:** No npm, no node_modules, no client-side code

## QA

| Gate | Result |
|------|--------|
| Triple Sims | 3/3 pass |
| Clippy (-D warnings) | 0 warnings |
| Forge RCE audit | Closed: auth gate + stdin-only JSON |
| ESIGN audit | Pass: signature field + 7-year retention verified |

*[Full PROOF_OF_ARTIFACTS.md on GitHub](https://github.com/cochranblock/oakilydokily/blob/main/PROOF_OF_ARTIFACTS.md)*
