# Learning Roadmap & Milestones

This directory outlines the strategic path from web application fundamentals to independent bug bounty research. The roadmap is designed around realistic 3-hour daily learning blocks, prioritizing deep conceptual understanding and hypothesis-driven manual testing.

---

## 🗺️ Roadmap Architecture

The roadmap is structured into 4 sequential but flexible phases:

```
┌────────────────────────────────────────────────────────┐
│ Phase 1: Protocol Foundations & Access Control         │
│ HTTP/1.1 & HTTP/2, Proxies, IDOR, Privilege Escalation │
└──────────────────────────┬─────────────────────────────┘
                           ▼
┌────────────────────────────────────────────────────────┐
│ Phase 2: Core Web Vulnerabilities & Exploitation       │
│ Authentication, SQLi, Command Injection, XSS           │
└──────────────────────────┬─────────────────────────────┘
                           ▼
┌────────────────────────────────────────────────────────┐
│ Phase 3: Modern Architectures & Logic Flaws            │
│ REST/GraphQL APIs, SSRF, Business Logic, Race Conds    │
└──────────────────────────┬─────────────────────────────┘
                           ▼
┌────────────────────────────────────────────────────────┐
│ Phase 4: Advanced Vectors & Bug Bounty Readiness       │
│ OAuth 2.0, Cache Poisoning, LLM AppSec, Proactive RT   │
└────────────────────────────────────────────────────────┘
```

---

## 📂 Key Files

- **[`milestones.md`](milestones.md)**: Detailed breakdown of each phase, key learning objectives, required labs, and completion criteria.
- **[`skill-matrix.md`](skill-matrix.md)**: Interactive tracker documenting competencies across HTTP mechanics, tooling, vulnerability analysis, and reporting.

---

## 🎯 Guiding Principles

1. **Depth Over Breadth**: Master the underlying mechanics of an HTTP request and server parsing before touching complex vulnerability scanners.
2. **Never Blindly Scan**: Modern security research rewards contextual comprehension of business logic over generic payload spraying.
3. **Evidence-Based Completion**: Milestones and skills are only marked as complete when verified through documented lab notes.
