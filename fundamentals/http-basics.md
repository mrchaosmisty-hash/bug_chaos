# HTTP Fundamentals for Security Researchers

HTTP (Hypertext Transfer Protocol) is an application-layer protocol designed to transfer hypermedia documents. To a bug bounty researcher, HTTP is the primary communication fabric: every web vulnerability manifests as a manipulation of HTTP requests and responses.

---

## 1. Anatomy of an HTTP Request

An HTTP/1.1 request is plain text composed of three distinct sections:

```http
POST /api/v1/user/update HTTP/1.1
Host: target.example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)
Authorization: Bearer eyJhbGciOiJIUzI1Ni...
Content-Type: application/json
Content-Length: 36
Connection: close

{"email":"user@victim.com","role":1}
```

### Breakdown:
1. **Request Line**:
   - **Method (Verb)**: `POST` (action requested).
   - **Request Target / Path**: `/api/v1/user/update` (endpoint URI).
   - **HTTP Version**: `HTTP/1.1`.
2. **Request Headers**: Key-value pairs providing metadata about the client, content format, authorization, and caching instructions.
3. **Empty Line (`\r\n\r\n`)**: Mandatory delimiter separating headers from the request body. Missing this produces a malformed request.
4. **Message Body (Payload)**: Optional content (JSON, form-urlencoded, multipart, XML) sent to the server.

---

## 2. Anatomy of an HTTP Response

```http
HTTP/1.1 200 OK
Date: Sat, 31 Oct 2026 09:30:00 GMT
Server: nginx/1.24.0
Content-Type: application/json; charset=utf-8
Content-Length: 48
Set-Cookie: sessionid=s%3Aabc123; HttpOnly; Secure; SameSite=Lax

{"status":"success","message":"Profile updated"}
```

### Breakdown:
1. **Status Line**:
   - **HTTP Version**: `HTTP/1.1`.
   - **Status Code**: `200` (numeric result).
   - **Reason Phrase**: `OK` (human-readable status).
2. **Response Headers**: Metadata describing server software, response content type, encoding, and cookie commands.
3. **Empty Line**: Separates headers from response body.
4. **Response Body**: The requested data or HTML/JSON rendering.

---

## 3. HTTP Methods & Idempotence

| Method | Idempotent? | Safe? | Typical AppSec Relevance |
| :--- | :---: | :---: | :--- |
| `GET` | Yes | Yes | Retrieve resource; query parameters frequently audited for IDOR, SQLi, reflected XSS. |
| `POST` | No | No | Submits data to modify state; prime target for CSRF, business logic bypass, authentication attacks. |
| `PUT` | Yes | No | Replaces an entire resource; target for unauthorized file uploads, mass assignment. |
| `PATCH` | No | No | Partial update; often prone to object-level access control flaws and hidden parameter binding. |
| `DELETE` | Yes | No | Removes a resource; test for missing authorization checks across user contexts. |
| `OPTIONS`| Yes | Yes | Queries server capabilities; crucial for auditing CORS preflight responses. |
| `HEAD` | Yes | Yes | Same as `GET` but without response body; occasionally used to probe cache states. |

*Note*: An operation is **idempotent** if making multiple identical requests has the same side-effect on the server as making a single request.

---

## 4. Status Code Classes

- **`2xx` (Success)**:
  - `200 OK`: Standard success.
  - `201 Created`: Resource generated (common in REST POST calls).
  - `204 No Content`: Action succeeded, no body returned.
- **`3xx` (Redirection)**:
  - `301 Moved Permanently`: Cached redirect.
  - `302 Found` / `307 Temporary Redirect`: Uncached or state-preserving redirects; often leak sensitive parameters in the `Location` header or redirect bodies.
- **`4xx` (Client Errors)**:
  - `400 Bad Request`: Malformed syntax or rejected schema.
  - `401 Unauthorized`: Missing or invalid authentication credentials.
  - `403 Forbidden`: Authenticated, but lacking authorization for this resource (crucial distinction from 401).
  - `404 Not Found`: Endpoint does not exist or hidden by server design.
- **`5xx` (Server Errors)**:
  - `500 Internal Server Error`: Unhandled exception; critical signal during fuzzing indicating unexpected inputs.
  - `502 Bad Gateway` / `504 Gateway Timeout`: Proxy or upstream service communication failure.

---

## 5. Security-Critical HTTP Headers

| Header | Role & Testing Significance |
| :--- | :--- |
| `Host` | Identifies backend virtual host. Audited for Host Header Injection, password reset poisoning, and routing bypasses. |
| `Authorization` | Passes credentials (e.g., `Bearer <JWT>`, `Basic <base64>`). Test token expiration, algorithm tampering, and reuse across tenants. |
| `Cookie` | Client sends stored session identifiers. Audit for entropy, tampering, and session fixation. |
| `Set-Cookie` | Server instructs browser to store cookies. Look for security flags: `HttpOnly`, `Secure`, `SameSite`. |
| `Origin` / `Referer` | Identifies requesting site. Audited in CORS misconfiguration, CSRF token validation bypasses, and open redirects. |
| `X-Forwarded-For` | De-facto header reporting client IP behind proxies. Audited for IP spoofing and rate-limit bypasses. |

---

## 6. HTTP/1.1 vs. HTTP/2

| Feature | HTTP/1.1 | HTTP/2 | AppSec Impact |
| :--- | :--- | :--- | :--- |
| **Format** | Textual / Delimited | Binary Framing | HTTP/2 reduces request parsing ambiguity; HTTP/1-to-2 translation introduces HTTP request smuggling. |
| **Multiplexing** | Pipelining (rarely supported) | True Multiplexing | Allows single-packet attack techniques to execute race conditions with microsecond synchronization. |
| **Header Compression** | None | HPACK | New attack surface around compression side-channels (CRIME). |
