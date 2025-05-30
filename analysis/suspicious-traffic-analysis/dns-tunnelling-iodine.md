# 🕵️ DNS Tunneling Traffic Analysis

This analysis investigates DNS tunneling as a suspicious use of DNS protocol to covertly transfer data. The PCAP file demonstrates how a tunneling tool (Iodine) encapsulates IP data over DNS queries, enabling potential command-and-control (C2) or data exfiltration over DNS.

---

## 📦 Traffic Capture Details

| Tool Used | Tunneling Method | PCAP File              |
|-----------|------------------|------------------------|
| Iodine    | DNS              | dns-tunnelling-iodine.pcap |

- **Source**: Elastic Security Analytics Examples  
- **Description**: Shows use of the Iodine tool to tunnel traffic over DNS (https://github.com/elastic/examples)

---

## 🔍 Key Observations

### 1. 🔗 Encoded DNS Queries

- Many DNS queries include long subdomains that appear base32/base64 encoded.
- For example: zi04aA-La-fl�te-na�ve-fran�aise-est-retir�-�-Cr�te.pirate.sea
- These subdomains act as data carriers in the tunnel.

### 2. ⏱️ High-Frequency Requests

- DNS queries are sent in rapid succession, far more frequently than regular DNS usage.
- These bursts correlate to outbound "packets" in the tunneled communication.

### 3. 📥 Query Types and Responses

- Queries primarily use:
- **TXT**
- **NULL**
- Responses from the DNS server also include encoded or padded data, completing the communication loop.

### 4. 📶 Bidirectional Traffic

- Two-way communication is observed:
- Encoded queries from client
- Encoded DNS answers as data responses

---

## 🔍 Comparison: Clean DNS vs DNS Tunneling

| Feature                | Clean DNS                          | DNS Tunneling                             |
|------------------------|-------------------------------------|-------------------------------------------|
| Query Length           | Short, human-readable               | Long, encoded subdomains                  |
| Query Frequency        | Infrequent, as needed               | Repeated, high frequency                  |
| Query Types            | A, AAAA, MX, etc.                   | Mostly TXT, NULL                          |
| Traffic Directionality | Client-initiated, unidirectional    | Bidirectional via queries and responses   |
| Payload Content        | IP address lookup                   | Encoded binary data                       |

---

## 🛡️ Security Implications

### 🧨 Firewall and Proxy Evasion

- DNS is typically allowed through firewalls, making it an attractive channel for bypassing perimeter security.

### 📤 Covert Exfiltration

- Sensitive data (files, credentials) can be hidden in subdomain queries and smuggled out over DNS.

### 👀 Difficult Detection

- DNS tunneling does not always trigger alerts unless:
- Deep packet inspection is applied.
- DNS traffic is analyzed for behavioral anomalies.

---

## 📸 Screenshots

| Description                 | Screenshot File             |
|-----------------------------|-----------------------------|
| Encoded DNS Query Example   | dns_tunnel_query.png        |
| High-Frequency Query Graph  | dns_tunnel_frequency.png    |
| DNS TXT Response Contents   | dns_tunnel_response.png     |

---

## ✅ Summary

The DNS tunnel captured in this analysis illustrates a covert channel for transmitting data using Iodine. The encoded query patterns, unusual frequency, and payload-rich responses sharply contrast with clean DNS behavior. This highlights why DNS should not be overlooked in network security monitoring and detection workflows.
