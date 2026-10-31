# Bug Chaos — Ethical Hacking & Bug Bounty Learning Journey

> A structured, Markdown-first portfolio, research notebook, and reference library documenting the journey from beginner to independent application security researcher.

[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg)](LICENSE)
[![Repository](https://img.shields.io/badge/GitHub-mrchaosmisty--hash%2Fbug__chaos-blue.svg)](https://github.com/mrchaosmisty-hash/bug_chaos)
[![Focus](https://img.shields.io/badge/Daily%20Commitment-3%20Hours-orange.svg)](#learning-methodology)

---

## 🎯 Project Overview & Goals

**Bug Chaos** is a dedicated learning repository designed to systematically bridge the gap between theoretical knowledge and practical, independent bug bounty research. 

### Primary Objectives
1. **Maintainable Markdown-First Architecture**: Clean, structured, link-verified technical documentation designed for long-term reference.
2. **Measurable, Evidence-Based Progress**: Strict tracking of hands-on lab attempts and independent solves without artificial inflation.
3. **Deep Vulnerability Analysis**: Root-cause analysis of core web vulnerability classes using raw HTTP requests and responses.
4. **Authorized Lab Documentation**: Systematic writeups of PortSwigger Web Security Academy and isolated local lab exercises.
5. **Real-World Case Study Research**: Reverse-engineering verified public bug bounty disclosures (focusing on 2025 and 2026 reports).
6. **Hypothesis-Driven Methodologies**: Reusable checklists that guide testing based on application logic rather than blind scanning.

---

## 🧭 Repository Directory Guide

| Directory | Purpose | Key Contents |
| :--- | :--- | :--- |
| [`roadmap/`](roadmap/README.md) | Structured learning progression & skill matrix | [Milestones](roadmap/milestones.md), [Skill Matrix](roadmap/skill-matrix.md) |
| [`fundamentals/`](fundamentals/README.md) | Protocol and foundational web architecture | [HTTP](fundamentals/http-basics.md), [Networking](fundamentals/networking.md), [Linux](fundamentals/linux.md), [JS](fundamentals/javascript.md), [APIs](fundamentals/apis.md), [Databases](fundamentals/databases.md), [Sessions](fundamentals/sessions-and-cookies.md) |
| [`vulnerabilities/`](vulnerabilities/README.md) | 10-point deep-dive vulnerability reference | [Template](vulnerabilities/_template.md), [IDOR](vulnerabilities/access-control/idor.md), [PrivEsc](vulnerabilities/access-control/vertical-privilege-escalation.md), Auth, Injection, XSS, SSRF |
| [`labs/`](labs/README.md) | Authorized exercise documentation | [Lab Template](labs/_template.md), [PortSwigger Academy Labs](labs/portswigger/README.md) |
| [`case-studies/`](case-studies/README.md) | Breakdown of verified public disclosures (2025–2026) | [Case Study Template](case-studies/_template.md), Real-world write-up breakdowns |
| [`methodologies/`](methodologies/README.md) | Hypothesis-driven testing checklists | [Access Control Methodology](methodologies/access-control-methodology.md) |
| [`tools/`](tools/README.md) | Manual testing tools and configuration hygiene | [Burp Suite](tools/burp-suite.md), [DevTools](tools/browser-devtools.md), [Caido](tools/caido.md), [Nuclei](tools/nuclei.md) |
| [`scripts/`](scripts/README.md) | Educational helpers and non-destructive utilities | Protocol inspection and formatting scripts |
| [`reports/`](reports/README.md) | Professional vulnerability reporting templates | [Bug Bounty Report Template](reports/bug-bounty-report-template.md) |
| [`progress/`](progress/README.md) | Daily execution logs & weekly assessments | [Daily Log Template](progress/daily-log-template.md), [Day 1 Log](progress/logs/day-001.md) |

---

## 🔄 Learning Methodology

All learning follows a continuous 6-stage cycle:

```
┌─────────┐     ┌──────────┐     ┌─────────────┐
│  LEARN  │ ──> │ PRACTICE │ ──> │ INVESTIGATE │
└─────────┘     └──────────┘     └─────────────┘
                                        │
┌─────────┐     ┌──────────┐            ▼
│ REVIEW  │ <── │ DOCUMENT │ <── ┌─────────────┐
└─────────┘     └──────────┘     │   EXPLAIN   │
                                 └─────────────┘
```

### The 3-Hour Daily Session Structure
The learner commits approximately **3 focused hours per day**, divided into four high-impact blocks:

```
┌─────────────────┬─────────────────┬─────────────────┬─────────────────┐
│   30 Minutes    │   90 Minutes    │   30 Minutes    │   30 Minutes    │
│  Foundations    │  Hands-On Labs  │  Case Studies   │  Documentation  │
│  RFC / Theory   │ Burp / Academy  │ 2025/2026 Write-│ Daily Log / Log │
│  Architecture   │ Manual Testing  │    ups Review   │   & Reflection  │
└─────────────────┴─────────────────┴─────────────────┴─────────────────┘
```

1. **30m Foundational Understanding**: Read protocol specifications, examine RFCs, understand data flow and browser security models.
2. **90m Practical Exercises**: Solve authorized exercises (PortSwigger Academy / local containers) using manual intercepting proxies.
3. **30m Case Study Investigation**: Dissect recent verified reports from HackerOne Hacktivity to understand how real targets failed.
4. **30m Documentation & Reflection**: Record honest hypotheses, blockers, and lessons in the [Daily Log](progress/daily-log-template.md).

---

## 📊 Skill Matrix Status Overview

Progress is assessed strictly against four verifiable stages:
- **`Not Started`**: Topic identified on the roadmap.
- **`Learning`**: Concept understood theoretically; lab work pending.
- **`Practiced With Help`**: Solved in a lab setting using hints or walkthroughs.
- **`Independently Demonstrated`**: Solved from scratch in an unfamiliar or clean lab environment with documented proof.

*Full details and updated tracking live in the [Skill Matrix](roadmap/skill-matrix.md).*

---

## 📚 Authoritative Research References

To prevent misinformation and maintain industry-standard accuracy, this repository exclusively references recognized authoritative sources:

- **[PortSwigger Web Security Academy](https://portswigger.net/web-security)** — Interactive lab platform and comprehensive vulnerability documentation.
- **[OWASP Web Security Testing Guide (WSTG)](https://owasp.org/www-project-web-security-testing-guide/)** — Premier manual testing methodology.
- **[OWASP Top 10](https://owasp.org/www-project-top-ten/)** & **[OWASP API Security Top 10](https://owasp.org/www-project-api-security/)** — Critical web & API application risks.
- **[HackerOne Hacktivity](https://hackerone.com/hacktivity)** — Verified public bug bounty disclosures.
- **[RFC Editor](https://www.rfc-editor.org/)** — Authoritative standards for HTTP/1.1 (RFC 9112), HTTP/2 (RFC 9113), and URI syntax (RFC 3986).

---

## ⚖️ Ethical-Use & Legal Statement

> **IMPORTANT**: All research, notes, scripts, and methodologies within this repository are created strictly for educational purposes and authorized security evaluations.

1. **Strictly Authorized Targets**: Testing is performed **only** on locally hosted sandboxes (e.g., Docker) or authorized third-party training platforms (e.g., PortSwigger Web Security Academy).
2. **No Unauthorized Scanning**: No automated or active scanning is ever performed against external systems without prior, written, contractual authorization.
3. **Zero Secrets in Version Control**: No authentication cookies, session tokens, passwords, API keys, or unredacted client data are ever committed to this repository. All proof-of-concept material must be sanitized.
4. **Compliance**: All activities adhere to applicable cybersecurity legislation, including the Computer Fraud and Abuse Act (CFAA), local penal codes, and responsible disclosure policies.
