# 🌐 HTTP vs HTTPS Traffic Analysis

**Tools Used**:  
- Wireshark  
- `curl` - Used `curl` to send GET requests with credentials via query strings.
- Browser - Visited HTTP and HTTPS websites.

---

## 🔍 Protocol Overview

- **HTTP (Hypertext Transfer Protocol)** uses port `80` and transmits all data in cleartext.
- **HTTPS (HTTP Secure)** uses port `443` and encrypts traffic using TLS to protect data from eavesdropping or tampering.
- Unlike DNS or ICMP, HTTP(S) operates at the application layer, commonly used for web browsing and data exchange.

| Feature            | HTTP                          | HTTPS                           |
|--------------------|-------------------------------|----------------------------------|
| Port               | 80                            | 443                              |
| Encryption         | ❌ None (cleartext)           | ✅ TLS (encrypted)               |
| Data Visibility    | Full payload visible           | Payload encrypted after handshake |
| Login/Credentials  | Easily exposed                 | Protected via encryption         |
| Security Level     | Insecure                       | Secure                           |

---

## 📁 Captured PCAPs

- `pcaps/clean/anon-http-traffic.pcapng`
- `pcaps/clean/anon-https-traffic.pcapng`

---

## 📸 Screenshots

- `screenshots/clean/anon-http-traffic-exposure01.png` – HTTP GET request with readable data  
- `screenshots/clean/anon-http-traffic-exposure02.png` – HTTP GET request with credentials visible
- `screenshots/anon-https-encrypted-handshake01.png` – TLS handshake, no payload visible
- `screenshots/anon-https-encrypted-handshake01.png` – TLS handshake, no payload visible

### 🖼️ Screenshot Description

- **HTTP Screenshot**:
  - `anon-http-traffic-exposure01.png`
    - Captured the **HTTP response packet** from `http://neverssl.com`
    
  - `anon-http-traffic-exposure02.png`
    - Captured the **HTTP GET request** to `httpbin.org`

- **HTTPS Screenshot**:
  - `anon-https-encrypted-handshake01.png`
    - Visited the secure website `https://google.com`
    - Captured the **TLS Client Hello** packet
    
  - `anon-https-encrypted-handshake01.png`
    - Used `curl` from CLI to simulate cleartext credential transfer:
       ```bash
       curl "http://httpbin.org/get?username=admin&password=1234"
    - Captured the **TLS Client Hello** packet 

---

## 🧪 Key Observations

### HTTP:

- Headers, query strings, and full page content are visible in cleartext.
-   - Used CLI based utility `curl` to simulate cleartext data transfer:
     ```bash
       curl "http://neverssl.com"
  - Captured the **HTTP response packet** 
  - Expanded `Hypertext Transfer Protocol` section
  - Showed:
     - `HTTP/1.1 200 OK`
     - `Content-Type: text/html`
     - Visible line based text data (page structure, text/html)
     - No encryption or obfuscation

- In the GET request to `httpbin.org`, credentials were directly seen in:
`GET /get?username=admin&password=1234 HTTP/1.1`
- Wireshark revealed:
  - Method: `GET`
  - URI: `/get?username=admin&password=1234`
  - Headers: `Host`, `User-Agent`, etc.
  - Visible HTML content (page structure, metadata)
- Demonstrates how **credentials and metadata** can be intercepted by attackers.

### HTTPS:

- All request and response contents were encrypted.
- All sensitive data (including query strings and headers) was encrypted.
- Only TLS handshake metadata (e.g., **Server Name Indication (SNI) and cipher suites**) were visible.
- No credential leakage observed.


---

## 🛡️ Security Implications

- **HTTP**
  - Highly vulnerable to:
    - Man-in-the-Middle (MITM) attacks
    - Credential theft via packet sniffing
    - Session hijacking
  - Attackers can intercept or modify content in transit.

- **HTTPS**
  - Protects integrity, confidentiality, and authenticity of the connection.
  - Prevents passive eavesdropping and data tampering.
  - Offers user protection through TLS encryption and server certificates.

---

## ✅ Summary

The comparison of HTTP and HTTPS traffic illustrates the **critical importance of encryption** in protecting user data. By analyzing captured PCAPs, it is clear that HTTPS hides all sensitive information while HTTP exposes it openly, making it a significant security risk in modern networks.

---
