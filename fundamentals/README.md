# Web Fundamentals for Application Security

A firm understanding of core computing and web protocols is the bedrock of application security research. Security vulnerabilities are almost always the unintended byproduct of protocol quirks, browser mechanisms, or developer assumptions colliding with reality.

---

## 📚 Core Modules

| Module | Description | Core Security Relevance |
| :--- | :--- | :--- |
| **[`http-basics.md`](http-basics.md)** | Anatomy of HTTP/1.1 and HTTP/2, verbs, headers, status codes | Intercepting, analyzing, and modifying client-server communications |
| **[`networking.md`](networking.md)** | DNS, TCP, TLS/SSL handshake, ports, proxies | Understanding request routing, man-in-the-middle, and host header routing |
| **[`linux.md`](linux.md)** | Essential command-line utilities (`curl`, `jq`, `grep`) | Scripting non-destructive tests, parsing JSON responses, pipeline analysis |
| **[`javascript.md`](javascript.md)** | DOM manipulation, execution context, SOP, CORS | Finding client-side sinks, understanding DOM-XSS, and SOP boundaries |
| **[`apis.md`](apis.md)** | REST, GraphQL, JSON structures, endpoint routing | Testing modern decoupled SPAs and mobile backends |
| **[`databases.md`](databases.md)** | Relational models, SQL syntax, parameterization | Identifying injection points and defensive architectural patterns |
| **[`sessions-and-cookies.md`](sessions-and-cookies.md)** | Session state, cookie flags (`HttpOnly`, `Secure`, `SameSite`), JWTs | Attacking authentication state, session hijacking, and CSRF |

---

## 🎯 Learning Approach

Do not skim these modules. When examining a vulnerability, always refer back to the protocol specification to understand *why* the browser or server behaves as it does.
