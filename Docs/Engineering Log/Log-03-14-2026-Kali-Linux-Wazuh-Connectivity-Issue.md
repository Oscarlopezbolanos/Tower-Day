## Engineering Log: Wazuh Agent Connectivity Issue
**Date:** March 14, 2026  
**Status:** Completed

---

## 1. Technical Challenge
* **Kali:** Linux was seen as disconnected in the Wazuh Dashboard
* **Cause:** was the ossec.conf file referenced old manager IP

## 2. Resolution Steps
* **Configuration Update:** Manually edited the `/var/ossec/etc/ossec.conf` file on the Kali Linux VM
* **Address Correction:** Updated the `<server><address>` tag from the legacy IP to the active Manager IP `192.168.56.107`
* **Service Restart:** Executed `systemctl restart wazuh-agent` to apply the new parameters
* **Verification:** Confirmed the agent status returned to `Active` via Wazuh WebUI
> **Strategic Note:** This event demonstrates the importance of Static IP Management in a secure environment. When infrastructure moves, documentation and configuration files must be synchronized to prevent a loss of visibility.
