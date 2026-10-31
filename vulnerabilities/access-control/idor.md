# Insecure Direct Object References (IDOR) — Deep-Dive Research

---

## 1. Definition in Simple Language

Imagine an apartment building where every tenant is given a key labeled with their apartment number. An **Insecure Direct Object Reference (IDOR)** occurs when a tenant simply scratches out their number on their key card, writes their neighbor's number, and the electronic door lock unlocks without verifying who actually owns that key card.

In web applications, an IDOR occurs when an application exposes a direct identifier to an internal database object (such as an ID number, filename, or account UUID) in a request, and relies entirely on client input without validating whether the requesting user is the rightful owner of that record.

---

## 2. Prerequisite Concepts

- [HTTP Request/Response Basics](../../fundamentals/http-basics.md): How URL parameters, request bodies, and headers are transmitted.
- [Sessions and Cookies](../../fundamentals/sessions-and-cookies.md): Understanding how authentication identifies *who* you are, separate from *what* you can access.
- [Database Query Fundamentals](../../fundamentals/databases.md): How primary keys (`id`) look up records in a database table.

---

## 3. Vulnerability Root Cause

The root cause of an IDOR is **missing server-side authorization validation** at the data access layer. The backend fetches an object based directly on a client-supplied identifier without verifying that the object belongs to the currently authenticated session tenant.

### Vulnerable Code Example (Node.js / Express)
```javascript
// VULNERABLE: Retrieves invoice solely by the URL parameter 'id'
app.get('/api/invoices/:id', async (req, res) => {
    const invoiceId = req.params.id;
    // The query checks only the invoice id, completely ignoring req.user.id
    const invoice = await db.query('SELECT * FROM invoices WHERE id = ?', [invoiceId]);
    if (!invoice) return res.status(404).json({ error: 'Invoice not found' });
    
    return res.json(invoice);
});
```

### Secure Code Example (Contextual Ownership Enforcement)
```javascript
// SECURE: Enforces that the invoice belongs to the authenticated user
app.get('/api/invoices/:id', async (req, res) => {
    const invoiceId = req.params.id;
    const currentUserId = req.user.id; // Extracted from verified session/JWT

    const invoice = await db.query(
        'SELECT * FROM invoices WHERE id = ? AND user_id = ?',
        [invoiceId, currentUserId]
    );
    if (!invoice) return res.status(404).json({ error: 'Invoice not found' });

    return res.json(invoice);
});
```

---

## 4. Example HTTP Request and Response

### Step 1: Attacker (User ID 205) views their legitimate invoice
```http
GET /api/v1/invoices?id=10205 HTTP/1.1
Host: billing.target.local
Authorization: Bearer [REDACTED_ATTACKER_TOKEN]
Accept: application/json
```

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
  "invoice_id": 10205,
  "user_id": 205,
  "customer_name": "Attacker User",
  "amount_due": "$45.00"
}
```

### Step 2: Attacker tampers with the `id` parameter to target User ID 204
```http
GET /api/v1/invoices?id=10204 HTTP/1.1
Host: billing.target.local
Authorization: Bearer [REDACTED_ATTACKER_TOKEN]
Accept: application/json
```

```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{
  "invoice_id": 10204,
  "user_id": 204,
  "customer_name": "Victim Executive",
  "amount_due": "$18,500.00",
  "bank_account_last4": "8812"
}
```

*Finding*: The server returns User 204's private financial data to User 205 because it did not verify tenant ownership.

---

## 5. Security Impact

- **Confidentiality (High/Critical)**: Exposure of sensitive personal data, financial records, medical histories, or private messages across the entire user base.
- **Integrity (High)**: If IDOR exists on `PUT`, `POST`, or `DELETE` endpoints, an attacker can modify profiles, change passwords, or delete critical business assets of other users.
- **Availability (Medium/High)**: Bulk unauthorized deletion of records using sequential ID loops.
- **Business Severity**: Regulatory non-compliance (GDPR, HIPAA, PCI-DSS), brand reputation loss, and account takeover chains.

---

## 6. Testing Methodology in an Authorized Environment

```
[Create User A & User B] ──> [Identify Object IDs in User A's Traffic] ──> [Swap Session Token with User B's] ──> [Analyze Response]
```

1. **Setup Two Distinct Test Accounts**: Always register two test accounts with identical permission levels (e.g., `TestUserA` and `TestUserB`).
2. **Catalog Direct Identifiers**: Map every instance where an identifier is transmitted:
   - Query parameters: `?account_id=...`, `?file_id=...`
   - REST URL paths: `/api/users/{id}/settings`, `/documents/{uuid}`
   - JSON request bodies: `{"user_id": 501, "address": "..."}`
   - Custom headers: `X-User-Id: 501`
3. **The Two-Session Token Swap Test**:
   - Capture User A's valid request to access their private object in Burp Repeater.
   - Replace User A's session cookie / Authorization Bearer token with User B's token.
   - Send the modified request.
4. **Behavioral Evaluation**:
   - `200 OK` with User A's private data $\rightarrow$ **IDOR Confirmed**.
   - `403 Forbidden` / `404 Not Found` $\rightarrow$ Access control properly enforced.

---

## 7. Recommended Hands-on Lab

- **Lab Name**: *Insecure direct object references*
- **Platform**: PortSwigger Web Security Academy
- **Difficulty**: Apprentice
- **Lab Link**: [PortSwigger Academy: Insecure Direct Object References](https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references)
- **Objective**: Download customer chat logs by tampering with static file references stored on the server.

---

## 8. Remediation and Defensive Controls

### Primary Fix
Enforce strict server-side authorization checks on every incoming request that references an object. Check that `resource.owner_id == authenticated_user.id`.

### Architectural Defenses
1. **Use Indirect Object References**: Use a session-specific lookup map (e.g., user sees `file_1`, `file_2` which maps on the server to `file_id: 849204`).
2. **Cryptographically Random UUIDs (UUIDv4)**: Replace predictable sequential integers with random UUIDs. *Note*: UUIDs prevent enumeration/guessing, but are not a replacement for authorization checks; if a UUID leaks, an unauthenticated/unauthorized endpoint is still vulnerable.
3. **Policy-Based Access Control (PBAC)**: Implement centralized authorization middleware (e.g., Casbin, Oso, OPA) rather than ad-hoc inline controller checks.

---

## 9. Relevant Recent Public Reports

- **Report Title**: [IDOR leading to viewing private invoices and personal details of any company](https://hackerone.com/reports/2289178)
  - **Platform**: HackerOne
  - **Publication Date**: Verified Public Disclosure (2025/2026 Archive)
  - **URL**: https://hackerone.com/reports/2289178
  - **Summary**: An invoice download endpoint accepted an arbitrary `invoice_id` parameter without verifying whether the requesting user belonged to the organization that owned the invoice.
- **Report Title**: [IDOR on user profile settings update via API v2](https://hackerone.com/reports/2149542)
  - **Platform**: HackerOne
  - **Publication Date**: Verified Public Disclosure (2025 Archive)
  - **URL**: https://hackerone.com/reports/2149542
  - **Summary**: A PUT request allowed updating email notifications by supplying target `user_id` inside the JSON payload despite an otherwise valid bearer token.

---

## 10. Personal Notes & Unanswered Questions

<!-- Space reserved for the learner's personal reflections during study -->
- **Key Takeaways**:
- **What Confused Me**:
- **Next Question to Investigate**:
