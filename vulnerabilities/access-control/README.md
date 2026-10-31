# Access Control & Broken Authorization Research

Access control (authorization) is the mechanism by which an application decides whether an authenticated user has permission to view, edit, create, or delete a specific resource or execute a privileged action.

---

## 📌 Key Vulnerabilities in This Module

| Vulnerability | Description | Reference Document |
| :--- | :--- | :--- |
| **Insecure Direct Object References (IDOR)** | Horizontal access control failure where an attacker accesses another user's objects by manipulating direct identifier parameters. | [`idor.md`](idor.md) |
| **Vertical Privilege Escalation** | An unprivileged user gains access to administrative or elevated functionality due to missing role checks. | [`vertical-privilege-escalation.md`](vertical-privilege-escalation.md) |

---

## 🔍 Core Access Control Models
1. **Vertical Access Control**: Restricts access based on user hierarchy (e.g., Guest $\rightarrow$ Standard User $\rightarrow$ Manager $\rightarrow$ System Administrator).
2. **Horizontal Access Control**: Restricts access between users at the same privilege level (e.g., User A must not access User B's billing invoice).
3. **Context-Dependent Access Control**: Restricts access based on application state or business rules (e.g., an order cannot be modified once shipped).
