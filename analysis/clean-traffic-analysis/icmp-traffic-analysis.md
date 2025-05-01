# 📡 ICMP Traffic Analysis

**Tool Used**: Wireshark  
**CLI tools**: `ping` and `traceroute`
**File**: `pcaps/clean/anon-icmp-traffic.pcapng`  
**Display Filter Applied**: `icmp`

---

## 🔍 Overview

Internet Control Message Protocol (ICMP) is used for diagnostic and network management purposes.  
Common ICMP tools include **ping** (checking if a host is reachable) and **traceroute** (mapping the path packets take to reach it's destination).

During capture:
- A simple `ping` command to an external IP (`8.8.8.8`) was issued.
- ICMP Echo Request and Echo Reply messages were observed.
- The communication occurred directly over **IP**, without TCP or UDP.

---

## 🧪 Key Observations

| Feature | Details |
|:--------|:--------|
| Protocol | ICMP |
| Port Used | 🚫 No ports (ICMP runs over IP directly) |
| Encryption | ❌ None |
| Data Exposure | ✔️ Payloads and IP addresses are visible |
| Message Types | Echo Request (Type 8), Echo Reply (Type 0) |

- **Echo Requests** were sent from the source IP (192.134.3.100) to `8.8.8.8`.
- **Echo Replies** were received, confirming that `8.8.8.8` is reachable.
- Each ICMP packet included an identifier and sequence number.

---

## 🛡️ Security Implications

- **Vulnerabilities**:
  - **Visibility**: ICMP messages are easily visible and readable on the network.
  - **Denial of Service (DoS) via ICMP Flooding**:
    - Attackers send massive numbers of Echo Requests to overwhelm a server or network device (also called "Ping Flood").
  - **Smurf Attack (ICMP Amplification and Reflection)**:
    - Attackers send Echo Requests to a network’s broadcast address, spoofing the victim’s IP address.
    - All devices on the network respond to the victim, massively amplifying traffic and overwhelming their network.
  - **Network Scanning and Reconnaissance**:
    - ICMP can be abused to map network structures and identify live hosts.
- **Mitigation**:
  - Rate-limit ICMP traffic.
  - Configure strict ICMP access control list filtering at the network edge;
    - Only recommended ICMP traffic for proper network operation should be allowed into the internal network (echo reply, source quench and unreachable).
    - ICMP messages required for proper network operation should be allowed to exit the network (echo, parameter problem, packet too big, source quench).
    - Both ICMP echo and redirect messages should be blocked inbound.
    - Block all other ICMP message types outbound.
  - Disable responses to broadcast pings at the router level.
  - Monitor for unusual ICMP traffic patterns.

---

## 🖼️ Visual Evidence

| Type | Screenshot |
|:-----|:-----------|
| ICMP Echo Request | ![anon-icmp-echo-request](https://github.com/user-attachments/assets/b1ddc0de-170f-4133-bf54-d694450aba5f)
| ICMP Echo reply | ![anon-icmp-echo-reply](https://github.com/user-attachments/assets/d6e85209-bf0c-43cd-903c-36996c47ebfc)


- *The screenshots shows a successful ICMP Echo Request and Echo Reply exchange between the local machine and the public DNS server at `8.8.8.8`.*

---

## 📚 Technical Deep Dive

- **ICMP Header Analysis**:
  - **Type**:
    - `8` → Echo Request
    - `0` → Echo Reply
  - **Code**:
    - Typically `0` for Echo-related messages.
  - **Checksum**: Used to verify message integrity.
  - **Identifier & Sequence Number**: Helps match requests to replies.

- **Packet Structure**:
  - ICMP header is embedded inside the IP packet.
  - Contains no TCP/UDP layer.

---

# ✅ Summary

The captured ICMP traffic demonstrates a classic echo request/reply pattern over IP.  
While extremely useful for network diagnostics, ICMP can also be leveraged by attackers for scanning and flooding purposes if left unmonitored.

---

## 📂 Folder Organization

| Folder | Content |
|:-------|:--------|
| `pcaps/clean/` | `anon-icmp-traffic.pcapng` file |
| `screenshots/clean-traffic` | `anon-icmp-echo-request`, `anon-icmp-echo-reply` screenshots showing ping echo request and reply |

---
