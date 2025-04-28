# Advanced Network Traffic Analysis
In my first project [Wireshark Traffic Analysis](https://github.com/chukwupg/wireshark-traffic-analysis), I captured and analyzed basic HTTP/HTTPS traffic and practiced identifying valid handshakes, DNS requests, and TLS communication. In this project, I intend to **build on those foundations** by targeting the specific improvement areas I identified.

## 🎯 Objectives

- Capture and analyze diverse protocols: **DNS, FTP, ICMP, HTTP, HTTPS**
- Explore and analyze **suspicious traffic samples** from public PCAP sources
- Compare **secure vs insecure traffic** (HTTP vs HTTPS)
- Identify signs of **DNS tunneling** and other real-world attack behavior
- Explore tools used to **anonymize sensitive traffic data** for safe sharing
- Practice using **custom Wireshark filters** to speed up investigations


## 🧰 Tools $ Commands

- **Wireshark** - Deep packet inspection and filtering
- **tcpdump** - Lightweight command-line traffic capture
- **nslookup, ping, traceroute, dig** - Generating DNS & ICMP traffic
- **FileZilla** - FTP server/client setup
- **Browser** For HTTP/HTTPS capture
- **TraceWrangler** (for traffic anonymization)

## 🔄 Protocols to Explore

| Protocol | Use Case | Status |
|----------|----------|--------|
| DNS      | Name resolution, tunneling research | ✔️ Analyzed |
| ICMP     | Ping, traceroute, network diagnostics | ✔️ Analyzed |
| FTP      | Insecure file transfer | 🅿️ Planned |
| HTTP     | Unencrypted web traffic | 🅿️ Planned |
| HTTPS    | Encrypted web traffic | 🅿️ Planned |

## 🌐 Real-World Threat Simulation

Once baseline (clean) traffic is captured and analyzed, I plan to:
- Analyze public PCAPs with suspicious traffic
- Inspect basic DNS tunneling and port scans
- Compare against normal traffic behaviors

## 📂 Project Structure

| Folder | Description |
|--------|-------------|
| `pcaps/clean/` | PCAP files for normal traffic |
| `pcaps/suspicious/` | PCAP files with malicious or unusual behavior |
| `analysis/` | Written insights and comparisons |
| `screenshots/` | Key findings visualized from Wireshark |
| `tools-used/` | Commands and configurations used in the project |

## 📈 Why This Matters

- Deepens hands-on protocol knowledge
- Strengthens cybersecurity incident analysis skills
- Connects normal vs suspicious traffic patterns
- Builds a solid foundation for forensic & threat detection

## 📬 Feedback & Collaboration

Feel free to open Issues, suggest improvements, or fork the repo. I’m open to feedback and suggestions as I learn!
