# Sessions, Cookies & JWT Architecture

Because HTTP is a stateless protocol, servers rely on tokens stored on the client to maintain state across successive requests.

---

## 1. Cookie-Based Session Management

In standard session architectures:
1. User authenticates with username and password.
2. Server generates a cryptographically random, high-entropy string (Session ID) and stores it in server-side storage (Redis, database).
3. Server returns the ID via the `Set-Cookie` header.
4. Browser attaches the `Cookie` header on subsequent requests automatically.

```http
HTTP/1.1 200 OK
Set-Cookie: session_id=q489fj209f8j209f; Path=/; Secure; HttpOnly; SameSite=Lax
```

---

## 2. Essential Cookie Security Flags

| Flag | Purpose | Security Impact if Missing |
| :--- | :--- | :--- |
| **`HttpOnly`** | Prevents client-side scripts from reading the cookie via `document.cookie`. | If an application suffers from XSS, any script can immediately exfiltrate the session token. |
| **`Secure`** | Ensures the cookie is transmitted only over encrypted HTTPS connections. | Vulnerable to packet interception over untrusted networks (e.g., public Wi-Fi). |
| **`SameSite=Strict`** | Cookie is never sent on cross-site requests (even following regular links). | Maximum CSRF defense, but impacts external navigation UX. |
| **`SameSite=Lax`** | Default in modern browsers. Cookie withheld on cross-site subrequests (images, iframes, POST forms), but sent on top-level GET navigation. | Mitigates standard POST-based CSRF; vulnerable if state-changing actions accept `GET`. |
| **`SameSite=None`** | Cookie sent in all cross-site contexts; requires `Secure` flag. | Completely exposed to cross-origin requests; requires explicit anti-CSRF tokens. |

---

## 3. JSON Web Tokens (JWT) Architecture

Unlike opaque session IDs, a JWT is a client-side, self-contained token representing claims encoded in base64url format:

$$\text{JWT} = \text{Header} \,.\, \text{Payload} \,.\, \text{Signature}$$

### Visual Structure
- **Header**: Algorithm and token type (`{"alg": "HS256", "typ": "JWT"}`).
- **Payload**: Claims and identity data (`{"sub": "1042", "role": "user", "exp": 1793440000}`).
- **Signature**: Hash or digital signature ensuring tamper resistance.

### Critical Security Vulnerabilities in JWTs:
1. **The `none` Algorithm Vulnerability**: Server accepts `"alg": "none"` without verifying any signature.
2. **Key Confusion (HMAC vs RSA)**: Server expects an asymmetric public key (RSA) for verification, but an attacker crafts a token signed with the HMAC-SHA256 algorithm using the public key as the secret key.
3. **Weak HMAC Secrets**: If HS256 is signed with a weak secret key (e.g., `secret`, `123456`), it can be cracked offline with `hashcat`.
4. **Lack of Expiration / Invalidation**: JWTs remain valid until their expiration claim (`exp`) passes; servers often lack token revocation mechanisms.
