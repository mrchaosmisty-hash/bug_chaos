# Burp Suite: Guide & Workflows for Ethical Hackers

Burp Suite by PortSwigger is the industry-standard platform for manual web security testing.

---

## 1. Initial Setup & Scope Definition

### Installing the CA Certificate
1. Launch Burp Suite and verify the proxy listener is active on `127.0.0.1:8080`.
2. Open the built-in browser (or configure your external browser proxy).
3. Navigate to `http://burpsuite` and download the `CA Certificate` (`cacert.der`).
4. In browser settings, search for `Certificates` $\rightarrow$ `Authorities` $\rightarrow$ `Import`. Trust the certificate to identify websites.

### Scope Configuration
To prevent intercepting background operating system and personal traffic:
1. Go to **Target** $\rightarrow$ **Scope settings**.
2. Click **Add** and specify your authorized lab host (e.g., `.*\.web-security-academy\.net$`).
3. In **Proxy** $\rightarrow$ **HTTP history**, click the filter bar and check **"Show only in-scope items"**.

---

## 2. Core Modules & Usage

### A. Proxy & HTTP History
- **Intercept**: Pauses requests in transit. Useful for altering single requests before they leave the browser. Keep **Intercept is off** by default during general browsing to avoid latency.
- **HTTP History**: Logs every HTTP request/response. Filter by MIME type, status code, or search term to trace parameter flow.

### B. Repeater (`Ctrl + R`)
The core tool for manual testing.
- Allows sending arbitrary modifications of an intercepted request repeatedly without re-triggering browser events.
- **Tabs**: Group tabs into folders by endpoint or hypothesis.
- **Inspector Panel**: Easily modify parameters, cookies, and headers without breaking formatting.

### C. Intruder (`Ctrl + I`)
Automated custom attack engine. Use responsibly in authorized environments.
- **Attack Types**:
  - **Sniper**: Uses a single set of payloads, placing each payload into each defined position sequentially. (Ideal for fuzzing single parameters for SQLi/XSS).
  - **Battering Ram**: Uses a single set of payloads, putting the same payload into all positions simultaneously.
  - **Pitchfork**: Uses multiple payload sets, iterating through them synchronously in lockstep (e.g., username list + matching password list).
  - **Cluster Bomb**: Uses multiple payload sets, testing all permutations (e.g., brute-forcing 100 usernames against 100 passwords = 10,000 requests).

---

## 3. High-Value Extensions (BApp Store)

- **Autorize**: Automatically replays requests with a secondary low-privileged session cookie to identify authorization flaws (IDOR, Broken Access Control) effortlessly during manual browsing.
- **Logger++**: Enhanced multi-threaded logging with advanced filtering and regex searches.
- **Param Miner**: Discovers hidden unkeyed parameters, headers, and query parameters.
