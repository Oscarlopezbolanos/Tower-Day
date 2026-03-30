# 📑 NIST Incident Response Report (FOR GITHUB)


# NIST Incident Report: SMB Enumeration Attack

## 1. Incident Summary
An SMB enumeration attack was performed from a Kali Linux system targeting a Windows 10 machine. The attacker attempted to discover accessible network shares and authenticate using multiple credentials.

---

## 2. Detection & Analysis

### Indicators Observed:
- Multiple failed login attempts
- NTLM authentication attempts
- Successful remote logon activity
- SMB share enumeration

### Source:
- IP: 192.168.56.106 (Kali)

### Target:
- IP: 192.168.56.120 (Windows 10)

### Wazuh Alerts:
- Logon failure events
- Successful remote login detection
- Authentication anomalies

---

## 3. Containment

Immediate actions recommended:
- Block SMB traffic (port 445) via firewall
- Isolate affected system if compromise suspected
- Disable unnecessary SMB services

---

## 4. Eradication

- Remove unauthorized access attempts
- Reset compromised credentials
- Review account permissions

---

## 5. Recovery

- Restore normal operations
- Re-enable services with proper restrictions
- Monitor logs for recurring activity

---

## 6. Lessons Learned

- SMB is a major lateral movement vector
- Weak or reused credentials increase risk
- Continuous monitoring (Wazuh) is critical
- Network segmentation can limit attack spread

---

## 7. Recommendations

- Implement network segmentation (pfSense)
- Restrict SMB access to trusted hosts only
- Enable multi-factor authentication (MFA)
- Enforce logging and alerting policies
- Regularly audit network shares

---

## 8. MITRE ATT&CK Mapping

| Technique | Description |
|----------|------------|
| T1021.002 | SMB/Windows Admin Shares |
| T1110 | Brute Force |
| T1078 | Valid Accounts |
