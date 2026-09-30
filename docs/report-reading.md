---
title: Reading a Stress Test Report: Metrics That Matter
description: How to read a stress test report — which metrics indicate real capacity, which are decoration, and the red flags in panel reporting
---

# Reading a Stress Test Report

Every panel shows graphs. Almost none show the numbers that let you conclude anything. This doc is the checklist for reading a stress test report — whether from a panel or your own test rig.

## The three real metrics

### 1. Degradation curve, not peak survival

"Site survived 50Gbps" means nothing. The real question: **at what load did latency degrade 50%? At what load did 5xx appear?**

A good report plots response time (P50/P95/P99) and error rate against offered load. The inflection point is your capacity. Everything past it is survival theater — the CDN absorbed it, your origin never saw it.

### 2. Recovery time

How many seconds after traffic stops until P95 returns to baseline? Protection that "survives" but takes 4 minutes to recover has failed in every way that matters to users.

Recovery time is also where cheap panels cheat: they stop the test, wait for caches to refill, then screenshot "recovered."

### 3. Per-layer attribution

Where did the traffic die? Edge CDN? WAF? App server? A report that says "attacked with X, site stayed up" without layer attribution tells you the test measured nothing. The finding is in which layer saturated first.

## Decorative metrics (ignore these)

- **Raw Gbps sent** — amplification inflates it; CDNs absorb infinite L4
- **Total request count** — 10M requests over an hour is 278 RPS, which is nothing
- **Concurrent "bots"** — means nothing without per-bot legitimacy (see L7 doc)
- **"Uptime during test"** — of what? The homepage? One URL surviving while APIs die is a fail

## Red flags in panel reports

| Flag | What it means |
|------|--------------|
| No offered-load axis | They're hiding that they never pushed past your capacity |
| Screenshot-only evidence | No logs, no raw data, nothing reproducible |
| Peak-only charts | No sustained window = they can't hold the rate |
| No method disclosure | "Proprietary method" = scripted GET floods |
| Instant recovery shown | Recovery under 5s means the test never reached origin |

## The one-question audit

Ask any panel: *"Show me P95 latency and error rate plotted against offered load, sustained for 10 minutes, with the method named."*

Panels that can produce this are rare. That's the point — the question filters for you.

## Related

- [L4 Flood Methods](l4-flood-methods.html)
- [L7 Stress Testing Methods](l7-stress-methods.html)
- [Stresser Lab vs ZeroDawn Comparison](https://zerodawnlab.com/stresserlab-vs-zerodawn.html)
- [IP Stresser Review 2026](https://zerodawnlab.com/ip-stresser-review-2026.html)
