# Engineering Log: Ubuntu Cloud-Lab Deployment & SIEM Integration
**Date:** March 15, 2026   
**Status:** Operational / Verified

---

## 1. Strategic Overview
> **Functional Purpose:** To establish a "Security Camera" (Wazuh) on the "Digital Filing Cabinet" (Nextcloud), the Ubuntu server required integration into the SOC monitoring environment. This log documents a sequential chain of technical hurdles encountered during a manual "Bare Metal" installation and the subsequent network alignment required for a secure build.

---

## 2. Infrastructure Inventory (Deployment Phase)
| Component | Role | IP Address | Status |
| :--- | :--- | :--- | :--- |
| **Wazuh Manager** | SIEM / Log Analysis | `192.168.56.107` | Online |
| **Ubuntu Server** | Nextcloud Hosting | `192.168.56.108` | **Target Node** |
| **Kali Linux** | Security Testing | `192.168.56.106` | Monitoring |

---

## 3. Technical Challenges & Resolutions
The deployment process revealed a chain of dependencies where network isolation and version drift prevented successful telemetry ingestion.

### **Phase A: Network Segmentation (Issue 1)**
* **Challenge:** The VM was initially isolated on a NAT network, preventing communication with the Wazuh Manager on the `192.168.56.0/24` lab subnet.
* **Resolution:** Reconfigured the VirtualBox adapter to **Host-Only**, restoring internal "line-of-sight" between the agent and the manager.

### **Phase B: Version Dependency & Package Management (Issues 2 & 3)**
* **Challenge:** A version mismatch (Agent 4.14.x vs. Manager 4.7.x) caused an immediate registration failure. Furthermore, an incorrect repository URL resulted in a corrupted 111-byte package download.
* **Resolution:** 1. Manually purged the incompatible agent: `sudo apt remove wazuh-agent`.
  2. Corrected the repository path to fetch the precise `4.7.3-1` build.
  3. **Verification:** Confirmed the `~13 MB` package size via `ls -lh wazuh-agent_4.7.3-1_amd64.deb`.

### **Phase C: Authentication & Manual Hardening (Issues 4 & 5)**
* **Challenge:** Post-installation, the agent failed to initialize because the configuration file still contained the `MANAGER_IP` placeholder.
* **Resolution:** 1. **Authentication:** Executed `/var/ossec/bin/agent-auth -m 192.168.56.107` to receive a valid security key.
  2. **Manual Configuration:** Used `nano` to edit `/var/ossec/etc/ossec.conf`, replacing the placeholder with the static Manager IP: `192.168.56.107`.
  3. **Service Initialization:** Enabled and started the service to ensure persistence:
     ```bash
     sudo systemctl enable wazuh-agent
     sudo systemctl start wazuh-agent
     ```

---

## 4. Final System Validation
The SOC dashboard now confirms successful ingestion from the Cloud Lab server.

| Agent | Status | Connectivity |
| :--- | :--- | :--- |
| **Cloud Lab Server** | **Active** | Verified via SIEM Telemetry |
| **Kali Linux** | **Active** | Verified |
| **Windows Guest** | Disconnected | Scheduled for Re-alignment |

---

## 5. Engineering Takeaways
> **Strategic Note:** This deployment highlighted the **"Version Match Rule"** in security architecture: an agent can never be newer than its manager. Furthermore, it proved that manual "Bare Metal" configuration is the only way to ensure placeholders like `MANAGER_IP` are correctly mapped, preventing "blind spots" in the monitoring network.

---
