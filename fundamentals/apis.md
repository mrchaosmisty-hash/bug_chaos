# API Fundamentals for Security Researchers

Modern web applications are decoupled: Single Page Applications (React, Vue, Angular) and mobile apps communicate with backend servers via APIs (Application Programming Interfaces).

---

## 1. REST Architecture & Resource Modeling

Representational State Transfer (REST) organizes functionality around **resources** identified by URIs, manipulated using standard HTTP verbs:

| Endpoint | Method | Action | Common Failure Mode |
| :--- | :---: | :--- | :--- |
| `/api/v1/orders` | `GET` | List orders for current user | Missing tenant scoping; leaks all orders |
| `/api/v1/orders/1042` | `GET` | View order 1042 | **BOLA / IDOR**: viewing another user's order |
| `/api/v1/orders` | `POST` | Create a new order | Price tampering via client-controlled request body |
| `/api/v1/orders/1042` | `PATCH` | Modify order 1042 | **Mass Assignment**: setting `status: "paid"` directly |
| `/api/v1/orders/1042` | `DELETE` | Delete order 1042 | Missing authorization check on deletion logic |

---

## 2. API Authentication & Headers

APIs generally avoid session cookies in favor of stateless authentication headers:

```http
GET /api/v2/account/billing HTTP/1.1
Host: api.target.local
Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9...
X-API-Key: sec_live_9a7d8...
Accept: application/json
```

### Auditing Touchpoints:
- Are API keys treated as secret credentials or exposed in client-side JavaScript bundles?
- Does the endpoint validate that the token belongs to the user requesting the specific resource identifier?
- Does changing the API version (e.g., from `/v2/` to `/v1/`) access deprecated, unpatched endpoints?

---

## 3. Discovering Hidden Endpoints & OpenAPI Schemas

Backend developers frequently rely on OpenAPI (Swagger) to document API contracts. Developers sometimes leave these documentation routes publicly accessible in production.

### Common Paths to Check (Authorized Scope Only):
- `/swagger-ui.html`
- `/api/swagger.json`
- `/openapi.json`
- `/v1/api-docs`
- `/docs`

If discovered, these schemas reveal hidden endpoints, administrative parameters, and expected payload structures without fuzzing.
