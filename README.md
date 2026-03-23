# High Availability E-commerce Network Defense 🛡️

![pfSense](https://img.shields.io/badge/pfSense-Firewall-red?style=flat-square&logo=pfsense)
![Suricata](https://img.shields.io/badge/Suricata-IPS-orange?style=flat-square)
![OpenVPNG](https://img.shields.io/badge/OpenVPN-Secure_Access-blue?style=flat-square&logo=openvpn)
![EVE-NG](https://img.shields.io/badge/EVE--NG-Network_Emulation-lightgrey?style=flat-square)
![Status](https://img.shields.io/badge/Status-InProgress-orange?style=flat-square)

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

## ✨ Features

### 1. Active-Passive HA Cluster
* Utilization of the **CARP** for IP address redundancy.
* Implementation of the **pfsync** mechanism on a dedicated interface for continuous State Table synchronization. 
* **Result:** In the event of a physical Master node failure, the Backup node takes over the traffic in under 3 seconds without dropping active TCP sessions.

### 2. Zero Trust Architecture
* Deployment of **VLANs (802.1Q)** to strictly isolate traffic zones (LAN, DB, WAN).
* Configuration of strict Role-Based Access Control firewall rules utilizing a *Default Deny* policy.

### 3. Advanced Protection
* **Suricata (IPS):** Deep Packet Inspection with active threat blocking.
* **pfBlockerNG-devel:** Reputation-based protection and geographical traffic blocking from high-risk countries on the WAN interface.

### 4. Secure Remote Access
* Deployment of an **OpenVPN** server using strong cryptography (AES-256-GCM).
* Certificate-based user authentication (Public Key Infrastructure - PKI).

### 5. Cost Efficency
* Enterprise-level edge protection achieved without expensive proprietaty licensing.

## The Process

## 📊 Proof of Concept / Testing

### Test 1: Failover Test
* **Scenario:** Simulated a sudden hardware failure of the Master pfSense node during active traffic.
* **Result:** The Backup node successfully detected the missing CARP heartbeats and assumed the Master role. The failover occured in ~3 seconds, and thanks to 'pfsync' state replication, active TCP sessions were not dropped.

![Failover Wireshark Test](wireshark_failover_test.png)
![Failover Ping Test](icmp_failover_test.png)

*> ICMP traffic dropping and recovering in ~3 seconds without breaking the session.*

### Test 2: Penetration Testing vs. IPS
* **Scenario:** An aggressive SYN port scan ('nmap -sS -Pn) was launched from an external WAN machine to map the edge network.
* **Result:** Suricata successfully detected the scanning signatures ('ET SCAN Possible Nmap User-Agent Observed') and immediately dropped the attacker's IP at the network layer. The firewall entered Stealth Mode, returning zero information.

![Nmap Port Scan](nmap_port_scan.png)

*> An attempt to map the edge network*

![Suricata Alerts](suricata_IPS_alert.png)
![Suricata Blok](suricata_IPS_block.png)

*> Automatic IP block applied by Suricata IPS in response to the scan.*

## What I Learned
* **Virtualization Nuances with IDS/IPS:** I discovered that hardware checksum offloading in hypervisors corrupts packets from Suricata's prespective. I learned how to troubleshoot this by disabling Hardware Cheksum Offloading in pfSense to allow proper deep packet inspection.
* 

## What can be improved


## 🚀 How to run this Project



*This project is for educational and demonstration purposes. It was prepared as part of engineering documentation validating practical skills in network engineering and cybersecurity.*
