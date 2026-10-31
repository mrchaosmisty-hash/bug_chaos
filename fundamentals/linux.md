# Linux & Command-Line Utilities for Security Researchers

The command line is the primary working environment for reproducible testing, script automation, and response parsing.

---

## 1. Precision HTTP Testing with `curl`

While GUI proxies (Burp/Caido) are great for exploratory analysis, `curl` provides clean, scriptable, and reproducible HTTP interactions.

### Essential Flags
- `-i` / `-I`: Include HTTP response headers / Fetch headers only (`HEAD`).
- `-v` / `--trace`: Verbose debugging, displaying handshake, headers, and raw bytes.
- `-s` / `-S`: Silent mode / Show errors only.
- `-X <METHOD>`: Specify HTTP verb (`POST`, `PUT`, `DELETE`, etc.).
- `-H "Header: value"`: Add or overwrite custom request headers.
- `-d "payload"` / `--data-raw`: Send POST body (sets `Content-Type: application/x-www-form-urlencoded`).
- `-b "name=val"` / `-c cookies.txt`: Send cookies / Store returned cookies in a jar.
- `-x http://127.0.0.1:8080`: Route traffic through Burp Suite or Caido proxy.
- `-k` / `--insecure`: Allow untrusted/self-signed SSL certificates (essential for local labs).

### Practical Examples

#### Send JSON to an authenticated API through a proxy:
```bash
curl -k -x http://127.0.0.1:8080 -X POST "https://lab.target.local/api/v1/profile" \
  -H "Authorization: Bearer [REDACTED_TOKEN]" \
  -H "Content-Type: application/json" \
  -d '{"username":"attacker","role":"admin"}'
```

#### Inspect response headers only:
```bash
curl -s -I "https://example.com"
```

---

## 2. Parsing JSON Responses with `jq`

Modern web apps and APIs communicate via JSON. `jq` extracts and filters nested fields effortlessly.

```bash
# Pretty-print JSON response
curl -s "https://api.example.com/users" | jq .

# Extract specific field from an array of objects
curl -s "https://api.example.com/users" | jq '.[].id'

# Filter users where role is admin
curl -s "https://api.example.com/users" | jq '.[] | select(.role == "admin")'
```

---

## 3. Text Processing & Wordlist Hygiene

| Utility | Common Use Case | Example Command |
| :--- | :--- | :--- |
| `grep` | Search lines matching a pattern | `grep -E "([0-9]{1,3}\.){3}[0-9]{1,3}" logs.txt` |
| `sort -u` | Deduplicate and sort lists | `sort -u endpoints.txt -o endpoints_clean.txt` |
| `cut` | Extract delimited columns | `cut -d',' -f1,3 export.csv` |
| `awk` | Field manipulation & conditional filtering | `awk '{print $2}' burp_history.txt` |
| `sed` | Stream editor / text replacement | `sed 's/http:\/\//https:\/\//g' urls.txt` |

---

## 4. Environment Variables & Safety Hygiene

Never hardcode test tokens or credentials into bash histories or test scripts. Use environment variables:

```bash
# Temporarily export token in active terminal session only
export TARGET_TOKEN="eyJhbGciOi..."
curl -H "Authorization: Bearer $TARGET_TOKEN" "https://lab.local/api"

# Unset when session ends
unset TARGET_TOKEN
```
