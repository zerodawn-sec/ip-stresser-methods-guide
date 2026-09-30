---
title: L7 Stress Testing Methods: HTTP Floods, Browser Simulation & CC Patterns
description: Layer7 stress testing methods — request floods, browser-simulation bypass, CC attack patterns and what each validates
---

# L7 Stress Testing Methods

Layer7 methods speak application protocol. They test what L4 floods can't reach: WAF rules, rate limits, application servers, databases. This is where modern protection lives — and where naive tools die.

## Why L7 is the hard layer

Every request must look legitimate to survive: right TLS fingerprint, right headers, right behavioral cadence. A scripted `requests` client fails at the handshake (JA3 flagged), fails at the challenge (no JS engine), fails at behavior (robotic timing). Modern L7 testing is browser emulation engineering.

## The method families

### 1. HTTP/HTTPS Request Flood

High-rate GET/POST against application endpoints.

**What it tests:** WAF rate limits, application server worker pools, database connection limits.

**The catch:** only works if the traffic passes the preceding layers. A flood that dies at the WAF tests nothing but the WAF.

### 2. Browser-Simulation Flood

Full browser emulation: real TLS stack, JS challenge execution, cookie handling, human-like timing.

**What it tests:** the complete protection chain end-to-end. This is the gold standard for L7 validation — if your app survives a browser-grade flood, it survives real incidents.

**Cost:** each "request" is expensive to produce (real browser context), so rates are lower than raw floods. Evaluate panels on legitimacy per request, not raw RPS.

### 3. CC Patterns (Challenge Collapsar)

Sustained, moderate-rate requests that mimic real user sessions: browsing patterns, random dwell times, cart clicks. Designed to slip under rate limits while draining server resources per session.

**What it tests:** behavioral detection, session-cost economics. A site that costs 50ms CPU per session falls to 1/10th the rate of one costing 5ms.

**Why it matters:** CC is the realistic threat model for most commercial sites. Raw floods get cleaned; CC drains.

### 4. Slow Methods (Slowloris-family)

Open connections, send headers/body at a crawl. Each connection occupies a server thread for minutes.

**What it tests:** server timeout configuration, thread-per-connection models. Largely historical against async servers, still lethal against misconfigured classics.

## Matching method to protection stack

| Protection | Naive method result | What actually tests it |
|------------|--------------------|-----------------------|
| Cloudflare free | Scripted flood → blocked at edge | Browser-sim with challenge solving |
| WAF + rate limit | Flood trips limit in seconds | CC pattern under the threshold |
| No protection | Anything works | Skip — fix the basics first |
| API endpoints | GET floods absorbed | POST with valid payloads + auth flow |

## Evaluating a panel's L7 capability

1. **Ask for a trial at low intensity** — legitimate panels allow it
2. **Watch the method list** — "HTTP GET" alone is 2010-era; look for browser-sim, challenge-solving, TLS-configurable methods
3. **Check concurrency reporting** — concurrent sessions ≠ total requests; panels conflating them are inflating numbers

## Related

- [L4 Flood Methods Explained](l4-flood-methods.html)
- [Bypass Testing: CDN & WAF](bypass-testing.html)
- [Game Server DDoS Stress Testing](https://zerodawnlab.com/game-server-ddos-guide.html)
