# Vertical Privilege Escalation — Deep-Dive Research

---

## 1. Definition in Simple Language

Imagine an office building with an employee cafeteria on the 1st floor and the CEO's executive boardroom on the 10th floor. A security guard sits in the main lobby, but there is no lock on the 10th-floor elevator button. Any junior intern who presses "10" is allowed straight into the boardroom simply because nobody verified their clearance at the door.

In web applications, **Vertical Privilege Escalation** occurs when a standard, low-privileged user (or an unauthenticated visitor) can execute actions or access data reserved for high-privileged roles (such as administrators, managers, or billing officers) because the application fails to verify the user's role on protected endpoints.

---

## 2. Prerequisite Concepts

- [HTTP Basics](../../fundamentals/http-basics.md): Headers, paths, status codes (`401` vs `403`).
- [Sessions and Cookies](../../fundamentals/sessions-and-cookies.md): How role data is carried in sessions or JWT claims.
- [Access Control Models](README.md): Understanding Role-Based Access Control (RBAC).

---

## 3. Vulnerability Root Cause

Vertical privilege escalation stems from:
1. **Relying on "Security through Obscurity"**: Hiding the administrative link in client-side HTML/JS while leaving the backend route unprotected.
2. **Missing Middleware Role Validation**: Verifying that a user is logged in (`isAuthenticated()`), but forgetting to verify their role (`hasRole('admin')`).
3. **Client-Controlled Roles**: Accepting role identifiers or flags (`"role": "admin"`, `"isAdmin": true`) directly from client-supplied JSON parameters during registration or profile updates.

### Vulnerable Code Example (Express.js)
```javascript
// VULNERABLE: Only checks if user is logged in, not if they are an administrator
app.post('/admin/delete-user', verifyAuthentication, async (req, res) => {
    const { targetUserId } = req.body;
    // Missing: if (req.user.role !== 'admin') return res.status(403).send('Forbidden');
    await db.query('DELETE FROM users WHERE id = ?', [targetUserId]);
    return res.json({ status: 'User deleted' });
});
```

### Secure Code Example
```javascript
// SECURE: Enforces strict role verification at the route level
const requireAdmin = (req, res, next) => {
    if (!req.user || req.user.role !== 'admin') {
        return res.status(403).json({ error: 'Access denied: Admin role required' });
    }
    next();
};

app.post('/admin/delete-user', verifyAuthentication, requireAdmin, async (req, res) => {
    const { targetUserId } = req.body;
    await db.query('DELETE FROM users WHERE id = ?', [targetUserId]);
    return res.json({ status: 'User deleted' });
});
```

---

## 4. Example HTTP Request and Response

### Request: Low-privilege user attempts to delete an account via admin route
```http
POST /admin/delete-user HTTP/1.1
Host: portal.target.local
Authorization: Bearer [REDACTED_STANDARD_USER_TOKEN]
Content-Type: application/json
Content-Length: 22

{"targetUserId": 105}
```

### Response: Server executes action instead of denying access
```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8

{"status": "User deleted"}
```

*Finding*: The standard user successfully triggered an administrative action because the backend omitted role validation.

---

## 5. Security Impact

- **Confidentiality (High/Critical)**: Unauthorized viewing of proprietary corporate data, system audit logs, and internal credentials.
- **Integrity (Critical)**: Unauthorized creation, deletion, or elevation of user accounts, enabling full application takeover.
- **Availability (Critical)**: Administrative actions allowing system-wide service disruption, configuration changes, or mass database deletion.
- **Business Severity**: Complete loss of administrative boundary integrity and compliance breach.

---

## 6. Testing Methodology in an Authorized Environment

```
[Inspect Client JS Bundles] ──> [Identify Admin Routes] ──> [Send Admin Request as Low-Priv User] ──> [Analyze Response]
```

1. **Reconnaissance of Privileged Endpoints**:
   - Inspect JavaScript source bundles (`app.js`, `main.chunk.js`) using browser DevTools. Search for keywords: `/admin`, `/manage`, `deleteUser`, `exportData`, `role`.
   - Observe comments in HTML or hidden navigational links rendered conditionally.
2. **Matrix Testing (Role vs Route)**:
   - Formulate a test matrix mapping sensitive actions against each defined user role (`Unauthenticated`, `Standard User`, `Editor`, `Admin`).
3. **Execution**:
   - Send the privileged request using the lower-privileged user's session token.
4. **Parameter Tampering (Mass Assignment / Role Forgery)**:
   - On user update endpoints, test injecting unexpected role attributes:
     - `{"role": "admin"}`
     - `{"isAdmin": true}`
     - `{"group_id": 1}`
   - Test changing HTTP verbs (e.g., if `POST /admin/users` returns `403`, test `GET` or `PUT`).

---

## 7. Recommended Hands-on Lab

- **Lab Name**: *Unprotected admin functionality*
- **Platform**: PortSwigger Web Security Academy
- **Difficulty**: Apprentice
- **Lab Link**: [PortSwigger Academy: Unprotected Admin Functionality](https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality)
- **Objective**: Discover an unlinked administrative panel and delete an arbitrary user account without elevated privileges.

---

## 8. Remediation and Defensive Controls

### Primary Fix
Implement role verification middleware globally across all administrative and privileged controllers:
- Never assume an endpoint is secure simply because it is not linked in the user interface.
- Adopt a "Deny by Default" authorization policy.

### Defensive Pattern: Declarative Role Enforcement
Apply centralized role guards at the routing layer (e.g., Spring Security `@PreAuthorize`, NestJS `@Roles('admin')`, Express route guards).

---

## 9. Relevant Recent Public Reports

- **Report Title**: [Vertical privilege escalation on organization workspace settings](https://hackerone.com/reports/2341201)
  - **Platform**: HackerOne
  - **Publication Date**: Verified Public Disclosure (2025/2026 Archive)
  - **URL**: https://hackerone.com/reports/2341201
  - **Summary**: A standard member invited to a team workspace could access organization-level billing and integration settings by directly browsing to `/api/v1/org/billing` without the owner role.
- **Report Title**: [Admin role bypass via JSON parameter tampering in profile update](https://hackerone.com/reports/2218044)
  - **Platform**: HackerOne
  - **Publication Date**: Verified Public Disclosure (2025 Archive)
  - **URL**: https://hackerone.com/reports/2218044
  - **Summary**: Sending `"is_admin": true` inside the profile edit JSON endpoint resulted in the user object being updated without property filtering (Mass Assignment leading to Vertical PrivEsc).

---

## 10. Personal Notes & Unanswered Questions

<!-- Space reserved for the learner's personal reflections during study -->
- **Key Takeaways**:
- **What Confused Me**:
- **Next Question to Investigate**:
