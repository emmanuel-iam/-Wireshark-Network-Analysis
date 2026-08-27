## Wireshark Network Analysis Lab

Hands-on network analysis project demonstrating packet capture, DNS analysis, TCP connection analysis, display filtering, HTTP inspection, TCP stream reconstruction, packet capture management, and troubleshooting with Wireshark.

Project Overview

This lab uses Wireshark to capture and analyze live network traffic from a local machine or lab environment. The goal is to build practical packet-analysis skills that apply to network engineering, SOC operations, cloud security, and incident response.

The lab focuses on understanding how traffic moves across a network and how packet-level evidence can be used to investigate connectivity problems, identify protocols, validate DNS resolution, inspect TCP sessions, and reconstruct application conversations.

Business Problem

Modern applications depend on networks for authentication, APIs, database traffic, web access, email, and cloud connectivity. When a user reports that a service is slow, unreachable, or behaving unexpectedly, packet analysis helps determine what is actually happening on the wire.

Wireshark provides visibility into that traffic by capturing packets from a network interface and allowing them to be filtered and inspected by protocol, address, port, flags, and application behavior.

Architecture

flowchart LR
    USER["User / Analyst Workstation"]
    NIC["Network Interface<br/>Ethernet / Wi-Fi"]
    WIRESHARK["Wireshark<br/>Packet Capture & Analysis"]
    LAN["Local Network"]
    DNS["DNS Server"]
    WEB["Web / Application Server"]
    INTERNET["Internet"]

    USER --> NIC
    NIC --> WIRESHARK
    NIC --> LAN
    LAN --> DNS
    LAN --> INTERNET
    INTERNET --> WEB

    DNS -->|"DNS Query / Response"| NIC
    WEB -->|"TCP / HTTP / HTTPS Traffic"| NIC

Skills Demonstrated

Skill

Practical Application

Packet Capture

Capture live traffic from an active interface

Display Filtering

Isolate relevant traffic in large captures

DNS Analysis

Identify queries, responses, record types, and returned IPs

TCP Analysis

Interpret SYN, SYN-ACK, ACK, RST, and connection behavior

HTTP Inspection

Analyze unencrypted web requests in an authorized lab

Stream Reconstruction

Reassemble a complete TCP conversation

Protocol Analysis

Examine DNS, HTTP, TCP, ICMP, and HTTPS-related traffic

Evidence Preservation

Save and export .pcapng captures

Troubleshooting

Use packet evidence to investigate network behavior

Technologies Used

Wireshark

Npcap

TCP/IP

DNS

HTTP / HTTPS

ICMP

TCP

nslookup

tshark

.pcapng packet captures

Windows / macOS / Linux terminal tools

Lab Environment

Component

Configuration

Primary Tool

Wireshark

Platform

Local workstation or lab VM

Capture Source

Active Ethernet or Wi-Fi interface

Packet Format

.pcapng

DNS Test Tool

nslookup

CLI Capture Tool

tshark

Cost

Free

Certification Alignment

CompTIA Network+ / Security+ / CySA+

Implementation

Step 1 — Install Wireshark

Wireshark was installed on the analysis workstation and the required packet-capture driver was configured.

Platform Notes

OS

Installation

Windows

Install Wireshark x64 and accept Npcap when prompted

macOS

Install the appropriate macOS package and allow required capture permissions

Linux

Install from the package manager and configure capture permissions

Linux example:

sudo apt install wireshark
sudo usermod -aG wireshark $USER
wireshark --version

Evidence

![Wireshark Installed](screenshots/01-wireshark-installed.png)

Step 2 — Capture Live Network Traffic

The active network interface was selected in Wireshark and a short live capture was performed while generating normal web traffic.

Process

Open Wireshark.

Select the active Ethernet or Wi-Fi interface.

Start the capture.

Generate network traffic from the workstation.

Stop the capture.

Review the resulting packets.

Evidence

![Live Packet Capture](screenshots/02-live-capture.png)

Step 3 — Apply Display Filters

Display filters were used to reduce large packet captures to the traffic relevant to a specific investigation.

Filter

Purpose

dns

Show DNS queries and responses

http

Show unencrypted HTTP traffic

tcp

Show all TCP traffic

tcp.flags.syn == 1

Identify TCP connection attempts

tcp.flags.reset == 1

Identify TCP resets

icmp

Show ICMP / ping traffic

ip.addr == 192.168.1.1

Isolate traffic to or from one host

ip.src == 10.0.0.5

Show traffic from one source

tcp.port == 443

Show traffic using TCP port 443

http.request

Show HTTP request packets

Key concept: Capture filters control what is recorded. Display filters control what is shown after the capture. This lab primarily uses display filters so the original capture remains intact.

Evidence

![Display Filters](screenshots/03-display-filters.png)

Guided Analysis Exercises

Exercise A — DNS Lookup Analysis

A DNS query was intentionally generated with:

nslookup google.com

Wireshark was then filtered with:

dns

The analysis identified:

The DNS query for google.com

The corresponding DNS response

The returned IPv4 address

The DNS A record in the packet detail pane

What This Demonstrates

This exercise shows how a hostname is resolved before a client can connect to a remote server. It also demonstrates how unusual or unexpected DNS lookups can become useful indicators during troubleshooting or security investigations.

Evidence

![DNS Query](screenshots/04-dns-query.png)
![DNS Response](screenshots/05-dns-response.png)

Exercise B — TCP Three-Way Handshake

A TCP connection was generated and filtered so the initial connection setup could be observed.

The normal handshake is:

Client                      Server
  |                           |
  | -------- SYN ---------->  |
  | <------ SYN-ACK --------  |
  | -------- ACK ---------->  |
  |                           |
  |     Connection Open       |

Packet

Flags

Meaning

1

SYN

Client requests a connection

2

SYN, ACK

Server acknowledges and accepts

3

ACK

Client confirms the connection

Troubleshooting Value

A SYN with no SYN-ACK can indicate an unreachable host, filtering, or service availability issue. A TCP reset can indicate a connection that was refused or forcibly closed.

Evidence

![TCP Handshake](screenshots/06-tcp-handshake.png)

Exercise C — HTTP Cleartext Demonstration

In an authorized lab environment only, unencrypted HTTP traffic was inspected to demonstrate why TLS/HTTPS is required for sensitive information.

A test HTTP POST request was generated and filtered with:

http.request.method == POST

The packet details demonstrated that form data sent over HTTP can appear in readable plaintext.

Security note: This exercise should only be performed against systems and networks you own or have explicit permission to analyze. Do not capture or inspect credentials belonging to other users.

What This Demonstrates

This exercise provides a practical demonstration of the security difference between HTTP and HTTPS. TLS encryption protects application payloads from being readable in ordinary packet captures.

Evidence

![HTTP POST Analysis](screenshots/07-http-post.png)

Exercise D — Follow a TCP Stream

A captured HTTP packet was selected and reconstructed using:

Right-click Packet → Follow → TCP Stream

This reassembled the individual TCP packets into a single readable application conversation.

What This Demonstrates

Following a TCP stream is useful for:

Reconstructing application conversations

Understanding request/response behavior

Investigating incidents

Troubleshooting web applications

Determining what data was transferred during a session

Evidence

![Follow TCP Stream](screenshots/08-tcp-stream.png)

Saving and Exporting Packet Captures

Captures were saved in .pcapng format for later analysis and portfolio evidence.

Save a Capture

File → Save As → capture-name.pcapng

Export Filtered Packets

Apply Display Filter
File → Export Specified Packets → Displayed

Reopen a Capture

File → Open → select .pcapng file

Command-Line Capture with tshark

tshark -i eth0 -w capture.pcapng -c 1000

Evidence

![Saved Capture](screenshots/09-saved-capture.png)

Validation

Skill

Validation Method

Expected Result

DNS Capture

Apply dns filter

Matching DNS query and response are visible

TCP Handshake

Inspect TCP session

SYN → SYN-ACK → ACK

Display Filters

Filter by IP, port, and protocol

Only relevant packets remain visible

Stream Reconstruction

Follow TCP Stream

Complete application conversation is reconstructed

Capture Management

Save and reopen .pcapng

Original packets remain available

Recommended Capture Files

captures/
├── dns-lookup.pcapng
├── tcp-handshake.pcapng
└── tcp-stream.pcapng

These files can serve as technical evidence of the analysis performed during the lab.

Security Considerations

Packet captures can contain sensitive information, including:

Internal and external IP addresses

Hostnames

DNS queries

URLs

Cookies

Authentication headers

Session identifiers

Application data

Cleartext credentials when insecure protocols are used

Before publishing packet captures or screenshots to GitHub, review and sanitize them carefully. Do not upload passwords, tokens, cookies, confidential traffic, personal information, or packet captures from networks you do not own or have permission to analyze.

Project Results

Traffic Generated
      ↓
Network Interface
      ↓
Wireshark Capture
      ↓
Protocol / IP / Port Filtering
      ↓
DNS Analysis
      ↓
TCP Handshake Analysis
      ↓
HTTP Inspection
      ↓
TCP Stream Reconstruction
      ↓
Save / Export PCAP Evidence

Outcome: Captured live network traffic with Wireshark, isolated traffic using display filters, analyzed DNS queries and responses, identified TCP connection establishment, inspected authorized HTTP traffic, reconstructed a TCP stream, and saved packet captures for later analysis.

Recommended Repository Structure

wireshark-network-analysis-lab/
├── README.md
├── screenshots/
│   ├── 01-wireshark-installed.png
│   ├── 02-live-capture.png
│   ├── 03-display-filters.png
│   ├── 04-dns-query.png
│   ├── 05-dns-response.png
│   ├── 06-tcp-handshake.png
│   ├── 07-http-post.png
│   ├── 08-tcp-stream.png
│   └── 09-saved-capture.png
├── captures/
│   ├── dns-lookup.pcapng
│   ├── tcp-handshake.pcapng
│   └── tcp-stream.pcapng
└── docs/
    ├── dns-analysis.md
    ├── tcp-analysis.md
    └── troubleshooting.md

Adding Screenshots

Upload screenshots to the screenshots/ folder and reference them like this:

![TCP Three-Way Handshake](screenshots/06-tcp-handshake.png)

Key Takeaway

This lab demonstrates practical network-analysis skills using packet-level evidence. It shows how Wireshark can be used to validate DNS resolution, understand TCP connection behavior, isolate relevant traffic, reconstruct application sessions, and support network troubleshooting and security investigations.
