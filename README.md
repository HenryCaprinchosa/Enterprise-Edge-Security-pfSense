# High Availability E-commerce Network Defense 🛡️

![pfSense](https://img.shields.io/badge/pfSense-Firewall-red?style=flat-square&logo=pfsense)
![Suricata](https://img.shields.io/badge/Suricata-IPS-orange?style=flat-square)
![OpenVPNG](https://img.shields.io/badge/OpenVPN-Secure_Access-blue?style=flat-square&logo=openvpn)
![EVE-NG](https://img.shields.io/badge/EVE--NG-Network_Emulation-lightgrey?style=flat-square)
![Status](https://img.shields.io/badge/Status-Done-green)

In modern e-commerce, a Single Point of Failure (SPOF) at the network edge means unacceptable downtime and financial loss. This project elmininates SPOF by implementing a fully redundant, High Availability cluster and a Zero Trust network architecture using strictly open-source solutions to achive enterprise-grade security.

![Network Topology](logical_topology.png)
*> Logical topology of the designed enviroment.*

## 🛠️ Technologies
* **Firewall & Routing:** pfSense
* **Network Security:** Suricata, pfBlockerNG, OpenVPN
* **Auditing:** Wireshark, Nmap, iperf3, Netcat, Syslog
* **Layer 2:** Cisco vIOS L2
* **Client Systems:** Ubuntu MATE 24.04.3 LTS
* **Server Systems:** Ubuntu Server 24.04 LTS
* **Virtualization Environment:** EVE-NG, VMWare Workstation Pro

## ✨ Key Implemented Features

### 1. High Availability Cluster (HA)
* Utilization of the **CARP** (Common Address Redundancy Protocol) for IP address redundancy.
* Implementation of the **pfsync** mechanism on a dedicated interface (VLAN) for continuous State Table synchronization. 
* **Result:** In the event of a physical Master node failure (e.g., power outage), the Backup node takes over the traffic in under 3 seconds without dropping active TCP sessions.

### 2. Network Segmentation & RBAC (Zero Trust)
* Deployment of **VLANs (802.1Q)** to strictly isolate traffic zones (LAN, DMZ, WAN, SYNC).
* Configuration of strict Role-Based Access Control (RBAC) firewall rules utilizing a *Default Deny* policy.

### 3. Advanced Protection (IDS/IPS & Geo-blocking)
* **Suricata (IPS):** Deep Packet Inspection (DPI) with active threat blocking (including port scanning and DoS attacks).
* **pfBlockerNG-devel:** Reputation-based protection and geographical traffic blocking from high-risk countries on the WAN interface.

### 4. Secure Remote Access
* Deployment of an **OpenVPN** server using strong cryptography (AES-256-GCM).
* Certificate-based user authentication (Public Key Infrastructure - PKI).

---

## 📊 Verification & Testing (Proof of Concept)

### Failover Test
The MAC address change in the packet capture indicates the immediate takeover of the VIP address by the standby firewall node without interrupting communication.
![CARP Failover Proof](failover.png)

### Intruder Blocking (Suricata)
The system automatically detects and drops packets during an aggressive network scanning attempt using Nmap.
![Suricata IDS/IPS Proof](suricata.png)

---

## 🚀 How to run this lab?
1. Import the `.unl` (or `.zip`) topology file into your EVE-NG server.
2. Ensure you have the `pfSense` and `vios-l2` images uploaded to the appropriate EVE-NG directories.
3. Import the `.xml` configuration files (located in the `configs/` directory) directly into the pfSense nodes to restore the HA cluster and firewall rules.

---
*This project is for educational and demonstration purposes. It was prepared as part of engineering documentation validating practical skills in network engineering and cybersecurity.*
