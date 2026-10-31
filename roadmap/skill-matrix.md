# Technical Skill Matrix

This skill matrix tracks practical, hands-on capabilities across application security and ethical hacking. 

### Status Definitions
- **`Not Started`**: Topic identified on the roadmap; study not yet initiated.
- **`Learning`**: Conceptual notes and theoretical mechanics reviewed; practical exercises pending.
- **`Practiced With Help`**: Successfully executed in a lab with the aid of walkthroughs, hints, or instructor guidance.
- **`Independently Demonstrated`**: Identified, exploited, and verified in an authorized, unassisted scenario with clear documentation.

> **Integrity Rule**: Never upgrade a skill to *Practiced With Help* or *Independently Demonstrated* without linking to a concrete lab note in [`labs/`](../labs/) or daily log in [`progress/logs/`](../progress/logs/).

---

## 1. HTTP & Protocol Fundamentals

| Skill / Competency | Current Status | Verification Note / Lab Link | Last Updated |
| :--- | :---: | :--- | :--- |
| Raw HTTP Request/Response Parsing | `Not Started` | — | 2026-10-31 |
| HTTP Status Code Semantics (2xx, 3xx, 4xx, 5xx) | `Not Started` | — | 2026-10-31 |
| Request Methods (GET, POST, PUT, DELETE, PATCH, OPTIONS) | `Not Started` | — | 2026-10-31 |
| Header Manipulation (`Cookie`, `Authorization`, `Host`, etc.) | `Not Started` | — | 2026-10-31 |
| TLS/SSL Handshake & Certificate Mechanics | `Not Started` | — | 2026-10-31 |
| Same-Origin Policy (SOP) & CORS Mechanics | `Not Started` | — | 2026-10-31 |
| Cookie Attributes (`HttpOnly`, `Secure`, `SameSite`) | `Not Started` | — | 2026-10-31 |

---

## 2. Security Tooling & Environment

| Skill / Competency | Current Status | Verification Note / Lab Link | Last Updated |
| :--- | :---: | :--- | :--- |
| Burp Suite Proxy Setup & CA Certificate Installation | `Not Started` | — | 2026-10-31 |
| Burp Repeater Request Modification & Replay | `Not Started` | — | 2026-10-31 |
| Burp Intruder / Caido Automate (Fuzzing workflows) | `Not Started` | — | 2026-10-31 |
| Browser DevTools (Network, Console, Storage, Breakpoints) | `Not Started` | — | 2026-10-31 |
| Linux CLI for Web Testing (`curl`, `jq`, `grep`, regex) | `Not Started` | — | 2026-10-31 |
| Nuclei Template Inspection & Safe Execution | `Not Started` | — | 2026-10-31 |

---

## 3. Access Control & Authorization

| Skill / Competency | Current Status | Verification Note / Lab Link | Last Updated |
| :--- | :---: | :--- | :--- |
| Insecure Direct Object References (IDOR) - Horizontal | `Not Started` | — | 2026-10-31 |
| Vertical Privilege Escalation (User -> Admin) | `Not Started` | — | 2026-10-31 |
| Unprotected Administrative Endpoints | `Not Started` | — | 2026-10-31 |
| Role Modification via Profile Mass Assignment | `Not Started` | — | 2026-10-31 |
| Broken Object-Level Authorization (BOLA in APIs) | `Not Started` | — | 2026-10-31 |

---

## 4. Authentication & Session Management

| Skill / Competency | Current Status | Verification Note / Lab Link | Last Updated |
| :--- | :---: | :--- | :--- |
| Username Enumeration via Response Timing/Messages | `Not Started` | — | 2026-10-31 |
| Password Reset Flow Weaknesses | `Not Started` | — | 2026-10-31 |
| Session Token Entropy & Predictability Analysis | `Not Started` | — | 2026-10-31 |
| Multi-Factor Authentication (MFA) Bypass Logic | `Not Started` | — | 2026-10-31 |
| JWT Validation & Signature Forgery Inspection | `Not Started` | — | 2026-10-31 |

---

## 5. Injection & Data Layer Flaws

| Skill / Competency | Current Status | Verification Note / Lab Link | Last Updated |
| :--- | :---: | :--- | :--- |
| SQL Injection (In-band / UNION-based) | `Not Started` | — | 2026-10-31 |
| SQL Injection (Boolean Blind & Time-Based) | `Not Started` | — | 2026-10-31 |
| OS Command Injection (Input concatenation) | `Not Started` | — | 2026-10-31 |
| Blind OS Command Injection (Out-of-Band / Time Delay) | `Not Started` | — | 2026-10-31 |

---

## 6. Client-Side Vulnerabilities

| Skill / Competency | Current Status | Verification Note / Lab Link | Last Updated |
| :--- | :---: | :--- | :--- |
| Reflected Cross-Site Scripting (HTML / Attribute context) | `Not Started` | — | 2026-10-31 |
| Stored Cross-Site Scripting | `Not Started` | — | 2026-10-31 |
| DOM-Based Cross-Site Scripting (Source to Sink tracing) | `Not Started` | — | 2026-10-31 |
| Cross-Site Request Forgery (CSRF) & Token Validation | `Not Started` | — | 2026-10-31 |
| CORS Misconfiguration Exploitation | `Not Started` | — | 2026-10-31 |

---

## 7. Business Logic, Concurrency & Server-Side Flaws

| Skill / Competency | Current Status | Verification Note / Lab Link | Last Updated |
| :--- | :---: | :--- | :--- |
| Multi-Step Workflow Bypass & State Tampering | `Not Started` | — | 2026-10-31 |
| Price & Quantity Tampering (Integer limits/negative values) | `Not Started` | — | 2026-10-31 |
| Server-Side Request Forgery (Basic Local / Cloud Metadata) | `Not Started` | — | 2026-10-31 |
| Limit Overrun Race Conditions (Single-packet technique) | `Not Started` | — | 2026-10-31 |

---

## 8. Reporting & Vulnerability Research

| Skill / Competency | Current Status | Verification Note / Lab Link | Last Updated |
| :--- | :---: | :--- | :--- |
| Reproducible Step-by-Step PoC Documentation | `Not Started` | — | 2026-10-31 |
| CVSS 3.1 / 4.0 Vector Calculation & Impact Scoping | `Not Started` | — | 2026-10-31 |
| HackerOne Hacktivity Disclosed Report Analysis | `Not Started` | — | 2026-10-31 |
| Redaction & Sensitive Data Scrubbing | `Not Started` | — | 2026-10-31 |
