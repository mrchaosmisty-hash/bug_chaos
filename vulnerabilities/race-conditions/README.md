# Race Conditions & Concurrency Vulnerabilities Research

A race condition arises when an application processes multiple concurrent requests that access shared resources or state without appropriate synchronization (locks, atomic database transactions).

---

## 🎯 Key Attack Vectors
- **Limit Overrun Attacks**: Submitting multiple requests simultaneously to redeem a single-use coupon, gift card, or withdrawal limit multiple times before the database writes the updated state.
- **Time-of-Check to Time-of-Use (TOCTOU)**: State changes occurring between when a condition is verified and when the dependent action is completed.
- **Single-Packet Attack (HTTP/2)**: Using HTTP/2 multiplexing to pack multiple requests into a single TCP packet, eliminating network jitter and synchronizing server-side execution to the microsecond.

---

## 📂 Sub-Topics & Reference Notes
- `limit-overrun-attacks.md` *(Planned - Phase 3)*
- `single-packet-attack-technique.md` *(Planned - Phase 3)*
