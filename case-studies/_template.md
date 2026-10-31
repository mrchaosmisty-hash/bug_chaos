# Case Study: [Report Title]

---

## 1. Report Metadata & Provenance

- **Platform**: HackerOne / Bugcrowd / Independent Blog
- **Target Program**: [Program Name]
- **Vulnerability Category**: [e.g., IDOR / BOLA / SSRF]
- **Date Publicly Disclosed**: YYYY-MM-DD (Prioritize 2025/2026 reports)
- **Report URL**: https://hackerone.com/reports/...
- **Reported By**: [Researcher Handle]
- **Reported Severity**: Low / Medium / High / Critical (CVSS: ...)
- **Bounty Awarded**: [Amount or Disclosed N/A]

---

## 2. Plain-English Summary
<!-- Explain what happened in 2-3 sentences that any beginner could understand. -->

---

## 3. Target Architecture & Environment
<!-- What type of system was this? E.g., decoupled React SPA with GraphQL gateway, microservices behind an API gateway, legacy monolithic PHP. -->

---

## 4. The Discovery Workflow
<!-- How did the researcher spot this when others missed it? Did they read JS bundles? Notice an unusual parameter? Observe an edge-case error message? -->
1. **Initial Reconnaissance**:
2. **Observation of Anomaly**:
3. **Hypothesis Formulated**:
4. **Verification**:

---

## 5. HTTP Exchange Reconstruction

### Tampered Request
```http
POST /api/v2/target-endpoint HTTP/1.1
Host: api.victim-company.com
Authorization: Bearer [REDACTED_RESEARCHER_TOKEN]
Content-Type: application/json

{
  "resource_id": "REDACTED_TARGET_UUID",
  "action": "export"
}
```

### Vulnerable Response
```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "status": "success",
  "download_url": "https://s3.amazonaws.com/target-bucket/victim-export.zip"
}
```

---

## 6. Root Cause Analysis
<!-- Why did this bug exist? Architectural oversight? Framework misconfiguration? Developer assumption? -->

---

## 7. Why Automated Scanners Missed It
<!-- Why did SAST, DAST, or automated scanners fail to detect this? (Usually requires multi-tenant authentication context or specific business logic understanding). -->

---

## 8. Defensive Remediation
<!-- What exact fix did the engineering team deploy to resolve the report? -->

---

## 9. Key Lessons for My Research
- **Pattern to look for in future targets**:
- **Testing checklist addition**:
