# Browser DevTools for Application Security Researchers

Modern Chromium and Firefox DevTools are potent security analysis tools that require no installation or setup.

---

## 1. Network Tab: Request Auditing & Replay

- **Filtering by Resource Type**: Click `Fetch/XHR` to isolate asynchronous API requests from static assets (images, CSS).
- **"Copy as cURL"**: Right-click any HTTP request $\rightarrow$ `Copy` $\rightarrow$ `Copy as cURL (bash)`. Allows immediate command-line reproduction with exact cookies and headers.
- **Preserve Log**: Check `Preserve log` to prevent network requests from disappearing during page redirects or form submissions.
- **Payload Inspection**: View parsed Query Strings and Form/JSON bodies cleanly formatted.

---

## 2. Storage Tab: State & Token Analysis

Inspect client-side storage mechanisms for sensitive data leaks:
- **Cookies**: Verify attributes (`HttpOnly`, `Secure`, `SameSite`). If `HttpOnly` is missing on session identifiers, mark as a security observation.
- **Local Storage & Session Storage**: Many modern SPAs erroneously store JWTs or refresh tokens in `localStorage`. Since `localStorage` is accessible to any JavaScript running on the origin, an XSS vulnerability translates into instantaneous token theft.

---

## 3. Sources & Debugger: JavaScript Code Review

1. **Pretty-Print Code**: Click the `{}` icon at the bottom of minified JavaScript bundles (`bundle.min.js`) to make client logic readable.
2. **Search Across All Files (`Ctrl + Shift + F`)**:
   - Search for endpoints: `/api/`, `v1/`, `graphql`.
   - Search for sensitive keys: `apiKey`, `token`, `secret`, `authorization`, `firebase`.
   - Search for DOM sinks: `innerHTML`, `document.write`, `location.href`, `eval`.
3. **XHR / Fetch Breakpoints**:
   - Set breakpoints that pause execution whenever a request URL contains a specific string (e.g., `/user/delete`). Inspect call stacks to trace how parameters were constructed.

---

## 4. Console Tab: Client-Side Testing

- Test DOM-based XSS source flows directly.
- Inspect global variables: `window`, `__INITIAL_STATE__`, `ENV`.
- Read active cookies accessible to script: `document.cookie`.
