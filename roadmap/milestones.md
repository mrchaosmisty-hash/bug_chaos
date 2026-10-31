# Learning Milestones & Target Checkpoints

This document tracks progress across the primary phases of the curriculum. Milestones are marked as achieved only when all associated labs and case studies have been completed and verified with documented notes.

---

## 📌 Phase 1: Foundations & Access Control (Weeks 1–3)

> **Objective**: Master the anatomy of HTTP interactions, configure an intercepting proxy environment, and identify broken authorization boundaries (horizontal and vertical).

### Key Milestones
- [ ] **M1.1 — Protocol Literacy**: Read and dissect raw HTTP/1.1 and HTTP/2 traffic. Understand verbs, headers, status codes, and connection state.
- [ ] **M1.2 — Proxy Fluency**: Configure Burp Suite Community / Caido with browser certificates, scope definitions, and Repeater workflows.
- [ ] **M1.3 — Insecure Direct Object References (IDOR)**: Identify numeric, UUID, and indirect object references; test access boundaries across multiple user contexts.
- [ ] **M1.4 — Vertical Privilege Escalation**: Inspect role enforcement mechanisms, hidden administrative routes, and unprotected endpoints.

### Hands-on Lab Checkpoints
- [ ] PortSwigger: *Unprotected admin functionality*
- [ ] PortSwigger: *User role can be modified in user profile*
- [ ] PortSwigger: *Insecure direct object references*
- [ ] PortSwigger: *User ID controlled by request parameter with data leakage in redirect*

---

## 📌 Phase 2: Core Vulnerabilities & Data Layer (Weeks 4–7)

> **Objective**: Understand stateful authentication mechanisms, injection into backend data stores, and script execution within the browser context.

### Key Milestones
- [ ] **M2.1 — Authentication Mechanics**: Inspect password reset flows, brute-force protections, multi-factor authentication (MFA) logic, and session fixation.
- [ ] **M2.2 — SQL Injection (SQLi)**: Differentiate in-band, blind boolean, and time-based SQLi; master parameterized queries as the root defense.
- [ ] **M2.3 — Cross-Site Scripting (XSS)**: Contrast Reflected, Stored, and DOM-based XSS; understand execution contexts, Content Security Policy (CSP), and context-aware escaping.
- [ ] **M2.4 — Command Injection**: Map application inputs to system shells and learn safe subprocess invocation.

### Hands-on Lab Checkpoints
- [ ] PortSwigger: *Username enumeration via different responses*
- [ ] PortSwigger: *SQL injection vulnerability in WHERE clause allowing retrieval of hidden data*
- [ ] PortSwigger: *Reflected XSS into HTML context with nothing encoded*
- [ ] PortSwigger: *Stored XSS into HTML context with nothing encoded*

---

## 📌 Phase 3: APIs, Logic Flaws & State Anomalies (Weeks 8–11)

> **Objective**: Evaluate modern decoupled architectures, examine business logic assumptions, and probe concurrency and server-side request routing.

### Key Milestones
- [ ] **M3.1 — API Authorization & BOLA**: Audit REST endpoints, mass assignment vulnerabilities, and object-property-level authorization.
- [ ] **M3.2 — Server-Side Request Forgery (SSRF)**: Probe internal service boundaries, cloud metadata services (IMDSv1/v2), and URI parsing discrepancies.
- [ ] **M3.3 — Business Logic & Flawed Assumptions**: Identify negative value manipulations, multi-stage workflow skips, and price tampering.
- [ ] **M3.4 — Race Conditions**: Exploit limit overruns and state synchronization gaps using HTTP/2 single-packet attack techniques.

### Hands-on Lab Checkpoints
- [ ] PortSwigger: *Basic SSRF against the local server*
- [ ] PortSwigger: *Excessive trust in client-side controls*
- [ ] PortSwigger: *Limit overrun race conditions*

---

## 📌 Phase 4: Advanced Modern Vectors & Bug Bounty Hunting (Weeks 12+)

> **Objective**: Attack identity federation protocols, cache layers, GraphQL endpoints, AI/LLM interfaces, and assemble professional bug bounty submissions.

### Key Milestones
- [ ] **M4.1 — OAuth 2.0 & OpenID Connect**: Audit redirect URI validation, CSRF state parameters, and token exchange flows.
- [ ] **M4.2 — GraphQL Security**: Introspect schema endpoints, probe nested queries for denial-of-service, and audit field-level authorization.
- [ ] **M4.3 — Web Cache Poisoning & Deception**: Identify unkeyed inputs and cache key discrepancies between edge caches and origin servers.
- [ ] **M4.4 — AI / LLM Application Security**: Audit prompt injection, insecure output handling, and direct tool calling authorization.
- [ ] **M4.5 — Independent Bug Bounty Transition**: Establish a formal reconnaissance and hypothesis pipeline, report drafting workflow, and program selection criteria.

### Hands-on Lab Checkpoints
- [ ] PortSwigger: *Authentication bypass via OAuth implicit flow*
- [ ] PortSwigger: *Finding a hidden GraphQL endpoint*
- [ ] PortSwigger: *Web cache poisoning with an unkeyed header*
