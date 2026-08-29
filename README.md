# Wireshark Network Analysis Lab

> Hands-on network analysis lab demonstrating packet capture, DNS analysis, TCP three-way handshake analysis, HTTP traffic inspection, TCP stream reconstruction, and packet capture preservation using Wireshark.

---

## Project Overview

This lab demonstrates practical network traffic analysis using Wireshark. I captured live network traffic, applied protocol-specific display filters, analyzed DNS queries and responses, examined TCP connection establishment, inspected HTTP traffic, reconstructed TCP streams, and saved packet captures for future analysis.

The lab demonstrates skills applicable to network engineering, SOC analysis, cloud security, cybersecurity operations, and incident response.

---

## Architecture

```mermaid
flowchart LR
    A[Windows Workstation] --> B[Network Interface Card]
    B --> C[Wireshark]
    B --> D[Local Network]
    D --> E[DNS Server]
    D --> F[Internet]
    F --> G[Remote Web Server]

    C --> H[Packet Capture]
    H --> I[Display Filters]
    I --> J[DNS Analysis]
    I --> K[TCP Analysis]
    I --> L[HTTP Analysis]
    I --> M[TCP Stream Reconstruction]
    H --> N[PCAPNG Export]
```

---

## Skills Demonstrated

| Skill | Implementation |
|---|---|
| Packet Capture | Captured live network traffic with Wireshark |
| Network Interface Analysis | Identified and monitored an active Ethernet interface |
| DNS Analysis | Examined DNS queries, responses, and A records |
| TCP Analysis | Identified the TCP SYN, SYN-ACK, and ACK handshake |
| Display Filtering | Isolated DNS, TCP, and HTTP traffic |
| HTTP Analysis | Inspected HTTP GET and POST requests |
| Security Analysis | Demonstrated exposure of application data over unencrypted HTTP |
| Stream Reconstruction | Reconstructed application communication using Follow TCP Stream |
| Network Troubleshooting | Used packet-level data to analyze client/server communication |
| Evidence Preservation | Saved network captures in PCAPNG format |

---

## Technologies Used

- Wireshark
- Windows
- Npcap
- TCP/IP
- DNS
- HTTP
- TCP
- TLS
- Command Prompt
- `nslookup`
- `.pcapng` packet captures

---

# Implementation

## Step 1 — Install Wireshark

Download the Windows x64 version of Wireshark from the official Wireshark website.

<img width="1078" height="893" alt="ws1" src="https://github.com/user-attachments/assets/d9e2af2a-6dcf-4937-ab27-b7f4bcb432a0" />

---

## Step 2 — Capture Network Traffic

Identified the active Ethernet network interface.

<img width="1978" height="1030" alt="ws2" src="https://github.com/user-attachments/assets/af7c2bb4-03a3-42c6-b6b9-03c012792840" />

Start a live packet capture on the Ethernet interface.

<img width="1983" height="1031" alt="ws5" src="https://github.com/user-attachments/assets/0781bd19-f557-4681-a839-03f85b7182eb" />


The capture began displaying TCP, HTTP, TLS, and other network traffic between the system and remote hosts.

---

## Step 3 — Generate and Analyze DNS Traffic

While Wireshark continued capturing network traffic, opened Command Prompt to generate DNS traffic for analysis.

<img width="1550" height="1269" alt="ws4" src="https://github.com/user-attachments/assets/3dbfec6d-9ba1-4aed-9af6-d40c9a40a727" />

Performed a DNS lookup for Google using:

    nslookup google.com

The DNS lookup successfully returned IPv4 and IPv6 addresses associated with `google.com`.

<img width="1721" height="915" alt="ws6" src="https://github.com/user-attachments/assets/f9a052b5-5fc6-49d6-ad61-9bbde0581471" />

Next, I applied the following Wireshark display filter:

    dns

This isolated DNS packets from the larger packet capture.

located the DNS query and corresponding DNS response for `google.com`.

<img width="1981" height="1036" alt="ws7" src="https://github.com/user-attachments/assets/cee6e046-189e-4ae8-b52a-29ac75d5dbe8" />

I expanded the DNS response and reviewed the returned DNS records.

I compared the IPv4 addresses displayed by Wireshark with the addresses returned by `nslookup`.

The results matched, confirming that Wireshark successfully captured the DNS resolution process.

<img width="2052" height="1208" alt="ws8" src="https://github.com/user-attachments/assets/01a23d69-8304-4f9f-9d1d-a8a764227775" />

After completing the DNS analysis, prepared Wireshark for the next capture.

<img width="1985" height="1030" alt="ws9" src="https://github.com/user-attachments/assets/a83e7646-d1ee-4908-9b4a-e01f96081a78" />

---

## Step 4 — Analyze a TCP Three-Way Handshake

For the next portion of the lab, I used an HTTP test website to generate TCP and HTTP traffic.

<img width="1617" height="1177" alt="ws10" src="https://github.com/user-attachments/assets/efdf47a9-6da7-4bc3-942c-72af05c136b5" />

I determined the destination IP address by running:

    nslookup zero.webappsecurity.com

The hostname resolved to:

    54.82.22.214

<img width="2070" height="824" alt="ws11" src="https://github.com/user-attachments/assets/1625ea43-eb79-463c-aa3e-28a5e5f6377c" />


Then filtered Wireshark traffic for the destination IP using:

    tcp and ip.addr == 54.82.22.214

The captured packets clearly showed the TCP three-way handshake:

    SYN → SYN-ACK → ACK

<img width="2001" height="976" alt="ws12" src="https://github.com/user-attachments/assets/52f46404-d681-4a1e-8b9d-19c40bd5be58" />

The packet sequence demonstrated that the client successfully established a TCP connection with the remote web server over TCP port 80.

After verifying the TCP handshake, prepared Wireshark for the next portion of the lab.

<img width="1982" height="1041" alt="ws13" src="https://github.com/user-attachments/assets/e8972e36-7ce9-4615-8f40-269550826b5d" />

---

## Step 5 — Inspect Cleartext HTTP Traffic

> **Security Note:** This exercise was performed in an authorized test environment designed for security training. No production credentials were used.

I returned to the HTTP test application's login page to generate a form submission that could be inspected with Wireshark.

I entered test credentials into the intentionally insecure HTTP application.

<img width="1452" height="895" alt="ws14" src="https://github.com/user-attachments/assets/f36a8337-aae1-4a66-a11e-f4288742fab4" />

I then applied the following Wireshark display filter:

    http.request.method == POST

This isolated HTTP POST requests.

After selecting the login POST request, I expanded the `HTML Form URL Encoded` section.

Wireshark exposed the submitted form parameters in plaintext.

<img width="1974" height="912" alt="ws15" src="https://github.com/user-attachments/assets/d6a41369-c33d-4fdb-8b28-faaf54313da0" />

This demonstrated a significant security weakness associated with transmitting authentication information over unencrypted HTTP.

Because HTTP does not provide TLS encryption, application data transmitted between the client and server can potentially be inspected through captured network traffic.

HTTPS addresses this risk by protecting application traffic with TLS encryption.

After completing the HTTP analysis, Prepared Wireshark for the next exercise.

<img width="1978" height="892" alt="ws16" src="https://github.com/user-attachments/assets/5a082469-30f4-4ea8-a125-8e03ac3a6144" />

---

## Step 6 — Follow a TCP Stream

I applied the following display filter:

    http

I selected an HTTP packet, right-clicked the packet, and navigated to:

    Follow → TCP Stream

<img width="1984" height="1030" alt="ws17" src="https://github.com/user-attachments/assets/a099d4a7-80da-4718-abb2-f7a8f6cdf1f4" />

Wireshark reconstructed the individual TCP packets into a complete application conversation.

The reconstructed stream allowed me to inspect the HTTP request sent by the client and the HTTP response returned by the server in a single view.

<img width="1312" height="1038" alt="ws18" src="https://github.com/user-attachments/assets/b332a772-a9b3-475b-881c-b4f0b99e2c25" />

This demonstrated how network analysts can reconstruct application-layer communication from individual network packets.

---

## Step 7 — Save the Packet Capture

After completing the packet analysis, Saved the capture for future review and documentation.

I selected:

    File → Save As

<img width="1228" height="816" alt="ws19" src="https://github.com/user-attachments/assets/bb7ff8eb-ae12-42ef-b87d-ee4d686cc4aa" />

I then selected the `.pcapng` format and saved the packet capture.

<img width="1361" height="930" alt="ws20" src="https://github.com/user-attachments/assets/e6da7f16-3b93-402a-85ed-34fdbbec28e8" />

Saving the capture preserves the network traffic so that it can be reopened and analyzed later without requiring another live capture.

---

# Lab Validation

The completed lab successfully demonstrated:

- Live packet capture using Wireshark
- Network interface selection
- DNS query and response analysis
- DNS A record verification
- `nslookup` DNS troubleshooting
- Wireshark display filtering
- TCP three-way handshake analysis
- HTTP traffic inspection
- HTTP POST request analysis
- Cleartext application data exposure over HTTP
- TCP stream reconstruction
- PCAPNG packet capture preservation

---

# Key Findings

### DNS Resolution

DNS traffic captured in Wireshark showed the workstation sending DNS queries and receiving responses containing IP addresses associated with the requested hostname.

The results captured in Wireshark matched the addresses returned by `nslookup`.

### TCP Connection Establishment

The packet capture demonstrated the standard TCP three-way handshake:

    Client → Server: SYN
    Server → Client: SYN-ACK
    Client → Server: ACK

This confirmed successful TCP connection establishment between the client and remote server.

### HTTP Security

The HTTP analysis demonstrated that unencrypted HTTP traffic can expose application-layer information inside packet captures.

This reinforces the importance of using HTTPS/TLS to protect sensitive data transmitted between clients and servers.

### TCP Stream Reconstruction

Wireshark's **Follow TCP Stream** feature reconstructed packets belonging to the same TCP session and displayed the application conversation in a readable format.

This capability is valuable during network troubleshooting, security investigations, and incident response.

---

# Security Considerations

Packet captures can contain sensitive information, including:

- IP addresses
- Hostnames
- DNS queries
- URLs
- Cookies
- Authentication headers
- Session identifiers
- Application data
- Cleartext credentials

Packet captures should therefore be handled as potentially sensitive security evidence.

Only network traffic from systems and networks that you own or are explicitly authorized to monitor should be captured and analyzed.

Sensitive information should be removed or sanitized before screenshots or packet captures are published to a public GitHub repository.

---

# Repository Structure

    Wireshark-Network-Analysis/
    │
    ├── README.md
    │
    ├── screenshots/
    │   ├── ws1.png
    │   ├── ws2.png
    │   ├── ws3.png
    │   ├── ws4.png
    │   ├── ws5.png
    │   ├── ws6.png
    │   ├── ws7.png
    │   ├── ws8.png
    │   ├── ws9.png
    │   ├── ws10.png
    │   ├── ws11.png
    │   ├── ws12.png
    │   ├── ws13.png
    │   ├── ws14.png
    │   ├── ws15.png
    │   ├── ws16.png
    │   ├── ws17.png
    │   ├── ws18.png
    │   ├── ws19.png
    │   ├── ws20.png
    │   ├── ws21.png
    │   ├── ws22.png
    │   ├── ws23.png
    │   └── ws24.png
    │
    └── captures/
        └── network-analysis.pcapng

---

# Project Results

By completing this lab, I gained hands-on experience capturing and analyzing live network traffic using Wireshark.

I demonstrated the ability to:

- Identify network interfaces
- Capture live network packets
- Filter large packet captures
- Analyze DNS resolution
- Identify TCP connection establishment
- Inspect HTTP requests
- Identify security risks associated with unencrypted traffic
- Reconstruct TCP conversations
- Preserve packet captures for later investigation

These skills provide a practical foundation for network troubleshooting, security monitoring, SOC investigations, cloud security analysis, and incident response.

---

# Key Takeaway

Wireshark provides packet-level visibility into how systems communicate across a network. By analyzing DNS, TCP, HTTP, and application traffic, I was able to move beyond simply observing whether a connection worked and examine exactly how the communication occurred between the client and remote systems.
