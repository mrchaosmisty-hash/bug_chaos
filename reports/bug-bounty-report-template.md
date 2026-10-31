# Bug Bounty Vulnerability Report Template

---

**Report Title**: [Vulnerability Type] on [Target Endpoint / Feature] allows [Specific Impact]

- **Target Asset**: `api.example.com` / `https://example.com/feature`
- **Vulnerability Category**: Broken Object Level Authorization (CWE-639 / OWASP API1:2023)
- **Severity**: Low / Medium / High / Critical
- **CVSS v3.1 Vector**: `CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:U/C:H/I:N/A:N` (Base Score: 6.5)
- **Date**: YYYY-MM-DD
- **Researcher**: [Your Name / Handle]

---

## 1. Summary
A brief 2–3 sentence overview. State the exact endpoint, the parameter involved, and what an attacker can achieve.

*Example*: An Insecure Direct Object Reference (IDOR) exists on the `/api/v1/user/invoices` endpoint. By modifying the `invoice_id` parameter in a GET request, an authenticated attacker can view the full billing records and personal identifying information of any customer on the platform.

---

## 2. Technical Description & Root Cause
Explain the underlying mechanics causing the issue.
- What endpoint is affected?
- What validation step did the server omit?
- Why does the flaw persist across sessions?

---

## 3. Step-by-Step Reproduction Instructions

> *Note for Triagers: The following reproduction steps utilize two test accounts (`AttackerUser` and `VictimUser`) created specifically to demonstrate this issue without accessing real customer data.*

1. Log into the application using the first test account (`VictimUser`) and navigate to the billing panel at `https://example.com/billing`.
2. Intercept the request to download the invoice receipt. Note that the invoice identifier is `10842`.
3. Log out, then log in using the second test account (`AttackerUser`).
4. In Burp Suite / Caido Repeater, send the following HTTP request with `AttackerUser`'s session token, but requesting `VictimUser`'s invoice ID (`10842`):

---

## 4. Raw HTTP Proof of Concept

### Request
```http
GET /api/v1/invoices/10842 HTTP/1.1
Host: api.example.com
Authorization: Bearer [REDACTED_ATTACKER_TOKEN]
Accept: application/json
```

### Response
```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
  "invoice_id": 10842,
  "customer_name": "Victim User",
  "billing_address": "123 Test St, Sandbox City",
  "total_billed": "$450.00",
  "status": "Paid"
}
```

---

## 5. Security Impact

- **Confidentiality**: Unauthorized disclosure of customer PII, billing totals, and addresses across organizations.
- **Integrity**: [State if modification is possible, or state "None (Read-only impact)"]
- **Availability**: None.
- **Business Impact**: Violation of data privacy regulations (GDPR / CCPA) and unauthorized cross-tenant data exposure.

---

## 6. Suggested Remediation

1. **Enforce Tenant Ownership**: Update the database query at the controller layer to verify that the requested `invoice_id` belongs to the `user_id` or `tenant_id` associated with the validated session token.
   ```sql
   SELECT * FROM invoices WHERE id = ? AND tenant_id = ?;
   ```
2. **Adopt UUIDs**: Replace sequential integer IDs with cryptographically random UUIDv4 identifiers to mitigate sequential enumeration.

---

## 7. Submission Checklist
- [ ] Confirmed that all session cookies and tokens are redacted (`[REDACTED]`).
- [ ] Confirmed that only accounts owned by the researcher were targeted.
- [ ] Verified that the finding is fully reproducible from the provided steps.
