# Business Logic Flaws Research

Business logic vulnerabilities are flaws in the design and implementation of an application that allow an attacker to elicit unintended behavior by manipulating legitimate features in ways the developers did not anticipate.

---

## 🎯 Typical Logic Attack Scenarios
- **Excessive Trust in Client Controls**: Relying on hidden input fields or disabled HTML buttons for pricing, discounts, or role permissions.
- **Workflow State Skipping**: Bypassing intermediate steps in multi-stage workflows (e.g., jumping from cart `/checkout` directly to `/order-confirmation` without payment).
- **Negative Value & Overflow Exploits**: Submitting negative product quantities to reduce total cart value or trigger integer overflow conditions.
- **One-Time Code / Coupon Replay**: Re-applying single-use promo codes across multiple concurrent browser tabs.

---

## 📂 Sub-Topics & Reference Notes
- `workflow-bypasses.md` *(Planned - Phase 3)*
- `financial-logic-flaws.md` *(Planned - Phase 3)*
