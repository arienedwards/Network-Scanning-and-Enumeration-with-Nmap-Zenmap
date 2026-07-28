# Network-Scanning-and-Enumeration-with-Nmap-Zenmap
This lab focused on using **Nmap** and **Zenmap** to discover hosts, identify open ports, detect operating systems, enumerate services, and analyze network topology across multiple network segments.


rcise was performed from a **Kali Linux attacker workstation** against systems located on both an **Internal Network (192.168.1.0/24)** and a **DMZ Network (10.1.1.0/28)**.

---

## Lab Objectives

- Discover live hosts on a network
- Perform host discovery using ICMP ping scans
- Conduct TCP port scanning
- Detect operating systems remotely
- Enumerate running services and versions
- Analyze IP protocol support
- Utilize packet tracing for scan analysis
- Visualize network topology with Zenmap
- Compare multiple scan results

---

## Lab Environment

### Systems

| Device | IP Address | Purpose |
|----------|------------|-----------|
| Kali Linux Attacker | 203.0.113.2 | Scanning workstation |
| pfSense Firewall | 203.0.113.1 | External gateway |
| pfSense LAN | 192.168.1.1 | Internal gateway |
| pfSense DMZ | 10.1.1.1 | DMZ gateway |
| Security Onion | 192.168.1.6 | Security monitoring |
| Ubuntu Server | 192.168.1.50 | Internal Linux host |
| Dawn Vulnerable Linux (DVL) | 10.1.1.10 | Vulnerable target |

---

## Network Topology

images/network-topology.png

---

# Nmap Scanning Activities

## 1. Viewing Nmap Options

Displayed available Nmap commands and scan options.

```bash
nmap
```

---

## 2. Host Discovery Scan

Performed a ping sweep against the DMZ network.

```bash
nmap -sP 10.1.1.*
```

### Results

Identified active hosts:

- 10.1.1.1 (DMZ Gateway)
- 10.1.1.10 (DVL Server)

---

## 3. MAC Address Spoofing During Host Discovery

Executed a ping scan while spoofing the source MAC address.

```bash
nmap -v -sP --spoof-mac 0 10.1.1.*
```

### Purpose

- Obfuscates scanner identity
- Simulates traffic from a different device
- Useful during penetration testing and red team operations

---

## 4. IP Protocol Scan

Scanned the DVL server to determine supported IP protocols.

```bash
nmap -sO 10.1.1.10
```

### Purpose

Identifies supported protocols such as:

- TCP
- UDP
- ICMP
- IGMP

---

## 5. TCP Connect Scan

Performed a TCP scan against the Security Onion server.

```bash
nmap -sT 192.168.1.6
```

### Open Ports Discovered

| Port | Service |
|--------|----------|
| 22 | SSH |
| 25 | SMTP |
| 80 | HTTP |
| 514 | Shell |

---

## 6. Operating System Detection

### Scan DVL Server

```bash
nmap -O 10.1.1.10
```

### Results

Detected:

- Linux 2.6.x
- Likely Linux Kernel 2.6.15–2.6.26

### Open Services

| Port | Service |
|--------|---------|
| 22 | SSH |
| 139 | NetBIOS |
| 199 | SMUX |
| 445 | Microsoft-DS |
| 631 | IPP |
| 3306 | MySQL |

---

### Scan Ubuntu Server

```bash
nmap -O 192.168.1.50
```

Initial OS detection was inconclusive.

### Aggressive OS Guessing

```bash
nmap -O --osscan-guess 192.168.1.50
```

### Purpose

Forces Nmap to provide likely operating system matches when confidence levels are low.

---

## 7. Scan a Specific Port

Scanned only TCP Port 80 on the DVL system.

```bash
nmap -p 80 10.1.1.10
```

### Findings

Confirmed web service availability.

---

## 8. Multi-Network Port Scan

Scanned Port 80 across both network segments.

```bash
nmap -p 80 192.168.1.0/24 10.1.1.0/28
```

### Purpose

Efficiently identifies web servers throughout multiple subnets.

---

## 9. Packet Trace Analysis

Captured and displayed all packets exchanged during a scan.

```bash
nmap --packet-trace 10.1.1.10
```

### Benefits

- Understands packet flow
- Troubleshoots firewall filtering
- Visualizes scan behavior

---

## 10. Interface and Routing Information

Displayed local interface and route information.

```bash
nmap --iflist
```

### Information Returned

- Network interfaces
- IP addresses
- Routing table entries
- Network reachability

---

## 11. Service Version Detection

Identified services and application versions running on the DVL system.

```bash
nmap -sV 10.1.1.10
```

### Purpose

- Service enumeration
- Vulnerability research
- Patch verification

---

# Zenmap Analysis

## Launch Zenmap

```bash
zenmap
```

---

## Ping Scan Profile

Target:

```text
192.168.1.0/24
```

Profile:

```text
Ping Scan
```

### Findings

Discovered active hosts throughout the internal network.

---

## Full Network Scan

Targets:

```text
192.168.1.0/24 10.1.1.0/28
```

Modified command:

```bash
nmap -T5 192.168.1.0/24 10.1.1.0/28
```

### Purpose

- Faster scan execution
- Comprehensive host discovery
- Port enumeration

---

## Topology Mapping

Zenmap generated a visual network map illustrating:

- Network relationships
- Routing paths
- Host locations
- Firewall placement

---

## Host and Port Analysis

Using Zenmap's:

### Ports/Hosts Tab

Reviewed:

- Open ports
- Service names
- Host status

### Services Tab

Filtered systems by service type:

- HTTP
- SSH
- FTP
- SMTP
- Telnet

---

## Comparing Scan Results

Used:

```text
Tools → Compare Results
```

Compared:

### Scan A

```text
Ping Scan
```

### Scan B

```text
Nmap -T5 Scan
```

### Benefits

- Identifies differences between scans
- Detects newly opened services
- Tracks network changes
- Supports security auditing

---

# Key Findings

## Security Onion (192.168.1.6)

Open Ports:

- 22/tcp (SSH)
- 25/tcp (SMTP)
- 80/tcp (HTTP)
- 514/tcp (Shell)

---

## Ubuntu Server (192.168.1.50)

Open Ports:

- 21/tcp (FTP)
- 22/tcp (SSH)
- 23/tcp (Telnet)
- 80/tcp (HTTP)

---

## Dawn Vulnerable Linux (10.1.1.10)

Open Ports:

- 22/tcp (SSH)
- 139/tcp (NetBIOS)
- 199/tcp (SMUX)
- 445/tcp (Microsoft-DS)
- 631/tcp (IPP)
- 3306/tcp (MySQL)

Detected Operating System:

```text
Linux Kernel 2.6.x
```

---

# Skills Demonstrated

- Network reconnaissance
- Host discovery
- TCP scanning
- Service enumeration
- OS fingerprinting
- Port analysis
- Packet inspection
- Network topology mapping
- Security assessment
- Zenmap visualization

---

# Tools Used

- Kali Linux
- Nmap
- Zenmap
- pfSense Firewall
- Ubuntu Linux
- Security Onion
- Dawn Vulnerable Linux (DVL)

---

# Lessons Learned

This lab demonstrated how Nmap and Zenmap can be leveraged to identify live hosts, enumerate services, fingerprint operating systems, and visualize network infrastructure. The combination of command-line scanning and graphical analysis provides a powerful approach for security assessments, vulnerability discovery, and network reconnaissance.

---

## Disclaimer

This project was performed in a controlled lab environment for educational and defensive cybersecurity purposes only. All scanning activities were conducted against authorized systems.
