# Columbus State University Tower-Day 
>Design and Security Hardening of a Hybrid Private Cloud Architecture for Small Business Environments. This project designs and hardens a **hybrid “private cloud”** environment that delivers secure file sharing and **administrative remote desktop** while remaining feasible for small IT teams.  
*Authors: Oscar Lopez-Bolanos / Patrick Cassibry,*
>Cybersecurity Nexus Program
## Tower Day Lab Build Documentation
### **pfSense Network + Wazuh + Nextcloud + Kali Lab**


---


## 1. Environment

This environment includes:
* **pfSense:** Firewall and DHCP server
* **Wazuh:** SIEM monitoring platform
* **Nextcloud:** Secure private cloud storage and file sharing
* **Kali Linux:** Attacker machine
* **Windows 11 host:** Type 2 Hypervisor host system
* **Tailscale:** Zero-Trust VPN for secure remote management
* **RustDesk:** Self-hosted ID/Relay server for secure GUI based remote desktop

The lab network is isolated using **VirtualBox Host-Only** networking and managed through pfSense firewall rules and **DHCP services**.

## 2. Technology Stack

| Category | Tool | Role in Architecture |
| :--- | :--- | :--- |
| **Networking** | **pfSense** | Perimeter firewall and internal DHCP management. |
| **Connectivity** | **Tailscale** | Zero-Trust overlay network for secure administration. |
| **SIEM / SOC** | **Wazuh** | Real-time threat detection and security monitoring. |
| **Cloud Storage** | **Nextcloud** | Hardened private cloud for file synchronization. |
| **Analysis** | **Kali Linux** | Offensive security testing and vulnerability validation. |
| **Guest OS** | **Ubuntu Server** | Primary host for all containerized and standalone services. |
| **Hypervisor** | **Windows 11 (Host)** | Type 2 Hypervisor platform supporting the virtualized infrastructure. |
| **Remote Access** | **RustDesk** | Self-hosted ID/Relay server for secure GUI-based remote desktop. |

## 3. Objectives

[x] Architecture Design: Develop a functional hybrid "private cloud" bridging local and remote assets.

[x] Network Hardening: Implement a Zero-Trust networking model using Tailscale for secure administrative access.

[x] Infrastructure Deployment: Configure an Ubuntu-based server environment for hosted services like Nextcloud.

[x] Active Security Monitoring: Integrate Wazuh SOC for real-time threat detection and log analysis.

[ ] System Resilience: Hardening of the pfSense perimeter and internal virtual machines against common attack vectors.

[ ] Documentation & Scalability: Provide a clear blueprint for small IT teams to replicate this high-security, low-cost architecture.

## 4. Lab Architecture

The architecture utilizes a **Zero-Trust Network Access (ZTNA)** model. **Tailscale** provides the secure transport layer, while **RustDesk** and **Wazuh** provide the administrative and monitoring interfaces.

```text
 [ Remote Admin ] 
        |
        | (1) Authenticated WireGuard Tunnel (Tailscale)
        v
 [ Management PC ] <--- (2) Graphical Remote Desktop (RustDesk)
        |
        +--- VirtualBox Host-Only Network (192.168.56.0/24)
        |           |
        |    pfSense Gateway (192.168.56.1)
        |           |
        +-----------+-----------------------+----------------------+-------------------+
        |                                   |                      |                   |
    Wazuh SIEM                         Nextcloud VM            Kali Linux       RustDesk Server
  (192.168.56.104)                   (192.168.56.105)        (192.168.56.106)    (IP MISSING)

The internal lab is logically isolated. External management is only possible through the authenticated Tailscale overlay, preventing any exposure to the public internet.
```
## 5. Network Configuration

The internal lab network is managed by **pfSense** to ensure consistent IP addressing and DNS resolution for the virtualized services.

* **Internal Network:** `192.168.56.0/24`
* **pfSense LAN IP:** `192.168.56.1`
* **DHCP Range:** `192.168.56.100` – `192.168.56.200`
* **DNS Server:** `192.168.56.1`

## 6. Major Problems Encountered & Troubleshooting

During deployment, several configuration issues were identified and resolved to ensure cross-system communication.

### **Problem 1: Host-to-pfSense Connectivity**
* **Symptom:** Windows Host could not reach the pfSense WebGUI at `https://192.168.56.1` (Error: `ERR_CONNECTION_REFUSED`).
* **Root Cause:** The VirtualBox Host-Only Adapter lacked a valid IPv4 configuration to communicate on the `192.168.56.x` subnet.
* **Solution:** 1. Manually configured the **VirtualBox Host-Only Ethernet Adapter** in Windows Network Settings.
    2. Set Static IP: `192.168.56.5` | Subnet: `255.255.255.0` | Gateway: `None`.

---

### **Problem 2: Ubuntu Server DHCP Negotiation**
* **Symptom:** Ubuntu VMs were not requesting DHCP leases from pfSense, remaining stuck on static IPs or no connectivity.
* **Important Discovery:** Ubuntu Server 24.04 utilizes **Netplan** for network management; legacy commands like `dhclient` are not installed by default.
* **Root Cause:** The default Netplan configuration was hardcoded for a static interface rather than DHCP.
* **Solution:** 1. Edited the Netplan YAML file: 
       ```bash
       sudo nano /etc/netplan/50-cloud-init.yaml
       ```
    2. Updated configuration from static to `dhcp4: true`.
    3. Applied changes:
       ```bash
       sudo netplan apply
       ```
* **Result:** All machines successfully obtained leases:
    * **Wazuh:** `192.168.56.104`
    * **Nextcloud:** `192.168.56.105`
    * **Kali:** `192.168.56.106`


## 7. Final Working Environment



Machine	IP
pfSense	192.168.56.1
Wazuh	192.168.56.104
Nextcloud	192.168.56.105
Kali	192.168.56.106

## 8. Network Flow Diagram

Windows Management Host

```
    [ Windows Management Host ]
                  |
       [ Host-Only Adapter: .5 ]
                  |
            [ pfSense: .1 ]
                  |
      +-----------+-----------+
      |           |           |
  [ Wazuh ]  [ Nextcloud ] [ Kali ]
    (.104)      (.105)      (.106)
      ^           |           |
      |           |           |
      +-----------+-----------+
             (Traffic Flow)
                  |
        [ Security Monitoring ]
```

## 9. Lessons Learned

> **1. Host Networking Verification:** Virtual machine configuration is irrelevant if the host-only adapter is misconfigured. Always verify the host's physical and virtual bridge status first.
>
> **2. Modern Linux Networking:** Ubuntu 24.04 relies exclusively on **Netplan**. Traditional tools like `dhclient` are deprecated; mastery of YAML-based network configuration is essential.
>
> **3. DHCP vs. Static Conflict:** DHCP will not override an existing static configuration in Netplan. A "Clean Slate" approach in the YAML file is required for successful lease negotiation.
## 10. Next Steps

- [ ] **Firewall Segmentation:** Implement granular pfSense rules to isolate the Kali "Attacker" from the "Production" Nextcloud server.
- [ ] **Attack Simulations:** Execute controlled brute-force and SQL injection tests from Kali Linux.
- [ ] **Wazuh Log Analysis:** Build custom rulesets in Wazuh to detect and alert on the simulated attacks.
- [ ] **Hardening Comparison:** Document the "Security Posture" before and after hardening configurations are applied.
## 11. Conclusion & Troubleshooting Timeline

This lab demonstrates a secure, segmented infrastructure monitored by **Wazuh SIEM**. By resolving real-world networking hurdles, the environment is now a stable platform for cybersecurity research.

### **Troubleshooting Timeline**

1. **Connectivity Failure:** pfSense web interface unreachable.
2. **Investigation:** Audited host network adapter settings.
3. **Host Fix:** Manually assigned IPv4 to VirtualBox host-only adapter.
4. **DHCP Issue:** Ubuntu servers failed to pull IPs despite pfSense being active.
5. **Netplan Discovery:** Identified static YAML blocks overriding DHCP requests.
6. **YAML Correction:** Modified `/etc/netplan/` to enable `dhcp4: true`.
7. **Resolution:** All systems confirmed operational with consistent connectivity.


