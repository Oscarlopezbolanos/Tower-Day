# SMB Hardening – Endpoint Security Control (Windows Firewall)

## 📌 Objective
After discovering that network-level controls (pfSense firewall rules) were ineffective due 
to a flat network architecture, endpoint hardening was implemented to mitigate SMB-based lateral movement.

---

## ⚠️ Problem Identified

Despite implementing pfSense firewall rules to block SMB (TCP 445):

- SMB port remained accessible
- Nmap scans from Kali showed:
  
  445/tcp open microsoft-ds

### Root Cause:
The environment was configured as a **flat Layer 2 network**, allowing direct communication between hosts without passing through pfSense.

---

## 🛠️ Solution: Endpoint Hardening (Windows)

To mitigate this, SMB access was restricted directly on the Windows machine.

### Step 1: Disable SMBv1 (Legacy Protocol)

```powershell
Set-SmbServerConfiguration -EnableSMB1Protocol $false -Force
Step 2: Block SMB via Windows Firewall
PowerShell
New-NetFirewallRule -DisplayName "Block SMB" `
-Direction Inbound `
-Protocol TCP `
-LocalPort 445 `
-Action Block
🧪 Validation
Before Hardening:
Bash
nmap -p 445 192.168.56.120
Result:

445/tcp open microsoft-ds
After Hardening:
Bash
nmap -p 445 192.168.56.120
Result:

445/tcp filtered microsoft-ds
✔ SMB access successfully blocked
✔ Lateral movement mitigated
🔍 Key Security Insight
This test demonstrates:
Network security controls alone are insufficient in flat architectures.
Even with firewall rules:
Traffic bypassed pfSense
Direct host-to-host communication was still possible
🛡️ Security Principle Applied
Defense in Depth
❌ Network Layer: Failed (pfSense bypassed)
✅ Endpoint Layer: Successful (Windows Firewall blocked SMB)
🎯 Real-World Impact
In enterprise environments:
Flat networks are high-risk
Endpoint hardening is critical when segmentation is weak
SMB is a common vector for:
Lateral movement
Ransomware propagation
Evidence
Screenshots included:
pfSense firewall rules
Nmap scans (before/after)
Windows PowerShell firewall rule creation
✅ Conclusion
Blocking SMB at the endpoint level effectively mitigated lateral movement risks, even when network segmentation controls were not fully enforced.
This reinforces the importance of layered security controls in cybersecurity architecture.
