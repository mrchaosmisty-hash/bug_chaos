# Lab 001: File path traversal, simple case

## 1. Overview & Metadata

- **Platform**: PortSwigger Web Security Academy
- **Vulnerability Category**: File path traversal (directory traversal)
- **Difficulty**: Apprentice
- **Lab URL**: [File path traversal, simple case](https://portswigger.net/web-security/file-path-traversal/lab-simple)
- **Date Documented**: 2026-11-03; attempt date not separately recorded.
- **Status**: Completed with guidance — `Practiced With Help`; no independent solve claimed.
- **Evidence Basis**: Learner's recorded conversation, including the reported Burp response and discussion of its screenshot. Original image/capture was not available for this documentation review; no fresh lab test was performed.

## 2. Lab Objective

Retrieve `/etc/passwd` through the product-image function in the authorized training lab.

## 3. Prerequisite Concepts & Hypothesis

- **Prerequisites**: [HTTP requests and responses](../../fundamentals/http-basics.md), [Linux paths](../../fundamentals/linux.md), [Burp Suite](../../tools/burp-suite.md).
- **Test rationale (reconstructed from the recorded steps)**: If `filename` controls a filesystem path without confinement to the image directory, `../` segments may reach files outside that directory.

## 4. Step-by-Step Execution & Proof of Concept

1. Captured a product-image **GET** request in Burp Suite and identified the `filename` parameter.
2. Sent the request to **Repeater**, replaced the filename with `../../../etc/passwd`, and sent it.
3. Observed **HTTP 200 OK** and Unix account entries consistent with `/etc/passwd` in the response body.

Sanitized request excerpt reconstructed from the conversation; host, cookies, and other headers omitted:

```http
GET /image?filename=../../../etc/passwd HTTP/2
```

Recorded response status:

```http
HTTP/2 200 OK
```

The body contained Unix passwd entries; raw account records are intentionally omitted. The returned file content establishes the disclosure; **200 OK alone would not prove traversal**. No exact response headers or illustrative account records are presented as captured evidence.

## 5. Root Cause & Impact

**Root cause (inferred from behavior)**: User-controlled path segments escaped the intended image directory because the server did not enforce confinement of the resolved file path. Each `../` moves up one directory. Server source code and the actual image-directory path were not inspected.

**Observed impact**: Disclosure of `/etc/passwd` outside the intended image directory. This exposes account metadata, not plaintext passwords. On typical Linux systems, `x` is a placeholder referring to password hashes stored separately in `/etc/shadow`; access to that file was not demonstrated.

**Potential impact**: Other files readable by the application could be exposed. No additional file reads, file writes, code execution, or server takeover were demonstrated.

## 6. Defensive Takeaway

- Prefer server-controlled image IDs mapped to approved files.
- If filenames are necessary, allowlist permitted values, canonicalize the final path, and enforce containment within the approved directory using a directory-aware check, including symlink resolution.
- Use least-privilege filesystem access as defense in depth. Moving sensitive files or blocking only the literal `../` string does not fix path handling.
- Verify a fix with allowed-image and rejected out-of-directory requests; remediation was not tested in this exercise.

## 7. Retrospective & Reflections

- **Demonstrated with help**: Capturing an image request, modifying a parameter in Repeater, and checking the response body alongside its status.
- **Learning point**: File permissions supplement correct path confinement; they do not replace it.
- **Evidence limits**: No independent retest, bypass variants, or remediation verification recorded. The hypothetical 403 discussion was a knowledge check, not a second observed lab result.
- **Next exercise**: Absolute path bypass, proposed only; no attempt or completion claimed.

### References

- [PortSwigger lab](https://portswigger.net/web-security/file-path-traversal/lab-simple) — publication date not stated; accessed 2026-11-03.
- [PortSwigger path traversal and prevention](https://portswigger.net/web-security/file-path-traversal) — publication date not stated; accessed 2026-11-03.
- [Linux passwd(5) manual](https://man7.org/linux/man-pages/man5/passwd.5.html) — manual dated 2025-09-21; accessed 2026-11-03.
