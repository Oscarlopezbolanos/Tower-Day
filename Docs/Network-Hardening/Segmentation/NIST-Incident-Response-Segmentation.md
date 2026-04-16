# Engineering Standard: Network Segmentation & Micro-Isolation
**Project:** Hybrid Private Cloud Architecture  
**Architects:** Oscar Lopez-Bolanos & Patrick Cassibry  
**Framework:** Zero Trust Architecture (ZTA) / NIST 800-207  
**Gateway Authority:** pfSense (VLAN Management)

---

### **Definitions & Core Tooling**
> **Functional Purpose:** To ensure architectural clarity, we define the primary frameworks used in this security validation:
> * **Micro-segmentation:** The practice of dividing a network into distinct security zones (VLANs) to limit the "blast radius" of an attack.
> * **Flat Network:** A traditional, linear network where all devices can communicate freely. This represents a significant security risk as one compromised host can reach any other host.
> * **Logic-Gapped Architecture:** Using firewall rules to emulate an "air-gap," ensuring that different functional zones (e.g., Attacker vs. Services) are isolated by default.

---

## 1. Strategic Overview
> **Strategic Intent:** This document outlines the transition from a traditional linear network to a **Segmented, Logic-Gapped architecture.** By isolating critical services (Nextcloud) from testing environments (Kali) and monitoring tools (Wazuh), we ensure that an attacker in one VLAN cannot "pivot" to another without being filtered and logged by the **pfSense** gateway.

## 2. Technical Implementation: The Segregated Lab
Our environment is divided into four high-integrity VLANs, each serving a unique functional purpose within the security ecosystem.

### **The Architecture Blueprint:**
![Screenshot: Segmented Network Architecture](./Screenshots/Segmented-Architecture.png)

| Segment | Purpose | Subnet | Security Posture |
| :--- | :--- | :--- | :--- |
| **VLAN 10** | **SIEM Monitoring** | 192.168.10.0/24 | Protected management zone for the Wazuh Manager. |
| **VLAN 20** | **Attacker Testing** | 192.168.20.0/24 | Isolated "Dirty" zone for Kali Linux simulations. |
| **VLAN 30** | **User Endpoints** | 192.168.30.0/24 | Hardened production zone for Windows 10/11 hosts. |
| **VLAN 40** | **Business Services** | 192.168.40.0/24 | High-security zone for the Nextcloud storage server. |

---

## 3. Defensive Control & Visibility
> **Functional Purpose:** A flat network allows an attacker to see every device. A segmented network forces the attacker to "knock on the door" of the firewall for every move they make.

### **pfSense Firewall Logic:**
1. **Implicit Deny:** By default, all "East-West" traffic between internal VLANs is blocked. 
2. **Strict Filtering:** Only specific, pre-authorized "Whitelisted" ports are allowed for inter-VLAN communication.
3. **Forensic Logging:** Any attempt to probe or bypass these segments is logged by pfSense and forwarded to the **Wazuh SIEM** for immediate alerting.

---

## 4. MITRE ATT&CK Mapping
| ID | Technique | Tactics |
| :--- | :--- | :--- |
| **T1090** | Proxy | Command and Control (Using pfSense as the controlled gateway) |
| **T1571** | Non-Standard Port | Defense Evasion (Filtering non-standard traffic between VLANs) |
| **T1018** | Remote System Discovery | Discovery (Prevented by VLAN-level ICMP suppression) |

---

## 5. The Real-World Cost of Inaction
* **Ransomware "Fast-Track":** In a flat network, ransomware can encrypt an entire company in minutes. In our segmented architecture, the ransomware is "trapped" within a single VLAN, preventing a total business shutdown.
* **Compliance Failure:** Modern audits (SOC2, PCI-DSS, HIPAA) strictly require network segmentation. A flat network is an automatic failure for any business handling customer or financial data.
* **Unmonitored Lateral Movement:** Without VLANs, internal traffic is invisible to the firewall. Segmentation brings all "East-West" movement into the light, providing the SOC with the visibility needed to respond to active threats.

---
