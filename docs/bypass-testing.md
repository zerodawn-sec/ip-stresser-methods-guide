---
title: Bypass Testing: CDN Origin Discovery, TLS Fingerprints & JS Challenges
description: How bypass testing works — finding the origin behind a CDN, defeating TLS fingerprint blocks, surviving JS challenges
---

# Bypass Testing: CDN & WAF

"Bypass" in stress testing means reaching the layer actually being protected. When a site sits behind a CDN, hitting the CDN edge tests Cloudflare — not your server. Bypass methods find the origin and test the real target.

## Part 1: Origin discovery

You can't test what you can't find. Common origin-exposure paths:

| Technique | How it leaks |
|-----------|-------------|
| Historical DNS records | Passive DNS databases remember pre-CDN IPs |
| Subdomain sweep | Staging/mail/FTP subdomains often resolve directly to origin |
| SSL certificate logs | Certificate transparency reveals certs tied to origin IPs |
| Mail headers | Outbound mail often exits from the origin server |
| Misconfigured services | Direct-IP vhosts, open status pages on origin |

**Test discipline:** discovery on your own assets is reconnaissance hygiene. Document every leak you find — each one is a finding your report should include.

## Part 2: TLS fingerprint layer

CDN/WAF front ends terminate TLS and fingerprint clients by handshake behavior (JA3/JA4), HTTP/2 settings frames.

- Scripted clients (curl, python-requests, most booter scripts) have fingerprints already in block databases
- Real browser stacks pass — so testing tools that use genuine browser TLS configurations pass too
- Fingerprint rotation exists but behavioral layering catches lazy rotation

**What to validate:** whether your protection blocks by fingerprint (bypassable by simulation) or by behavior (much harder).

## Part 3: JS challenges & 5-second shield

JavaScript challenges require executing code and returning a computed token. Options for a testing client:

1. **Full JS engine** (headless browser) — executes like a real visitor, obtains clearance tokens
2. **Token replay** — reuse a manually obtained clearance cookie until it expires
3. **Precomputed tokens** — some panels maintain solved-challenge pools

For validation: a protection that only stops token-less clients is one breached-API-key away from useless. Test with challenge-solving traffic.

## Part 4: The honest framing

Bypass capability in a stress testing panel exists for one legitimate reason: **to test the origin directly instead of the CDN**. If your origin can be found and hit directly, your CDN's Tbps of protection is irrelevant — that's the finding that matters.

## Related

- [L7 Stress Testing Methods](l7-stress-methods.html)
- [Cloudflare Bypass Guide](https://zerodawnlab.com/cloudflare-bypass.html)
- [CDN Origin Protection](https://zerodawnlab.com/cdn-bypass.html)
