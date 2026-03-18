## Incident Report: SSH Brute Force Attack Detection

# Incident Summary

A simulated SSH brute force attack was conducted from a Kali Linux attacker machine targeting an Ubuntu server (cloud-lab). The activity was successfully detected using the Wazuh SIEM platform.

---

Environment

Role| System| IP Address
SIEM| Wazuh Manager| 192.168.56.104
Target| Ubuntu Server| 192.168.56.108
Attacker| Kali Linux| 192.168.56.106

---

# 1. Preparation

- Wazuh SIEM deployed and configured
- Agents installed on monitored endpoints
- SSH service enabled on target system
- Network connectivity validated

---

# 2. Detection & Analysis

Attack Execution

A brute-force attack was launched using Hydra:

hydra -l root -P rockyou.txt -t 4 ssh://192.168.56.108

Observed Indicators

- High volume of failed SSH login attempts
- Repeated authentication failures
- Rapid login attempts in short intervals

Wazuh Alerts

- sshd: authentication failed
- PAM: Multiple failed logins detected
- Maximum authentication attempts exceeded

Analysis

The activity matches a brute-force attack pattern, where an attacker attempts multiple password combinations to gain unauthorized access.

Mapped to MITRE ATT&CK:

- T1110 – Brute Force

---

# 3. Containment, Eradication, and Recovery

Immediate Actions (Recommended)

- Block attacker IP (192.168.56.106) via firewall (pfSense)
- Disable root SSH login
- Implement account lockout policies
- Enable fail2ban or similar protection

Long-Term Mitigation

- Enforce SSH key-based authentication
- Disable password authentication
- Monitor login attempts continuously
- Apply rate limiting

---

# 4. Post-Incident Activity

Lessons Learned

- Detection depends on real attack traffic
- Missing dependencies can delay testing (wordlists issue)
- SIEM visibility is effective for authentication-based attacks

Improvements

- Configure active response in Wazuh
- Integrate network-based detection (Security Onion)
- Create alert escalation rules

---

# Conclusion

The lab successfully demonstrated detection of a brute-force attack using Wazuh. The system generated meaningful alerts that can support incident response workflows in real-world environments.
