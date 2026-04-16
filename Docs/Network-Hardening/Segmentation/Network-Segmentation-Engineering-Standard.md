# Engineering Standard: Network Segmentation & Micro-Isolation
**Project:** Hybrid Private Cloud Architecture  
**Architects:** Oscar Lopez-Bolanos & Patrick Cassibry  
**Governance:** Zero Trust Architecture (ZTA) / NIST SP 800-207  
**Control Authority:** pfSense Firewall / VLAN 802.1Q

---

### **Definitions & Core Tooling**
> **Functional Purpose:** To move away from a "Flat" network where one compromise leads to a total breach, we utilize the following logic:
> * **Micro-segmentation:** The practice of dividing a network into distinct security zones (VLANs) to limit the "blast radius" of a security incident.
> * **Logic-Gap:** A security control that emulates a physical "air-gap" by using firewall stateful packet inspection to block all unauthorized inter-VLAN communication.
> * **Default Deny:** The fundamental posture where all traffic is prohibited unless explicitly allowed by a "Pinhole" firewall rule.

---

## 1. Strategic Intent
The objective of this standard is to eliminate "Linear" network vulnerabilities. In a traditional small business network, a compromise on a single workstation (VLAN 30) would allow an attacker to reach the database server (VLAN 40) or the SIEM (VLAN 10) without resistance. This standard mandates a **Stateful Firewall Gateway** (pfSense) to act as a "Choke Point" for all East-West traffic.

---

## 2. Technical Implementation: Segmented Topology
The architecture is structured into four high-integrity Virtual Local Area Networks (VLANs). No traffic is permitted to traverse these boundaries without explicit inspection.

| Segment ID | Subnet | Functional Role | Security Posture |
| :--- | :--- | :--- | :--- |
| **VLAN 10** | 192.168.10.0/24 | **SOC / Monitoring** | Management only; contains Wazuh Manager. |
| **VLAN 20** | 192.168.20.0/24 | **Untrusted / Testing** | Isolated zone for Kali Linux simulations. |
| **VLAN 30** | 192.168.30.0/24 | **User Endpoints** | Production zone for Windows 10/11 hosts. |
| **VLAN 40** | 192.168.40.0/24 | **Critical Services** | Restricted zone for Nextcloud and Data Assets. |

---

## 3. Firewall Enforcement Logic (pfSense)
To maintain the **Logic-Gap**, the pfSense gateway enforces the following mandatory traffic policies:

### **3.1 Isolation Requirements**
* **Inter-VLAN Block:** By default, all traffic between VLANs 20, 30, and 40 is dropped at the gateway.
* **ICMP Suppression:** Echo requests (Pings) are disabled between subnets to prevent unauthorized **Nmap** discovery and reconnaissance.
* **Management Pinhole:** Only the Wazuh Manager (VLAN 10) is authorized to initiate connections to other subnets for log collection and telemetry.

### **3.2 Validation Protocol**
To verify the integrity of the segmentation, administrators must perform a "Visibility Test" from the Untrusted Zone (VLAN 20):
```bash
# Verification: Attempt to ping the Critical Service zone
# Result: Connection Timeout (Packet Dropped by pfSense)
ping 192.168.40.1 -c 4

# Verification: Attempt to map internal interfaces
# Result: Scope limited to VLAN 20 local subnet only
ip route show
```

---

## 4. Defensive Visibility (SIEM Integration)
> **Functional Purpose:** A segmented network forces an attacker to "knock on the door" of the firewall for every move they make, creating a visible log for the SIEM.

1. **Deny-Log Forwarding:** Every "Default Deny" event in pfSense is forwarded to the **Wazuh SIEM**.
2. **Alert Triggering:** Unauthorized cross-VLAN probes are categorized as **Level 10 (Critical)** alerts, indicating a potential lateral movement attempt.
3. **Audit Trail:** Technical validation results and discovery scan logs are documented in our [NIST Segmentation Report](./NIST-Incident-Response-Style-Report.md).

---

## 5. Strategic Recommendations for Maturity
To further enhance this architecture, the following controls are recommended for future implementation:
* **MFA (Multi-Factor Authentication):** Enforce MFA for all inter-VLAN administrative jumps.
* **Intrusion Prevention (Snort/Suricata):** Enable Deep Packet Inspection (DPI) on the pfSense interfaces to detect exploit signatures within authorized traffic.

---
