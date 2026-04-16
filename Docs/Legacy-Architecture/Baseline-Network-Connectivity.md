# Network Connectivity Validation: Initial Baseline
**Project:** Hybrid Private Cloud Architecture  
**Architects:** Oscar Lopez-Bolanos & Patrick Cassibry
**Classification:** Internal Infrastructure Audit

---

## 1. Strategic Overview
> **Functional Purpose:** A secure network is only as effective as its underlying routing. This document verifies "Line-of-Sight" connectivity between all virtual nodes and the Management Host. By utilizing **Tailscale** as a Zero-Trust VPN, we establish an encrypted tunnel to the Windows 11 Desktop (Hypervisor), allowing for secure **RustDesk** administration of the virtualized environment from any remote location.

---

## 2. Connectivity & Management Matrix
This table tracks the successful handshakes between the primary lab assets and the Remote Management entry point.

| Source Asset | Destination Asset | Protocol | Result | Functional Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **Remote Admin** | **Windows Host** | **Tailscale (WireGuard)** | PASS | **Secure Transport:** Encrypted VPN tunnel for remote access. |
| **Remote Admin** | **Windows Host** | **RustDesk (TCP)** | PASS | **Remote Management:** Accessing the Type 2 Hypervisor GUI. |
| **Windows Host** | **pfSense-GW** | HTTPS (443) | PASS | **Firewall Admin:** Configuring the perimeter gateway. |
| **Security Testing** | **Wazuh-Mgr** | TCP (1514) | PASS | **SOC Ingestion:** Security telemetry flow. |
| **Ubuntu-Srv** | **Wazuh-Mgr** | TCP (1515) | PASS | **SOC Enrollment:** Registering new security agents. |

---

## 3. The Management Architecture
The project utilizes a multi-layered approach to ensure management is both convenient and highly secure:
1. **The Tunnel (Tailscale):** Creates a private "Overlay Network." This allows the admin to connect to the host without exposing any ports to the public internet.
2. **The Interface (RustDesk):** Provides the graphical interface needed to manage the Windows 11 Host.
3. **The Hypervisor (VirtualBox):** The Type 2 Hypervisor used to manage the "Bare Metal" configurations of the Ubuntu and Kali VMs.

---

## 4. Engineering Findings
Verification confirms that the **Tailscale + RustDesk** combination effectively bridges the gap between the remote admin and the local hypervisor. This architecture satisfies the "Private" requirement of the Cloud Architecture by ensuring the lab remains invisible to the public internet while maintaining a 100% encrypted management path.

> **Strategic Note:** In this configuration, if an attacker finds your home IP address, they find nothing. Access is only possible for devices authenticated within the Tailscale mesh, which significantly hardens the management plane against external brute-force attacks.

---
