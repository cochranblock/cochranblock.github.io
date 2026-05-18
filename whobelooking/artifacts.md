---
title: "whobelooking — Proof of Artifacts"
layout: default
---

# Proof of Artifacts — whobelooking

*Concrete evidence that this project works and catches real threats.*

[← Back to docs index](../) | [Timeline of Invention](timeline) | [GitHub repo](https://github.com/cochranblock/whobelooking)

---

## What This Does

You give it traffic data (Cloudflare logs, access logs, CSV, pcap, syslog). It tells you which companies are silently evaluating your site, which pages they read, whether they're a threat, and how to respond.

**Real example:** cochranblock.org caught Microsoft evaluating it across 14 IPs and 4 network ranges over 8 consecutive days. They downloaded the resume 5 times. Total marketing spend: $0.

## Live Deployments

- whobelooking.org (live)
- whobelooking.com (live)
- Diamond binary: 6.6 MB

## Log Formats Supported

| Format | Parser |
|--------|--------|
| Nginx / Apache combined/common | f400–f410 |
| W3C Extended log | f412 |
| IIS | f413 |
| HAProxy | f414 |
| AWS ALB/ELB | f436 |
| Azure diagnostic JSON | f437 |
| GCP Cloud Logging JSON | f439 |
| Generic JSON (Caddy, Traefik, custom) | f438 |
| Syslog RFC 3164 + RFC 5424 | f420–f424 |
| ELK/Logstash JSON | f426 |
| Splunk JSON | f427 |
| OTLP/JSON logRecords | f425 |
| Binary pcap v2.4 | f454 |
| Hex dump (xxd/Wireshark/tcpdump) | f455 |
| Raw IP fallback (ACAS/Nessus, email headers) | f435 |
| Cloudflare GraphQL pull | `/api/cf/pull` |

## Architecture

```
INPUT (any source)       PIPELINE                  OUTPUT
────────────────         ────────                   ──────
Cloudflare API  ─┐
Access logs     ─┤       IP + path + timestamp
pcap / syslog   ─┘─────► → rDNS batch              HTML report (WASM, in-browser)
                          → RDAP whois              + threat classification
                          → company identification  + SIEM ingest
                          → redb enrichment cache
```

## Privacy Model

`/try` and `/scan` run entirely in the browser via WASM (219 KB). Customer logs never leave their machine.

## Test Coverage

| Date | Tests | Status |
|------|-------|--------|
| 2026-04-22 | 66 | Triple Sims 3/3 |
| 2026-05-16 | 194 | 14/14 standards |
| 2026-05-17 | 241 | Triple Sims 3/3 |
| 2026-05-18 | **269 unit + 8 smoke** | All stages pass |

*[Full PROOF_OF_ARTIFACTS.md on GitHub](https://github.com/cochranblock/whobelooking/blob/main/PROOF_OF_ARTIFACTS.md)*
