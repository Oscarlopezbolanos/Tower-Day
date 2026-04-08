
## Attack Simulation: SSH Pivot (Lateral Movement)

# Objective

Simulate lateral movement within a segmented network by leveraging valid SSH credentials to access an internal Linux system after initial compromise.

---

# Lab Environment

Component| Details
SIEM| Wazuh
Attacker| Kali Linux (192.168.56.106)
Initial Target| Nextcloud / Ubuntu Server (192.168.56.108)
Network| 192.168.56.0/24 (pfSense segmented lab)

---

# Key Findings

- Attack Type: Lateral Movement via SSH
- Attacker IP: 192.168.56.106
- Target IP: 192.168.56.108
- Detection: Successful
- MITRE Technique: T1021.004 (Remote Services: SSH)

---

# Attack Execution

# Step 1: Initial Access (Previously Established)

The attacker obtained valid SSH credentials through a brute force attack.

---

# Step 2: SSH Pivot

ssh user@192.168.56.108

---

# Step 3: Internal Enumeration

whoami
ip a

---

# Detection Results (Wazuh)

Wazuh successfully detected:

- SSH login events
- PAM authentication logs
- User session creation
- Source IP associated with login

---

# Analysis

The attack leveraged valid credentials to move laterally within the network. Since SSH access is a legitimate service, detection relied on log analysis rather than signature-based blocking.

Wazuh collected authentication logs from "/var/log/auth.log" and generated alerts related to login activity and session establishment.

---

# Challenges & Troubleshooting

Issue| Resolution
No logs initially| Verified agent connectivity
Missing SSH alerts| Confirmed auth.log monitoring in Wazuh config

---

# MITRE ATT&CK Mapping

- T1021.004 – Remote Services (SSH)
- T1078 – Valid Accounts

---

# Lessons Learned

- Lateral movement often uses legitimate credentials
- SSH activity must be monitored closely
- Detection depends heavily on log visibility
