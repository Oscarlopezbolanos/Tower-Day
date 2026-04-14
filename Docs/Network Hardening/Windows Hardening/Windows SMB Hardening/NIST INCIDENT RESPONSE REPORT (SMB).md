#  NIST INCIDENT RESPONSE REPORT (SMB)

```markdown
# Incident Report – SMB Exposure and Mitigation

##  Incident Summary

An exposed SMB service (TCP 445) was identified on a Windows host during internal
network testing. Despite firewall rules configured on pfSense, the service remained accessible due to flat network design.

---

##  Detection

Tool Used:
- Nmap

Command:
```bash
nmap -p 445 192.168.56.120

## Finding:

445/tcp open microsoft-ds

## Analysis
Root Cause:
Flat Layer 2 network allowed direct communication
Traffic did not traverse pfSense firewall
Firewall rules were effectively bypassed

## Risk Assessment

Severity: High
Potential Impact:
Lateral movement
Unauthorized file access
Ransomware propagation (e.g., SMB exploitation)

## Containment

Immediate action taken:
Disabled SMBv1 protocol
Blocked TCP port 445 using Windows Firewall
Command used:
PowerShell
New-NetFirewallRule -DisplayName "Block SMB" -Direction Inbound -Protocol TCP -LocalPort 445 -Action Block

## Eradication

No malware present
Vulnerability mitigated through configuration hardening

## Recovery

Validation performed using Nmap:
Bash
nmap -p 445 192.168.56.120

## Result:

445/tcp filtered microsoft-ds
 Service no longer accessible

## Lessons Learned

Network segmentation is critical
Flat networks allow firewall bypass
Endpoint controls are essential as a fallback
SMB should be tightly controlled or disabled when unnecessary

## Recommendations

Implement VLAN-based segmentation
Route all inter-network traffic through pfSense
Restrict SMB access to trusted hosts only
Monitor SMB activity via SIEM (Wazuh)

## MITRE ATT&CK Mapping

T1021.002 – SMB/Windows Admin Shares (Lateral Movement)

## Conclusion

The SMB exposure was successfully mitigated using endpoint-level controls, demonstrating the importance of layered security and proper network design.
