# Educational Scripts & Non-Destructive Automation

This directory stores lightweight educational scripts and non-destructive utilities designed to assist with manual security testing, response parsing, and learning workflows.

---

## ⚖️ Script Development Principles

1. **Non-Destructive by Design**: Scripts written here perform passive parsing, header inspection, formatting, or single-request demonstrations. No denial-of-service, flood testing, or uncontrolled crawlers.
2. **Readability Over Complexity**: Code should be clean, well-commented, and easily understandable by a beginner.
3. **No Hardcoded Secrets**: Scripts must read target URLs or test credentials from command-line arguments or environment variables.

---

## 📂 Planned & Available Utilities

| Script | Purpose | Language |
| :--- | :--- | :---: |
| `cookie_flag_checker.py` | Inspects response headers to verify `HttpOnly`, `Secure`, and `SameSite` flags on cookies. | Python 3 |
| `jwt_inspector.py` | Decodes unverified JWT headers and payloads without calling external APIs. | Python 3 |
| `header_diff.py` | Compares HTTP response headers between two privilege tiers (e.g., Admin vs User). | Bash / Python |
