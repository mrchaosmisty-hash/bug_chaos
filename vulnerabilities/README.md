# Vulnerability Research & Reference Library

This directory houses deep-dive technical research across core web application vulnerability classes. Each vulnerability entry adheres strictly to a standardized 10-point documentation framework to ensure depth, clarity, and practical utility.

---

## 📂 Vulnerability Categories

| Category | Description | Primary Reference Notes |
| :--- | :--- | :--- |
| **[`access-control/`](access-control/README.md)** | Broken authorization, IDOR, vertical/horizontal privilege escalation | [`idor.md`](access-control/idor.md), [`vertical-privilege-escalation.md`](access-control/vertical-privilege-escalation.md) |
| **[`authentication/`](authentication/README.md)** | Flaws in user identity verification, password resets, MFA, and sessions | Brute force, MFA bypass, credential stuffing |
| **[`injection/`](injection/README.md)** | Untrusted input altering interpreter commands (SQLi, Command Injection) | SQL injection, OS command injection |
| **[`xss/`](xss/README.md)** | Cross-Site Scripting (Reflected, Stored, DOM-based) | Client-side script execution in victim browser context |
| **[`ssrf/`](ssrf/README.md)** | Server-Side Request Forgery | Coercing backend servers into fetching internal/cloud resources |
| **[`business-logic/`](business-logic/README.md)** | Flaws in application design assumptions and operational flows | Price tampering, coupon abuse, workflow skips |
| **[`race-conditions/`](race-conditions/README.md)** | Concurrency flaws and state synchronization anomalies | Limit overruns, TOCTOU, single-packet attacks |

---

## 📋 The 10-Point Documentation Standard

Every vulnerability entry in this repository must use the structure defined in **[`_template.md`](_template.md)**:

1. **Definition in Simple Language**: High-level mental model.
2. **Prerequisite Concepts**: Protocol mechanics or architecture required.
3. **Vulnerability Root Cause**: Code and design failures.
4. **Example HTTP Request and Response**: Commented, raw HTTP PoC.
5. **Security Impact**: Realistic technical and business consequences.
6. **Testing Methodology in an Authorized Environment**: Step-by-step hypothesis testing.
7. **Recommended Hands-On Lab**: Authorized exercise (e.g., PortSwigger Academy).
8. **Remediation and Defensive Controls**: Code-level and architectural fixes.
9. **Relevant Recent Public Reports**: Verified 2025/2026 disclosures with links and dates.
10. **Personal Notes & Unanswered Questions**: Learner reflections and questions.
