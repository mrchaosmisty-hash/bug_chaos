# Injection Vulnerabilities Research

Injection vulnerabilities occur when untrusted user input is supplied to an interpreter as part of a command or query, tricking the interpreter into executing unintended commands.

---

## 🎯 Primary Injection Sub-Classes
- **SQL Injection (SQLi)**: Untrusted input modifying backend database queries (in-band, blind boolean, time-based, out-of-band).
- **OS Command Injection**: Unsanitized user inputs passed directly into operating system shells (`exec()`, `system()`, `popen()`).
- **Server-Side Template Injection (SSTI)**: User input embedded into template rendering engines (Jinja2, FreeMarker, Velocity, ERB).
- **XPath / LDAP / NoSQL Injection**: Exploitation of non-relational or directory query interpreters.

---

## 📂 Sub-Topics & Reference Notes
- `sql-injection.md` *(Planned - Phase 2)*
- `os-command-injection.md` *(Planned - Phase 2)*
- `ssti.md` *(Planned - Phase 3)*
