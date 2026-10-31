# Networking Fundamentals for Application Security

Understanding how data travels between the client and server allows security researchers to identify routing misconfigurations, SSRF boundaries, and interception opportunities.

---

## 1. The TCP/IP Model (AppSec View)

Security testing primarily operates at the top layers, but lower-layer mechanics directly influence attacks like SSRF and cache poisoning:

1. **Application Layer (HTTP, DNS, TLS, SSH)**: Where web security logic lives.
2. **Transport Layer (TCP, UDP)**: Manages reliable, ordered delivery via ports.
   - Handshake: `SYN` $\rightarrow$ `SYN-ACK` $\rightarrow$ `ACK`.
   - Security focus: Port discovery, timeout behaviors, race condition synchronization.
3. **Internet / Network Layer (IP, ICMP)**: Routing packets between IP addresses.
   - Security focus: Loopback interfaces (`127.0.0.1`, `::1`), private ranges (RFC 1918: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`), and link-local cloud metadata (`169.254.169.254`).
4. **Link Layer (Ethernet, Wi-Fi)**: Physical data transfer between local nodes.

---

## 2. Domain Name System (DNS) Mechanics

DNS translates human-readable hostnames (`target.com`) into routable IP addresses (`93.184.216.34`).

### Key Record Types
- **`A`**: Maps domain to IPv4 address.
- **`AAAA`**: Maps domain to IPv6 address.
- **`CNAME`**: Alias to another canonical domain name (audited for subdomain takeover).
- **`TXT`**: Arbitrary text, frequently storing SPF, DKIM, and site verification tokens.

### Security Implications: DNS Rebinding
In SSRF defenses, an application may check if a domain resolves to a public IP before fetching it. If an attacker controls the authoritative DNS server, they can set a TTL of 0 and return a public IP for the initial validation check, followed immediately by `127.0.0.1` for the actual HTTP fetch, bypassing IP-based allowlists.

---

## 3. TLS / SSL Handshake & HTTPS

Transport Layer Security (TLS) ensures confidentiality, integrity, and authenticity between client and server.

### The Handshake Sequence
1. **ClientHello**: Supported TLS versions, cipher suites, and Server Name Indication (SNI).
2. **ServerHello & Certificate**: Chosen cipher suite, server SSL certificate signed by a trusted Certificate Authority (CA).
3. **Key Exchange**: Ephemeral Diffie-Hellman keys exchanged to generate symmetric session keys.
4. **Finished**: Secure symmetric encryption begins.

### Proxy Interception & CA Certificates
When using Burp Suite or Caido to inspect HTTPS traffic, the browser warns of an untrusted certificate because the proxy dynamically signs certificates with its own internal CA. To intercept traffic without warnings, the researcher must install the proxy's root CA certificate into the browser's trust store.

---

## 4. Proxies, Gateways, and Reverse Proxies

- **Forward Proxy**: Sits in front of the client (e.g., corporate proxy, Burp Suite interceptor).
- **Reverse Proxy / Load Balancer / WAF**: Sits in front of backend web servers (e.g., Cloudflare, AWS CloudFront, Nginx).
- **Header Forwarding**: Reverse proxies append headers like `X-Forwarded-For`, `X-Forwarded-Proto`, and `X-Forwarded-Host`. If backend applications implicitly trust these headers from untrusted clients, vulnerabilities like IP-spoofing and host-header poisoning emerge.
