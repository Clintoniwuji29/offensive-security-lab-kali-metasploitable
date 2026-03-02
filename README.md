# offensive-security-lab-kali-metasploitable
This project demonstrates a full offensive security lifecycle within a controlled virtual lab environment. The lab simulates a real-world attacker compromising a vulnerable machine, from initial network configuration to exploitation and packet-level traffic analysis.
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

