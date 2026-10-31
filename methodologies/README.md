# Testing Methodologies & Checklists

This directory contains hypothesis-driven, systematic testing methodologies. Modern bug bounty hunting favors structured curiosity and rigorous workflows over haphazard spraying.

---

## 🧭 Methodology Principles

1. **Hypothesis-Driven**: Never tamper with a parameter blindly. Always form an explicit hypothesis: *"If I replace my user ID with User B's ID, the server will either return a 403 or expose User B's record."*
2. **Context-First**: Map the application workflow and understand business intent before firing payloads.
3. **Reproducibility**: Document every step so findings can be reliably reproduced in an authorized report.

---

## 📂 Available Methodologies

| Checklist | Vulnerability Focus | Document Link |
| :--- | :--- | :--- |
| **Access Control & Authorization** | IDOR, Vertical PrivEsc, BOLA, Horizontal Isolation | [`access-control-methodology.md`](access-control-methodology.md) |
| **Authentication & Sessions** | Login, Password Resets, MFA, JWTs | *(Planned - Phase 2)* |
| **Input Validation & Injection** | SQLi, Command Injection, File Uploads | *(Planned - Phase 2)* |
| **Client-Side Security** | Reflected / Stored / DOM XSS, CSRF, CORS | *(Planned - Phase 2)* |
