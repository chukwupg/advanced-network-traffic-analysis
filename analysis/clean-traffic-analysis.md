### 📡 DNS Traffic Analysis

**Tool Used**: Wireshark  
**File**: `anon-dns-traffic.pcapng`  
**Filter Used**: `dns`

#### 🔍 Observations:
- DNS queries were made to resolve domain names like `fing.com`, `microsoft.com`, `x.com` to IP address.
- Queries used **UDP port 53**, and all were unencrypted.
- DNS response packets returned IPv4 addresses (A records) for the requested domains.

#### 📑 Details:
- Each DNS request includes the domain name being resolved.
- Responses included multiple IPs in some cases (fing.com).
- DNS traffic is lightweight and readable, exposing the full query and response info.

#### ⚠️ Security Notes:
- Because DNS traffic is unencrypted, attackers can:
  - Monitor domains being visited.
  - Hijack or spoof responses (DNS spoofing).
- This is why secure DNS (DoH or DoT) is often recommended.

#### 🖼️ Screenshot:
[DNS Query/Response](https://github.com/user-attachments/assets/e5da1936-fbc4-4f7e-aad8-e25262f76a9c)
