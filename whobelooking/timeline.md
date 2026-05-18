---
title: "whobelooking — Timeline of Invention"
layout: default
---

# Timeline of Invention — whobelooking

*Dated, commit-level record of what was built, when, and why.*

[← Back to docs index](../) | [Proof of Artifacts](artifacts) | [GitHub repo](https://github.com/cochranblock/whobelooking)

---

| Date | Commit | Event |
|------|--------|-------|
| 2026-04-07 | — | v0.1.0 — Cloudflare GraphQL visitor pull + rDNS batch + /24 neighbor scanning |
| 2026-04-08 | 32575fa | Headless Chrome CDP integration — screenshots, perf benchmarking |
| 2026-04-08 | 66f3412 | Performance benchmark: FPS, CLS, paint timing, resource count via Chrome DevTools Protocol |
| 2026-04-08 | 4dc2607 | 8 federal API integrations: SAM.gov, USASpending, SBIR, Federal Register, Grants.gov, Contract Awards, Regulations.gov, GSA CALC+ |
| 2026-04-09 | — | sled caching with zstd compression + dedup across all 8 sources |
| 2026-04-10 | 0dbdba2 | CTO OSINT pipeline: HN Algolia, GitHub user search, Reddit, YC (headless), Podcasts (headless). Cross-verification across 2+ sources. Email extraction. Draft generation. |
| 2026-04-21 | — | Live: caught Microsoft (14 IPs, 4 network ranges, 8 days), Google VPN (4 visits, 3 days), IBM (392 hits, 6 days), Two Six Technologies (5 Ashburn IPs) |
| 2026-04-21 | — | Blocked NextGenWebs Spain attacker (88.151.x.x) probing .aws/credentials — detected, /24 blocked at CF edge in 10 minutes |
| 2026-04-22 | d506ff5 | v0.2.0 — web frontend (axum), sled-backed queue |
| 2026-04-22 | 9a4fa7f | CI gate: 34 tests, exopack Triple Sims, queue system tests |
| 2026-04-22 | d266378 | 42 real tests. IP redaction test caught leak (88.151.32.0). |
| 2026-04-22 | 5eb7526 | Interactive ops center demo — auto-plays real cochranblock.org story (8 days: Microsoft → attacker → resume download) |
| 2026-04-22 | 37cebe3 | Cosmic theme: cochranblock palette (#050508 void, #00d9ff cyan, #9d4edd purple, #00ffcc teal). |
| 2026-04-22 | 478ff80 | Stripe Checkout integration |
| 2026-04-22 | d9da014 | Gemini Man pattern — PID lockfile hot reload |
| 2026-04-22 | a655968 | Filesystem queue: pending/approved/rejected/ready folders |
| 2026-04-22 | 2895cb5 | Dynamic download endpoints: drop PDF in ready/{id}.pdf → /download/{id} exists |
| 2026-04-22 | e8ea8ac | Payment flow: free submit, pay at download |
| 2026-04-22 | 14a9c47 | Security: Stripe session_id verification — checks payment_status == "paid" before serving PDF |
| 2026-04-22 | 3ee9cdf | 66 tests: order flow, capacity limits, admin auth, download security |
| 2026-04-22 | 5fff9f8 | 14-point exopack standards gate |
| 2026-04-22 | 0393b72 | Zero clippy warnings. 66 tests, Triple Sims 3/3. |
| 2026-04-22 | — | whobelooking.org + whobelooking.com live. Diamond binary 6.6MB. |
| 2026-05-06 | 7fbfe61 | `/api/scan/run` — fan-out probe of ~80 attack-surface paths, JSON/CSV output, Swagger UI + Claude skill + email delivery |
| 2026-05-09 | 63e9e26 | Free / Unlicense pivot. Order flow removed in favor of self-serve. |
| 2026-05-15 | — | v0.3.0 — WASM-driven `/try` page: drop a log → in-browser parse + DoH PTR + RDAP + classify + standalone HTML report download. |
| 2026-05-15 | — | wbl-detect v0.2.0 — `parse` (5 log formats), `aggregate` (per-IP rollup + 4-class lattice), `report` (cyberpunk standalone HTML), `json_lite`. |
| 2026-05-15 | — | Double-binary standard enforced via `exopack::deny_release_with_tests!()` tripwire. |
| 2026-05-16 | — | KOVA tokenization applied to `wbl-detect` public + internal surface (f400–f470, t100–t151). 14/14 standards, 194 tests, TRIPLE SIMS 3/3. |
| 2026-05-16 | — | OTEL — server-side: `opentelemetry` 0.27 + OTLP/gRPC trace + metric exporter. `wbl.scan.*` + `wbl.enrichment.*` instruments. |
| 2026-05-16 | — | OTEL ingest — `wbl-detect::parse::f425` decodes OTLP/JSON logRecords into `t100` Events. |
| 2026-05-16 | — | Airgap mode — env-driven for isolated networks: `WBL_DOH_URL`, `WBL_RDAP_BASE`, `WBL_ENRICHMENT_URL`, `WBL_FONTS_HREF`, `WBL_AIRGAP_NOTICE`. CSP `connect-src` computed per-request. |
| 2026-05-16 | — | Government / SIEM log format ingest: syslog RFC 3164 + RFC 5424, ELK/Logstash JSON, Splunk JSON. 227 tests, 14/14 standards. WASM 219 KB. |
| 2026-05-17 | — | Remove server-side `/api/scan/run` batch endpoint. Scanning is now entirely WASM-driven; WASM calls `/api/probe` as local proxy. 910 lines removed. |
| 2026-05-17 | — | W3C Extended log, IIS, HAProxy parsers. Raw IP fallback (`f435`): any unstructured file (ACAS/Nessus, email headers, config dumps) has IPs regex-extracted. `render` CLI: log → rDNS-enriched standalone HTML. 241 tests. |
| 2026-05-18 | — | End-to-end smoke test stage: hits running server at localhost:8082, tests `/health`, `/try` CF panel, `/api/cf/pull` validation. 269 unit + smoke stages. |
| 2026-05-18 | — | AWS ALB/ELB (`f436`), Azure diagnostic JSON (`f437`), generic JSON fallback (`f438`) parsers. Azure covers Activity Log, App Gateway, CDN. Generic JSON covers Caddy, Traefik, custom JSON. 255 tests. |
| 2026-05-18 | — | `/api/cf/pull` local proxy: browser calls same-machine whobelooking, server fires CF Analytics GraphQL, returns NDJSON. Token in RAM for one round-trip, never stored. |
| 2026-05-18 | — | GCP Cloud Logging JSON (`f439`), binary pcap v2.4 + hex dump ingest (`f454–f457`). Ethernet/IPv4/IPv6/Linux SLL link types. HTTP from TCP payload. `render` CLI reads as bytes, auto-routes binary. 265 tests. |
| 2026-05-18 | 42c2f63 | Smoke test fix: CF edge returns HTTP 200 with empty NDJSON for unknown tokens, not 401. Assertion changed from status-code to content-type check. 269 unit + 8 smoke, all stages pass. |

---

*[Full history on GitHub](https://github.com/cochranblock/whobelooking/commits/main)*
