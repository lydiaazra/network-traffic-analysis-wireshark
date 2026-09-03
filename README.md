# Network Traffic Analysis with Wireshark
A hands-on network traffic analysis project capturing and inspecting live packets using Wireshark. This project covers protocol identification across DNS, TCP, HTTP, ICMP, TLS, ARP, and DHCP traffic.

## Objectives

- Capture live network traffic on a Wi-Fi interface using Wireshark
- Apply display filters to isolate specific protocols
- Understand how each protocol behaves at the packet level
- Analyse protocol hierarchy across a full capture session

## Tools and Technologies

| Tool | Purpose |
|------|---------|
| Wireshark | Packet capture and protocol analysis |
| curl | Generate controlled HTTP and HTTPS traffic |
| Windows Command Prompt | Run ping, curl, and ipconfig commands |
| ipconfig /flushdns | Clear DNS cache before captures |

## Protocols Analysed

### 1. DNS (Domain Name System)
**Filter:** `dns`

DNS translates domain names into IP addresses. Captured DNS query and response packets on UDP port 53. The query showed my computer asking for the IP of a domain, and the response returned the resolved A record.

### 2. TCP (Three-Way Handshake)
**Filter:** `tcp.flags.syn == 1`

Before any data is sent over TCP, both devices complete a three-way handshake. Captured the SYN, SYN-ACK, and ACK sequence that establishes every TCP connection.

### 3. HTTP (Hypertext Transfer Protocol)
**Filter:** `tcp.port == 80` then Follow TCP Stream

Used curl to generate plaintext HTTP traffic. The Follow TCP Stream view revealed the complete GET request and HTTP 200 response in fully readable text, including all headers and the HTML body.

### 4. ICMP (Internet Control Message Protocol)
**Filter:** `icmp`

ICMP is used to test network reachability. Ran `ping google.com` and captured alternating Echo Request (Type 8) and Echo Reply (Type 0) packets with sequence numbers and round-trip times visible.

### 5. TLS (Transport Layer Security)
**Filter:** `tls`

TLS encrypts HTTPS traffic. Captured the TLS handshake packets including Client Hello, Server Hello, Certificate, and Change Cipher Spec. After the handshake, all Application Data packets were fully encrypted and unreadable.

### 6. ARP (Address Resolution Protocol)
**Filter:** `arp`

ARP resolves IP addresses to MAC addresses on the local network. Captured the router broadcasting "Who has [IP]? Tell [gateway IP]" requests that happen automatically in the background of any active network.

### 7. DHCP (Dynamic Host Configuration Protocol)
**Filter:** `dhcp`

DHCP assigns IP addresses automatically when a device joins a network. Captured the DHCP Request (my computer asking for an IP) and DHCP ACK (router confirming the assignment) after reconnecting to Wi-Fi.

### 8. Protocol Hierarchy Statistics
**Path:** Statistics then Protocol Hierarchy

Analysed traffic composition across a full capture. The hierarchy showed the layered protocol structure from Ethernet at the base up through IPv4, TCP and UDP, and application protocols including TLS, HTTP, DNS, and ICMP.

## Screenshots

| Screenshot | Description |
|------------|-------------|
| `screenshots/dns.png` | DNS query and response packets |
| `screenshots/tcp.png` | TCP three-way handshake |
| `screenshots/tcp.port-80.png` | HTTP traffic on port 80 |
| `screenshots/http.png` | Follow TCP Stream showing plaintext HTTP |
| `screenshots/icmp.png` | ICMP Echo Request and Echo Reply |
| `screenshots/tls.png` | TLS handshake packets |
| `screenshots/arp.png` | ARP broadcast request on local network |
| `screenshots/dhcp.png` | DHCP Request and ACK on Wi-Fi reconnect |
| `screenshots/protocol-hierarchy.png` | Protocol Hierarchy statistics breakdown |

## Key Learnings

- Always capture on the correct network interface; use `ipconfig` to identify the active adapter
- Flush DNS cache with `ipconfig /flushdns` before capturing DNS traffic
- Modern browsers auto-upgrade HTTP to HTTPS; use `curl http://` to generate plaintext traffic
- Follow TCP Stream reveals the complete readable conversation even when packets appear as TCP
- ARP and DHCP happen automatically in the background with no special commands needed

## Disclaimer
All packet captures were performed on a personally owned test network for educational and portfolio purposes. No third-party systems or networks were accessed without authorisation.
