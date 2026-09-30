---
title: L4 Flood Methods Explained: SYN, UDP, ACK & Amplification
description: How Layer4 flood methods work, what each one measures, and why raw Gbps numbers mislead
---

# L4 Flood Methods Explained

Layer4 methods attack the transport layer: connection state and packet-forwarding capacity. They don't care what the application is — they care about bandwidth, packet-per-second limits, and connection tables.

## The four core mechanisms

### 1. SYN Flood

Sends TCP handshakes (SYN packets) without completing them. The target allocates half-open connection state for each, eventually exhausting the connection table.

**What it tests:** SYN cookie implementation, half-open connection limits, state-table capacity.

**Weakness:** any modern stack with SYN cookies absorbs this losslessly. Against unprotected legacy systems it still works — which is exactly why you test: to find out which one you have.

### 2. UDP Flood

Saturates bandwidth with UDP packets. No handshake needed, trivially parallelizable.

**What it tests:** raw bandwidth and the packet-per-second forwarding capacity of routers/switches/firewalls. Many devices fail on PPS long before Gbps — a 10Gbps-rated firewall may choke at 2M pps of small packets.

**Key insight:** PPS beats Gbps. Small packets (64-byte) at high rate stress forwarding ASICs harder than large packets at the same bandwidth.

### 3. ACK / ACK+PSH Exhaustion

Sends ACK packets for connections that don't exist, or floods established connections with ACK+PSH. Forces the target's firewall to process each packet against its state table — a CPU cost per packet, not just bandwidth.

**What it tests:** firewall state-table CPU, per-packet processing limits. Effective against devices that pass SYN floods fine but melt on stateful inspection load.

### 4. Reflection / Amplification

Uses third-party open services (DNS, NTP, memcached) as force multipliers: small spoofed queries, large responses aimed at the target.

**What it tests:** nothing about your defenses that you can't test directly — but historically the biggest single-incident traffic numbers. For validation purposes, treat amplification as the "can our pipe absorb 500Gbps" scenario.

**Note:** amplification testing against third-party reflectors means involving infrastructure you don't own. Restrict validation to your own assets or contracted ranges.

## Choosing by target

| Target | Effective | Why |
|--------|-----------|-----|
| Legacy/unchecked stack | SYN flood | No cookie protection, state table exhausts |
| Modern CDN-fronted site | None of the above directly | L4 dies at CDN edge; test L7 instead |
| Game server (bare TCP) | UDP PPS + ACK exhaustion | Protocol-aware cleaning may still drop naive floods |
| Firewall device itself | Small-packet UDP + ACK | PPS and state-CPU are the real limits |

## Red flags in panel marketing

- "Tbps capacity" without PPS figures
- No per-method duration/concurrency limits published
- No mention of what layer each method actually targets

A panel that can't tell you whether its UDP method is bandwidth-oriented or PPS-oriented doesn't know itself.

## Related

- [L7 Stress Testing Methods](l7-stress-methods.html)
- [Reading a Stress Test Report](report-reading.html)
- [Layer4 Methods Overview](https://zerodawnlab.com/all-methods.html)
