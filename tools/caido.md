# Caido: Modern Lightweight Intercepting Proxy

Caido is a lightweight, modern web security auditing toolkit written in Rust, built as a resource-efficient alternative to Burp Suite.

---

## 1. Why Caido?

- **Extremely Fast & Lightweight**: Negligible RAM and CPU footprint compared to Java-based proxies.
- **Client/Server Architecture**: The backend daemon can run locally or on a remote VPS, while you interact with it via a desktop client or web browser interface.
- **Modern UI & Workflow**: Designed with developer-friendly keybindings and clean typography.

---

## 2. Core Workflows

### Intercept & HTTP History
- Clean live stream of requests.
- Filter traffic using Caido's custom query language (CQL):
  ```
  req.host.contains("target.local") && resp.code == 200
  ```

### Replay (Equivalent to Burp Repeater)
- Modify headers, methods, and payloads in place.
- Tab-based organization with split-screen side-by-side request and response views.

### Automate (Equivalent to Burp Intruder)
- Fast wordlist fuzzing and parameter cycling.
- Highly performant due to the Rust asynchronous network engine.

---

## 3. Best Practices in Authorized Labs
1. Ensure the Caido CA certificate is installed into your browser.
2. Configure **Project Scopes** to isolate lab findings.
3. Use Caido when running testing on constrained hardware or remote testing instances where Burp Suite's resource demands are prohibitive.
