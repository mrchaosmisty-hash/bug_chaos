# Nuclei: Fast Template-Based Security Auditing

Nuclei is a fast, customizable vulnerability scanner powered by simple YAML-based DSL templates created by ProjectDiscovery.

---

## 1. Core Architecture & Philosophy

Rather than performing black-box brute force scans, Nuclei executes specific, declarative YAML templates that model known CVEs, misconfigurations, and exposure checks.

### Anatomy of a Safe Nuclei Template:
```yaml
id: exposed-git-config

info:
  name: Exposed .git/config File
  author: security-researcher
  severity: medium
  description: Detects if the internal Git configuration file is accessible.

http:
  - method: GET
    path:
      - "{{BaseURL}}/.git/config"

    matchers-condition: and
    matchers:
      - type: word
        words:
          - "[core]"
          - "repositoryformatversion"
        condition: and

      - type: status
        status:
          - 200
```

---

## 2. Safe & Authorized Usage Guidelines

> ⚠️ **CRITICAL WARNING**: Never run automated scanners against production assets without explicit contractual authorization. Blind, high-rate scanning can cause denial of service, trigger firewall bans, or corrupt database state.

### Recommended Command-Line Flags for Authorized Testing
```bash
# Run a SINGLE specific template against an authorized local lab target
nuclei -u https://lab.target.local -t http/misconfiguration/exposed-git-config.yaml -rate-limit 5

# Inspect what requests would be sent WITHOUT actually firing them (Dry Run)
nuclei -u https://lab.target.local -t cves/2025/ -dry-run

# Limit concurrency to prevent service disruption
nuclei -u https://lab.target.local -t exposures/ -c 2 -rate-limit 3
```

---

## 3. Best Practices for Learners
- **Read Templates Before Running Them**: Inspect the YAML file to understand the exact HTTP request being sent and the matcher logic.
- **Write Your Own Templates**: Turn authorized lab exercises or verified findings into custom templates to solidify your understanding of response matching.
