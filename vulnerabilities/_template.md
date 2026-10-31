# [Vulnerability Name] — Deep-Dive Research

---

## 1. Definition in Simple Language
<!-- Explain what this vulnerability is using a clear, real-world mental model that a beginner can grasp immediately. -->

---

## 2. Prerequisite Concepts
<!-- List the underlying web concepts, protocols, or mechanisms that must be understood first. Link to fundamentals/ notes where applicable. -->
- Concept 1 (e.g., [HTTP Basics](../fundamentals/http-basics.md))
- Concept 2 (e.g., [Sessions and Cookies](../fundamentals/sessions-and-cookies.md))

---

## 3. Vulnerability Root Cause
<!-- Detail the code-level or architectural flaw that allows the vulnerability to exist. Include a pseudocode or code comparison (vulnerable vs secure). -->

### Vulnerable Code Example
```language
// Code demonstrating the architectural or logic mistake
```

### Explanation of Failure
<!-- Describe why the above fails to enforce security invariants. -->

---

## 4. Example HTTP Request and Response
<!-- Provide raw, annotated HTTP messages illustrating the attack exchange. Redact sensitive values. -->

### Request
```http
POST /path/endpoint HTTP/1.1
Host: target.example.local
Authorization: Bearer [REDACTED_TOKEN]
Content-Type: application/json

{"key": "value"}
```

### Response
```http
HTTP/1.1 200 OK
Content-Type: application/json

{"status": "success"}
```

---

## 5. Security Impact
<!-- Detail consequences across the CIA triad (Confidentiality, Integrity, Availability) and business impact. -->
- **Confidentiality**:
- **Integrity**:
- **Availability**:
- **Business Severity**:

---

## 6. Testing Methodology in an Authorized Environment
<!-- Step-by-step hypothesis-driven discovery and verification procedure. -->
1. **Hypothesis Formulation**:
2. **Identification & Endpoint Mapping**:
3. **Parameter Tampering / Payload Construction**:
4. **Behavioral Analysis & Verification**:

---

## 7. Recommended Hands-on Lab
<!-- Direct reference to an authorized training lab (e.g., PortSwigger Web Security Academy). -->
- **Lab Name**: 
- **Platform**: PortSwigger Web Security Academy / Local Docker
- **Difficulty**: Apprentice / Practitioner / Expert
- **Lab Link**: [PortSwigger Academy Lab Title](https://portswigger.net/web-security/...)

---

## 8. Remediation and Defensive Controls
<!-- How developers and architects permanently resolve and prevent this flaw. -->
### Primary Fix
<!-- Canonical code-level solution (e.g., prepared statements, centralized authorization checks) -->

### Defense-in-Depth
<!-- Secondary layers of protection (e.g., rate limits, WAF rules, least privilege) -->

---

## 9. Relevant Recent Public Reports
<!-- Publicly disclosed vulnerability reports (prioritize 2025 and 2026). Do NOT invent CVEs, dates, or programs. -->
- **Report Title**: 
  - **Platform**: HackerOne / Bugcrowd
  - **Publication Date**: YYYY-MM-DD
  - **URL**: https://...
  - **Summary**: Brief description of the researcher's findings and root cause.

---

## 10. Personal Notes & Unanswered Questions
<!-- Learner space for reflections, lingering questions, and areas needing further testing. -->
- **Key Takeaways**:
- **What Confused Me**:
- **Next Question to Investigate**:
