# Lab Writeup: [Lab Title]

---

## 1. Overview & Metadata

- **Platform**: PortSwigger Web Security Academy / Local Environment
- **Vulnerability Category**: [e.g., Access Control / IDOR]
- **Difficulty**: Apprentice / Practitioner / Expert
- **Lab URL**: https://portswigger.net/...
- **Date Attempted**: YYYY-MM-DD
- **Solved Independently?**: [ ] Yes (No hints) / [ ] With Walkthrough / [ ] In Progress

---

## 2. Lab Objective
<!-- What specific condition must be satisfied to complete this lab? -->
- Target Goal: 

---

## 3. Prerequisite Concepts & Hypotheses
<!-- What protocol mechanic or behavior is expected here? -->
- **Prerequisites**: [Link to relevant fundamentals or vulnerability doc]
- **Initial Hypothesis**: *If I modify parameter X in request Y from value A to value B, then the application will...*

---

## 4. Step-by-Step Execution & Proof of Concept

### Step 1: Mapping & Interception
<!-- Describe what was observed when interacting with the application legitimately in Burp Suite / Caido. -->

### Step 2: Request Tampering
```http
POST /target/endpoint HTTP/1.1
Host: [TARGET_LAB_ID].web-security-academy.net
Authorization: Bearer [REDACTED]
Content-Type: application/json

{"tampered_param": "target_value"}
```

### Step 3: Server Response & Observation
```http
HTTP/1.1 200 OK
Content-Type: application/json

{"status": "authorized", "data": "flag_or_secret_extracted"}
```

---

## 5. Root Cause Analysis
<!-- Why did this vulnerability exist in the lab application? -->

---

## 6. Defensive Takeaway
<!-- How would this flaw be remediated in a production codebase? -->

---

## 7. Retrospective & Reflections
- **What worked immediately**:
- **Where I got stuck or made false assumptions**:
- **Follow-up question or related technique to explore**:
