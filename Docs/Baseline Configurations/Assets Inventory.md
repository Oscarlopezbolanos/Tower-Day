# Asset Inventory: Baseline Environment
**Project:** Hybrid Private Cloud Architecture  
**Architects:** Oscar Lopez-Bolanos & Patrick Cassibry  
**Classification:** Internal Baseline Documentation

---

## 1. Project Overview
> **Functional Role:** This project acts as a secure "office in a box." Before applying security locks (Hardening), we must catalog every virtual device in our environment to ensure 100% visibility.

## 2. Infrastructure Inventory

| Hostname | Technical Role | Functional Purpose | CPU/RAM | Platform | License |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **pfSense-GW** | Network Gateway | Perimeter defense & routing. | 1 vCPU / 1GB | FreeBSD | Open Source |
| **Wazuh-Mgr** | SIEM / SOC | Threat detection & alerting. | 2 vCPU / 4GB | Ubuntu 24.04 | Open Source |
| **Nextcloud-Srv** | Cloud Storage | Secure data sovereignty. | 1 vCPU / 2GB | Ubuntu 24.04 | Open Source |
| **Kali-Attacker** | Offensive Suite | Security validation testing. | 2 vCPU / 2GB | Kali Linux | Open Source |
| **RustDesk-Relay** | ID/Relay Server | Encrypted remote access. | 1 vCPU / 1GB | Ubuntu 24.04 | Open Source |

## 3. Deployment Specifications
* **Hypervisor:** Oracle VirtualBox (Type 2) - **Open Source**
* **Network Topology:** Isolated Host-Only Adapter (`192.168.56.0/24`)
* **Hardware Foundation:** Windows 11 Physical Host

> **Strategic:** We are using "computers inside of a computer." This allows us to simulate a full corporate data center on a single laptop or desktop, ensuring the entire environment is isolated from the real-world internet for safety.

## 4. Baseline Security Configuration
Before hardening, the following "weak" states exist for testing purposes:
* **Authentication:** Password-based login is active (rather than secure keys).
* **Network Traffic:** Internal "East-West" traffic is unmonitored.
* **Public Exposure:** Management ports are accessible within the local subnet.

---


















