# Lab 003: User role controlled by request parameter

## 1. Overview & Metadata

- **Platform**: PortSwigger Web Security Academy
- **Category**: Access control / vertical privilege escalation through a forgeable cookie
- **Difficulty**: Apprentice
- **Lab URL**: [User role controlled by request parameter](https://portswigger.net/web-security/access-control/lab-user-role-controlled-by-request-parameter)
- **Date Documented**: 2026-11-04; exact attempt date/time not separately recorded.
- **Status**: Completed, confirmed by the learner. Tracked conservatively as `Practiced With Help`; independent completion without hints was not established.
- **Evidence**: Learner's account and follow-up confirmation of browser cookie editing, admin-panel access, and deletion of `carlos`. No screenshot or raw HTTP capture was supplied for this lab.
- **Progress Log**: [2026-11-04](../../progress/logs/2026-11-04.md)

## 2. Lab Objective

Use the lab's forgeable administrator cookie to access privileged functionality and delete the training account `carlos`.

## 3. Prerequisite Concepts & Hypothesis

- **Prerequisites**: [Sessions and cookies](../../fundamentals/sessions-and-cookies.md), [Burp Suite](../../tools/burp-suite.md).
- **Test rationale, reconstructed from the reported steps**: If the server trusts a browser-controlled admin flag, changing that flag from false to true may allow a normal account to perform an administrative action.

## 4. Step-by-Step Execution & Proof of Concept

1. Logged in using a normal lab account.
2. Intercepted a subsequent request in Burp and changed the admin cookie value from `false` to `true`.
3. Edited the same cookie in the browser's developer tools cookie storage to `true`, as confirmed in follow-up.
4. Accessed the admin panel and deleted `carlos`; the learner confirmed completion.

Illustrative header excerpts only, using the official lab's cookie spelling. These are not verbatim captures; session cookies and other headers are omitted.

Before modification:

```http
Cookie: Admin=false
```

After modification:

```http
Cookie: Admin=true
```

The Burp edit changes the intercepted request. Editing browser storage changes the value available for subsequent matching requests, until changed or overwritten again. The learner edited a cookie, not JavaScript or page HTML.

No baseline denial, HTTP status, exact deletion request, or independently observed solved banner is claimed. The two edits were not tested separately, so this record does not establish that both were necessary.

## 5. Root Cause & Impact

The application trusted an editable client-supplied flag to authorize administrator operations. The normal account could exercise privileged functionality by forging that flag.

**Confirmed impact**: Admin-panel access and deletion of `carlos`, according to the learner. This is vertical privilege escalation through functionality; a permanent change to the account's stored role was not demonstrated.

## 6. Defensive Takeaway

- Determine permissions from trusted server-side account/session data.
- Enforce authorization for every privileged endpoint and action, including user deletion; deny access by default.
- Do not treat an unverified browser-supplied admin flag as proof of permission.
- Verify that ordinary accounts remain denied when the flag is changed, missing, or forged, while legitimate administrators retain intended access. These remediation checks were not performed in this lab.

## 7. Retrospective & Reflections

- **Practiced**: Intercepting and modifying a cookie in Burp, editing browser cookie storage, and confirming a privileged action.
- **Clarification**: This was cookie manipulation. JavaScript comprehension or editing was not demonstrated.
- **Limits**: No independent solve, separate test of each editing method, or additional account compromise claimed.
- **Follow-up**: Review cookies versus browser JavaScript, and explain why the server must enforce permissions.

### References

- [PortSwigger lab](https://portswigger.net/web-security/access-control/lab-user-role-controlled-by-request-parameter) — publication date not stated; accessed 2026-11-04.
- [PortSwigger access control](https://portswigger.net/web-security/access-control) — publication date not stated; accessed 2026-11-04.
