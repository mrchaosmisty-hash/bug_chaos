# Cross-Site Scripting (XSS) Research

Cross-Site Scripting (XSS) occurs when an application includes untrusted data in a web page without proper validation or escaping, enabling an attacker to execute arbitrary client-side JavaScript in a victim's browser context.

---

## 🎯 Primary XSS Types
1. **Reflected XSS**: Malicious script originates from the current HTTP request (e.g., search parameter) and is reflected immediately in the HTTP response.
2. **Stored (Persistent) XSS**: Malicious script is saved permanently in backend storage (database, comments, forum posts) and served to any victim who views the page.
3. **DOM-Based XSS**: Vulnerability exists entirely within client-side JavaScript when an untrusted **Source** flows into an unsafe execution **Sink** without safe sanitization.

---

## 📂 Sub-Topics & Reference Notes
- `reflected-xss.md` *(Planned - Phase 2)*
- `stored-xss.md` *(Planned - Phase 2)*
- `dom-xss.md` *(Planned - Phase 2)*
