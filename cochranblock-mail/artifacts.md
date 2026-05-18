---
title: "cochranblock-mail — Proof of Artifacts"
layout: default
---

# Proof of Artifacts — cochranblock-mail

*Sovereign SMTP/IMAP server. One binary, zero cloud.*

[← Back to docs index](../) | [Timeline of Invention](timeline) | [GitHub repo](https://github.com/cochranblock/cochranblock-mail)

---

## What It Is

Single Rust binary implementing SMTP (send/receive) and IMAP (client access). No cloud relay, no SaaS dependency, no per-seat pricing.

Runs on the IRONHIVE cluster. All mail stays on owned hardware.

## Architecture

```
Internet → MX record → cochranblock-mail :25 (SMTP)
Client   → cochranblock-mail :993 (IMAP TLS)
```

*[GitHub repository](https://github.com/cochranblock/cochranblock-mail)*
