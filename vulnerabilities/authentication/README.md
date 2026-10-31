# Authentication & Credential Management Research

Authentication is the process of verifying that an entity (user, service, device) is who they claim to be.

---

## 🎯 Common Vulnerability Vectors
- **Username Enumeration**: Subtle differences in response status codes, error messages, or response times that reveal valid accounts.
- **Brute Force & Credential Stuffing**: Missing or bypassable rate-limiting, IP-throttling, or CAPTCHA mechanisms.
- **Password Reset Weaknesses**: Predictable reset tokens, reset token leakage via `Referer` or Host Header poisoning, and missing token expiration.
- **Multi-Factor Authentication (MFA) Bypass**: Direct endpoint access skipping MFA challenge steps, brute-forceable 4/6 digit OTP codes, or state reuse.
- **Session Fixation**: Server reuses the pre-authentication session identifier after successful login.

---

## 📂 Sub-Topics & Reference Notes
- `username-enumeration.md` *(Planned - Phase 2)*
- `password-reset-flaws.md` *(Planned - Phase 2)*
- `mfa-bypasses.md` *(Planned - Phase 2)*
