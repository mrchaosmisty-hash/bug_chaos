# Server-Side Request Forgery (SSRF) Research

Server-Side Request Forgery (SSRF) allows an attacker to induce the backend server-side application to make HTTP requests to an arbitrary domain or IP chosen by the attacker.

---

## 🎯 High-Risk SSRF Targets
- **Internal Loopback & Admin Interfaces**: Probing services bound strictly to `127.0.0.1` (e.g., Redis, internal metrics, local admin panels).
- **Cloud Metadata Services**: Accessing cloud instance credentials and configuration data (e.g., AWS IMDSv1/v2 at `http://169.254.169.254/latest/meta-data/`).
- **Internal Microservices & Intranet Endpoints**: Probing unauthenticated internal RFC 1918 networks (`10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`).

---

## 📂 Sub-Topics & Reference Notes
- `basic-ssrf.md` *(Planned - Phase 3)*
- `cloud-metadata-ssrf.md` *(Planned - Phase 3)*
- `ssrf-bypass-techniques.md` *(Planned - Phase 3)*
