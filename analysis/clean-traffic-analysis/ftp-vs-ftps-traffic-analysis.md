## 🔍 FTP vs FTPS Traffic Analysis

**Tools Used**
  - Wireshark
  - CLI tool - FTP
  - FileZilla

---

### 🌐 Protocol Overview

- **FTP (File Transfer Protocol)** is used for transferring files between client and server. It communicates over port **21 (control)** and **20 (data)**, but lacks encryption.
- **FTPS (FTP Secure)** is an extension of FTP that adds **TLS encryption** for secure communication.
  - FTPS comes in two forms:
    - **Explicit FTPS**: Starts as plain FTP, then upgrades to TLS.
    - **Implicit FTPS**: Assumes TLS from the beginning (usually port 990).
- Both protocols support login, file listing, and transfers — but differ drastically in security.

---

### 🧪 Traffic Generation Process

#### FTP (Insecure)

1. Connected to public test server `ftp test.rebex.net` using the command line interface.
    - **Username**: demo
    - **Password**: password

2. Performed:
    - Login (`USER`, `PASS`)
    - File listing (`ls`)
    - File download (`get readme.txt`)

3. Captured traffic using Wireshark with filter: `ftp || tcp.port == 21`

#### FTPS (Secure)

1. Connected to public test server `test.rebex.net` using FileZilla with **explicit FTPS**:

2. Configured FileZilla:
    - Protocol: FTP
    - Encryption: Require explicit FTP over TLS
    - Same credentials as above

3. Captured FTPS traffic in Wireshark with filter: `ftp || tls`

---

### 🖼️ Screenshot Description

  - **FTP Screenshot**:
    - Shows login sequence:
     - `USER demo`
     - `PASS password` (in plaintext)
    - Shows file operations:
     - `RETR readme.txt`
     - `226 Transfer complete`
     
  - **FTPS Screenshot**:
    - TLS handshake is visible (`Client Hello`, `Server Hello`)
    - All further communication is encrypted as `Application Data`
    - No credentials or file names are visible

---

### 📌 Key Observations

#### FTP:
- **Credentials in Cleartext**: The `USER demo` and `PASS password` commands are visible in plain text during login.
- **File Transfer is Unencrypted**: The contents of `readme.txt` appear directly in the data stream.
- **Data Can Be Reconstructed**: Data transfer occurs on separate connection (still unencrypted), using *Follow TCP Stream*, the full data can be extracted from the PCAP.
- **No Encryption or TLS Handshake**: FTP uses no encryption mechanisms by default.
- Commands (`RETR`, `LIST`, etc.) readable in clear text

#### FTPS:
- **TLS Handshake Detected**: Traffic starts with a `Client Hello`, followed by `Server Hello` and key exchange.
- **Encrypted Application Data**: All further FTP communication is encrypted under TLS.
- **No Readable Credentials or File Content**: Neither login information nor file content can be viewed in the PCAP.
- Only initial server banner is visible before encryption begins (Explicit FTPS).

---

### 🛡️ Security Implications

#### FTP (Unencrypted):
- **Credential Theft Risk**: Any attacker capturing this traffic can easily obtain credentials (usernames and passwords).
- **File Exposure | Susceptible to MITM attacks**: Transferred files (even sensitive ones) can be intercepted and extracted.
- **Highly Vulnerable**: Often **targeted by attackers** on open or misconfigured networks. Especially dangerous on public Wi-Fi or internal LANs without segmentation.

#### FTPS (Encrypted):
- **Confidentiality Preserved**: Sensitive data is protected from sniffing or man-in-the-middle attacks.
- **TLS Encryption in Action**: FTPS is suitable for secure file transfers over untrusted networks.
- **Requires Correct Configuration**: Improper FTPS setups (e.g., fallback to plain FTP) could reintroduce vulnerabilities.
- **Still relies on trusted certificates**: expired/self-signed certs may trigger warnings.

---

### ✅ Summary

This analysis shows the critical importance of encryption in file transfer protocols. 
- FTP reveals everything in plaintext, including sensitive credentials and file content.
- FTPS offers strong protection via TLS, securing both authentication and data in transit.

For all production environments, **FTPS or SFTP** should be used in place of plain FTP.

---
