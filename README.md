# IP Stresser & Booter Methods Guide

Practical reference for Layer4/Layer7 stress testing methods: how each method works, what it measures, and when to use it. Built for defenders who run capacity tests on their own infrastructure and for buyers evaluating stress testing panels.

**Read the full guide:** [zerodawn-sec.github.io/ip-stresser-methods-guide](https://zerodawn-sec.github.io/ip-stresser-methods-guide/)

## Docs

| Doc | What it covers |
|-----|----------------|
| [L4 Flood Methods Explained](docs/l4-flood-methods.md) | SYN/UDP/ACK floods, reflection, PPS vs Gbps |
| [L7 Stress Testing Methods](docs/l7-stress-methods.md) | HTTP floods, browser-simulation bypass, CC patterns |
| [Bypass Testing: CDN & WAF](docs/bypass-testing.md) | TLS fingerprinting, JS challenges, origin discovery |
| [Reading a Stress Test Report](docs/report-reading.md) | Metrics that matter, red flags in panel reports |

## Related

- [ZeroDawn English Guides](https://zerodawnlab.com/) — IP stresser & booter guides
- [ZeroDawn 中文站](https://zerodawnsec.com/) — DDoS压力测试指南

## Topics covered

- Layer4: SYN flood, UDP flood, ACK exhaustion, amplification
- Layer7: HTTP/HTTPS floods, CC patterns, browser simulation
- Bypass: JA3/JA4 fingerprinting, 5-second shield, CDN origin discovery
- Panel evaluation: power, concurrency, method coverage, reporting
