# Lab 002: Unprotected admin functionality

## 1. Overview & Metadata

- **Platform**: PortSwigger Web Security Academy
- **Vulnerability Category**: Access control / vertical privilege escalation
- **Difficulty**: Apprentice
- **Lab URL**: [Unprotected admin functionality](https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality)
- **Date Documented**: 2026-11-03; exact attempt time not recorded.
- **Status**: Completed with guidance — `Practiced With Help`.
- **Evidence Basis**: Learner's reported steps and supplied screenshot showing the lab title, `/administrator-panel`, "User deleted successfully!" and "Lab Solved".

## 2. Lab Objective

Access the unprotected admin panel and delete the lab user `carlos`.

## 3. Prerequisite Concepts & Hypothesis

- **Prerequisites**: [HTTP basics](../../fundamentals/http-basics.md), [vertical privilege escalation](../../vulnerabilities/access-control/vertical-privilege-escalation.md).
- **Test rationale**: A hidden administrative route may still allow privileged actions if the server fails to enforce authorization. A path found in `robots.txt` provides a discovery lead.

## 4. Step-by-Step Execution & Proof of Concept

1. Opened `/robots.txt` on the authorized lab instance and found the administrative URL, as reported by the learner.
2. Navigated to `/administrator-panel`.
3. Used the panel to delete `carlos`.
4. Observed "User deleted successfully!" and the lab's solved indicator in the supplied screenshot.

This was a browser-based exercise. No Burp capture, HTTP response code, deletion request method, or exact deletion endpoint was supplied; none is inferred here. The screenshot confirms the successful result, while the preceding discovery steps come from the learner's account. The live instance hostname and original screenshot are omitted from this public write-up.

## 5. Root Cause & Impact

The administrative panel and deletion action lacked effective server-side authorization. A visitor could perform an administrator-only action. This demonstrates vertical privilege escalation through privileged functionality; it does not require changing the visitor's stored account role.

**Observed impact**: Unauthorized deletion of the lab account `carlos`. Broader account compromise or access to other systems was not demonstrated.

The `robots.txt` entry helped locate the panel; it was not the permission failure. Removing the entry would leave the underlying flaw intact. An attacker might discover the route through guessing, directory enumeration, or other disclosures. Enumeration was discussed conceptually and was not performed in this exercise.

## 6. Defensive Takeaway

- Enforce authentication and the required server-side permissions on every administrative endpoint and action, including deletion.
- Deny access by default and derive permissions from trusted server-side account/session data.
- Protect the underlying action even if the admin page or navigation link is hidden.
- Verify that anonymous and ordinary accounts cannot read privileged data or execute privileged actions, while authorized admins can. Fixes were not implemented or tested here.

## 7. Retrospective & Reflections

- **Demonstrated with help**: Finding an administrative route through `robots.txt`, opening the panel, and verifying a privileged action through its visible outcome.
- **Learning confirmed**: A successful HTTP status alone is insufficient evidence. Hiding an address does not repair missing authorization.
- **Evidence limits**: No independent solve, brute-force run, or exact HTTP exchange claimed.
- **Next topic**: Parameter-based access control. No additional lab completion is established by this record.

### References

- [PortSwigger lab](https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality) — publication date not stated; accessed 2026-11-03.
- [PortSwigger access control](https://portswigger.net/web-security/access-control) — publication date not stated; accessed 2026-11-03.
