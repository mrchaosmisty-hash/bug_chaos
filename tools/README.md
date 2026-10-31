# Security Testing Tools & Environment Hygiene

Tools do not find bugs by themselves; they simply accelerate manual observation and hypothesis testing. This directory documents essential tools, their configurations, and safe operational guidelines.

---

## 🛠️ Core Tooling Reference

| Tool | Category | Core Purpose | Reference Guide |
| :--- | :--- | :--- | :--- |
| **Burp Suite** | Intercepting Proxy | Industry-standard web security testing platform | [`burp-suite.md`](burp-suite.md) |
| **Browser DevTools** | Client Inspection | Analyzing DOM, Network waterfall, JavaScript breakpoints, local storage | [`browser-devtools.md`](browser-devtools.md) |
| **Caido** | Modern Proxy | Rust-based lightweight, fast alternative to Burp Suite | [`caido.md`](caido.md) |
| **Nuclei** | Vulnerability Scanner | Fast template-based scanning for authorized testing & known CVE checks | [`nuclei.md`](nuclei.md) |

---

## 🔒 Scope & Hygiene Principles

1. **Strict Target Scope**: Always configure your proxy's target scope (e.g., `.*\.web-security-academy\.net`) to prevent intercepting your own personal traffic (banking, email) or untargeted websites.
2. **Never Save Sensitive Raw Captures**: Raw proxy captures (`.burp`, `.har`) contain session tokens and private cookies. Exclude them via `.gitignore`.
3. **No Automated Aggression**: When learning, focus on manual inspection in Repeater before touching automated scanners or fuzzers.
