# JavaScript & Browser Security Architecture

JavaScript executes within the user's browser sandbox. Understanding its execution context, access to the Document Object Model (DOM), and the browser's isolation policies is essential for finding client-side vulnerabilities.

---

## 1. The Same-Origin Policy (SOP)

The Same-Origin Policy is the cornerstone of web application security. It prevents scripts running in one origin from reading confidential data from another origin.

### Origin Definition
An **Origin** is defined strictly by the tuple:
$$\text{Origin} = (\text{Protocol}, \text{Host}, \text{Port})$$

Comparing against `https://example.com:443/app/profile`:

| URL | Same Origin? | Reason |
| :--- | :---: | :--- |
| `https://example.com/app/dashboard` | **Yes** | Same protocol, host, and default port (443) |
| `http://example.com/app/profile` | **No** | Different protocol (`http` vs `https`) |
| `https://api.example.com/profile` | **No** | Different host (subdomain differs) |
| `https://example.com:8443/profile` | **No** | Different port (8443 vs 443) |

### What SOP Restricts:
- Script access across `iframe` boundaries (cannot read `contentDocument` of another origin).
- Reading cross-origin HTTP responses generated via `fetch()` or `XMLHttpRequest` (unless permitted by CORS).

### What SOP Permits:
- Embedding resources cross-origin: `<img>`, `<video>`, `<script src="...">`, `<link rel="stylesheet">`.
- Submitting cross-origin HTML forms (`<form method="POST" action="https://victim.com/transfer">`), which is the foundation of **Cross-Site Request Forgery (CSRF)**.

---

## 2. Cross-Origin Resource Sharing (CORS)

CORS is a mechanism allowing servers to selectively relax SOP restrictions using HTTP response headers.

### Core Headers
- `Access-Control-Allow-Origin`: Explicitly declares which origin can read the response (e.g., `https://trusted.com`).
- `Access-Control-Allow-Credentials`: Set to `true` if cookies or authorization headers can be included in cross-origin requests.
- `Access-Control-Allow-Methods`: Permitted HTTP methods (`GET, POST, OPTIONS`).

### High-Impact Misconfigurations:
1. **Dynamic Reflection of `Origin` Header**:
   If the server reflects whatever is in `Origin: https://evil.com` and sets:
   ```http
   Access-Control-Allow-Origin: https://evil.com
   Access-Control-Allow-Credentials: true
   ```
   Any malicious website visited by a logged-in user can steal sensitive profile and session data.
2. **`null` Origin Whitelist**:
   Some backends whitelist `Origin: null` (used by sandboxed iframes or local files). If allowed with credentials, sandboxed iframes can read private responses.

---

## 3. The DOM: Sources & Sinks

In client-side JavaScript, vulnerabilities like DOM-XSS occur when untrusted data flows from an input **source** into an execution **sink** without proper sanitization.

### Common Sources (User-Controlled Inputs)
- `location.search` (URL query parameters)
- `location.hash` (URL fragments after `#`)
- `document.referrer`
- `window.name`
- `postMessage` event listeners

### Common Dangerous Sinks (Execution Points)
- **HTML Sinks**: `element.innerHTML`, `element.outerHTML`, `document.write()`
- **Execution Sinks**: `eval()`, `setTimeout(string)`, `Function()`
- **URL / Navigation Sinks**: `location.href`, `location.replace()`, `window.open()`
