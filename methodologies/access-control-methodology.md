# Access Control & Authorization Testing Methodology

A hypothesis-driven, step-by-step checklist for auditing authorization boundaries and object-level permissions in an authorized environment.

---

## 1. Setup & Environment Preparation

- [ ] **Create Test Account Matrix**:
  - `User A` (Low-privilege tenant / Standard User)
  - `User B` (Different low-privilege tenant at the exact same permission tier)
  - `Admin User` (If accessible in local lab environment)
  - `Anonymous / Unauthenticated` session
- [ ] **Configure Proxy Setup**:
  - In Burp Suite or Caido, configure two distinct browser profiles or use container tabs to keep session cookies completely separate.
  - Set explicit target scope to avoid intercepting third-party traffic.

---

## 2. Reconnaissance & Surface Mapping

- [ ] **Map All State-Changing & Sensitive Data Endpoints**:
  - Account profile, billing records, order histories, internal messages, file downloads, settings updates.
- [ ] **Catalog Object Identifiers**:
  - Identify all instances where direct references are passed:
    - URL query parameters (`/view?id=1042`, `/report?uuid=...`)
    - REST path segments (`/api/v1/workspaces/88/members`)
    - JSON request bodies (`{"document_id": "doc_9912"}`)
    - Custom request headers (`X-Tenant-Id: 5`)
- [ ] **Analyze Identifier Predictability**:
  - Are IDs sequential integers (`1001`, `1002`)?
  - Are IDs UUIDs? (Check if UUIDs are leaked elsewhere in public profile endpoints, comments, or team directory endpoints).
  - Are IDs encoded (Base64, Hex, MD5 of email)?

---

## 3. Horizontal Access Control Checklist (IDOR / BOLA)

### Testing Procedure
1. Log in as `User A` and perform the target action (e.g., download receipt `id=5001`).
2. Log in as `User B` and identify `User B`'s receipt ID (e.g., `id=5002`).
3. In Burp Repeater, replay the request for `id=5001` using `User B`'s authentication token/cookie.

- [ ] **Read Endpoints (Data Leakage)**:
  - Can User B read User A's private profile, invoices, files, or messages?
  - Check HTTP response code and body size.
- [ ] **Write / Modify Endpoints (`POST`, `PUT`, `PATCH`)**:
  - Can User B change User A's email, phone number, shipping address, or preferences?
- [ ] **Delete Endpoints (`DELETE`)**:
  - Can User B delete User A's uploaded documents, comments, or active orders?

---

## 4. Vertical Privilege Escalation Checklist

### Testing Procedure
1. Identify all endpoints used exclusively by high-privilege roles (`/admin/users`, `/api/v1/management/export`, `/settings/billing/discount`).
2. Replay these requests using `User A` (Standard User) credentials.
3. Replay these requests without any authentication cookies (`Unauthenticated`).

- [ ] **Direct URL Navigation**:
  - Access administrative routes directly in the browser while logged in as a standard user.
- [ ] **Hidden Parameter Injection (Mass Assignment)**:
  - Intercept profile update or registration requests and test injecting administrative parameters:
    ```json
    {
      "email": "user@test.local",
      "role": "admin",
      "isAdmin": true,
      "tier": "enterprise",
      "group_id": 1
    }
    ```
- [ ] **HTTP Verb Manipulation**:
  - If `POST /admin/delete` returns `403 Forbidden`, test:
    - `GET /admin/delete?id=...`
    - `PUT /admin/delete`
    - `PATCH /admin/delete`

---

## 5. Bypasses & Edge-Case Checks

- [ ] **Path Traversal in URL Routing**:
  - Test `/api/v1/users/..;/admin/settings` or `/api/v1/users/%2e%2e/admin`.
- [ ] **Extension Appending**:
  - Test `/admin/users.json`, `/admin/users.xml`, or `/admin/users/`.
- [ ] **Header Spoofing (Reverse Proxy Bypasses)**:
  - Test adding headers that mimic internal routing:
    - `X-Custom-IP-Authorization: 127.0.0.1`
    - `X-Forwarded-For: 127.0.0.1`
    - `X-Original-URL: /admin`
    - `X-Rewrite-URL: /admin`
- [ ] **IDOR via Numeric Type Manipulation**:
  - Pass arrays instead of integers: `{"id": [1042]}` or `{"id": {"id": 1042}}`.
  - Pass string representation: `{"id": "1042"}`.

---

## 6. Safe Proof of Concept Standard

- **Read-Only Demonstration**: When demonstrating an IDOR, access your own secondary test account's data (`User B` $\rightarrow$ `User A`). **Never** access another real user's private data.
- **Minimal Perturbation**: For write/delete vulnerabilities, modify only records belonging to your second test account.
- **Redaction**: Redact tokens, personal email addresses, and test credentials in the report.
