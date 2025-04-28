## 📡 DNS Traffic Analysis

**Tool Used**: Wireshark  
**File**: `pcaps/clean/anon-dns-traffic.pcapng`  
**Filter Used**: `dns`

### 🔍 Overview

Domain Name System (DNS) traffic was captured during normal web browsing and direct name resolution commands (e.g., `nslookup`, `dig`). DNS plays a critical role in resolving human-readable domain names to machine-readable IP addresses.

During capture:
- Standard DNS queries (`A` record requests) were sent.
- Standard DNS responses contained the corresponding IP addresses.
- Traffic was predominantly over **UDP port 53**, with no encryption.

### 🧪 Observations:

| Feature | Details |
|:--------|:--------|
| Protocol | DNS over UDP |
| Port Used | 53 |
| Encryption | ❌ None |
| Data Exposure | ✔️ Full domain names and IP addresses visible |
| Query Types | A (IPv4 Address Record) |
| Response Types | Multiple A records possible per query |

- Queries used **UDP port 53**, and all were unencrypted.
- Queries included domains such as `fing.com`, `microsoft.com`, `x.com`.
- DNS response packets returned IPv4 addresses (A records) for the requested domains.
- Some responses contained multiple IPv4 addresses for the same domain (multiple A records), indicating use of load balancing or redundancy.

### 📑 Details:
- Each DNS request includes the domain name being resolved.
- Responses included multiple IPs in some cases (eg `fing.com`).
- DNS traffic is lightweight and readable, exposing the full query and response info.

### 🛡️ Security Implications

- **Vulnerabilities**:
  - **Visibility**: Anyone with access to network traffic can easily observe the domains being queried.
  - **DNS Spoofing**: Attackers can forge responses and redirect users to malicious sites.
  - **DNS Hijacking**: Redirection of queries to malicious DNS servers.
  - **DNS Amplification Attacks**:
    - Attackers send small DNS queries with a spoofed victim's IP address.
    - DNS servers respond with much larger replies to the victim, overwhelming their network (Denial of Service).
  - **DNS Reflection Attacks**:
    - The attacker reflects DNS responses off legitimate servers to a victim, amplifying attack power without exposing their own IP.
- **Mitigation**:
  - Using DNS over HTTPS (DoH) or DNS over TLS (DoT) can encrypt DNS traffic and protect user privacy.
  - Secure DNS servers with rate-limiting and response-size controls to reduce amplification risks.

### 🖼️ Visual Evidence

| Type | Screenshot |
|:-----|:-----------|
| DNS | [DNS Query/Response](https://github.com/user-attachments/assets/e5da1936-fbc4-4f7e-aad8-e25262f76a9c) |

*The screenshot shows both the DNS query for `fing.com` and the corresponding IP addresses returned in the DNS response, under the expanded DNS protocol fields.*

### 📚 Technical Deep Dive 

- **DNS Header Analysis**:
  - **Transaction ID**: Unique per query/response pair.
  - **Flags**: Indicates query type (standard, inverse) and status (successful, error).
- **Message Structure**:
  - **Questions**: 1 (domain being queried)
  - **Answers**: 1 or more (multiple A records possible for a single query)
  - **Authorities**: 0 (in simple queries)
  - **Additional Records**: 0 (in simple queries)

- **Multiple A Records**:
  - Some domains return several IPv4 addresses in a single response.
  - This technique supports load balancing, redundancy, and faster failover.

### ✅ Summary

- The captured DNS traffic clearly demonstrates unencrypted name resolution over UDP port 53, highlighting how exposed standard DNS queries are during everyday internet usage.

- Securing DNS queries is critical for maintaining user privacy and preventing redirection attacks.

### 📂 Folder Organization Reminder

| Folder | Content |
|:-------|:--------|
| `pcaps/clean/` | `dns-traffic.pcapng` file |
| `screenshots/clean-traffic` | `anon-dns-traffic.png` screenshot showing DNS query and response |

---
