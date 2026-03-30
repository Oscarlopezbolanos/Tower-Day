## Incident Report: SSH Lateral Movement

# Incident Summary

A lateral movement attack was simulated using valid SSH credentials to access an internal Linux system. The activity was successfully detected by Wazuh through authentication log monitoring.

---

# Timeline of Events

Time| Event
HH:MM| SSH login initiated from attacker
HH:MM| Authentication successful
HH:MM| User session established

---

# Detection Method

- Wazuh SIEM
- Log analysis of "/var/log/auth.log"
- PAM authentication monitoring

---

# Incident Description

After gaining initial access, the attacker used valid credentials to log into an internal Ubuntu server via SSH. This action represents lateral movement within the network using legitimate services.

---

# Impact Assessment

- No system compromise beyond authorized access
- Demonstrates risk of credential reuse
- Highlights visibility of internal movement

---

# Root Cause

The system allowed SSH authentication using valid credentials without additional restrictions such as multi-factor authentication or IP filtering.

---

# Response Actions (NIST Framework)

# 1. Detection & Analysis

- Wazuh detected SSH login activity
- Logs confirmed source IP and user

# 2. Containment

- Recommend restricting SSH access
- Limit allowed IP ranges

# 3. Eradication

- No malicious persistence established

# 4. Recovery

- System remained operational

---

# Lessons Learned

- Credential-based attacks are difficult to distinguish from legitimate use
- Logging and monitoring are critical for detection
- Lateral movement is a key phase in real-world attacks

---

# Recommendations

- Disable password-based SSH authentication
- Implement SSH key-based authentication
- Enforce multi-factor authentication
- Restrict SSH access via firewall rules (pfSense)

---

📊 MITRE ATT&CK Mapping

- T1021.004 – Remote Services (SSH)
- T1078 – Valid Accounts
