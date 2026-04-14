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
```
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


# Technical Guide: Windows SMB Hardening
**Project:** Hybrid Private Cloud Architecture  
**Focus:** Mitigation of Unauthorized Data Access  
**Target:** Windows 10 Workstation (`192.168.56.20`)

---

## 1. The Vulnerability: Legacy SMB
> **Functional Purpose:** Server Message Block (SMB) is the "internal delivery service" Windows uses to share files and printers. By default, older versions (SMBv1) are highly insecure and act as a wide-open door for attackers to steal data or spread malware like ransomware.

* **Identified Risk:** SMBv1 lacks modern encryption and is susceptible to "Man-in-the-Middle" and "Brute Force" attacks.
* **Discovery Tool:** Security Validation was performed using **Hydra** and **Nmap** from the Kali Linux node.

## 2. Implementation: Disabling Legacy Protocols
> **Functional Purpose:** In the "Bare Metal" approach, if you don't need a service, you kill it. By disabling SMBv1, we remove the most common entry point used in major global cyberattacks (such as WannaCry).

### **Hardening Steps:**
1.  **Registry/Feature Removal:** - Navigated to `Turn Windows features on or off`.
    - Unchecked `SMB 1.0/CIFS File Sharing Support`.
2.  **PowerShell Verification:** - Executed `Get-SmbServerConfiguration | Select EnableSMB1Protocol` to ensure the status returned as `False`.

## 3. Configuration: Enforcing SMB Signing
> **Functional Purpose:** Think of "SMB Signing" like a digital wax seal on an envelope. It proves that the file hasn't been tampered with while moving across the network. Even if an attacker "grabs" the data, they cannot change it without breaking the seal.

### **Security Policy Adjustment:**
* **Tool:** Local Group Policy Editor (`gpedit.msc`)
* **Path:** `Computer Configuration \ Windows Settings \ Security Settings \ Local Policies \ Security Options`
* **Action:** Enabled `Microsoft network server: Digitally sign communications (always)`.



## 4. Firewall Integration (Network Level)
> **Functional Purpose:** Host-based security isn't enough. We also told our "Security Guard" (pfSense) to block any SMB traffic that doesn't come from a known, authorized administrator.

* **Port Block:** Port 445 (SMB) traffic is now restricted via the **pfSense Firewall** to prevent "East-West" lateral movement from compromised nodes.

## 5. Verification of Hardening
After these steps were implemented, a follow-up scan from the **Kali Linux** node was conducted:
* **Result:** SMBv1 connection refused.
* **Result:** Attempts to intercept traffic failed due to forced encryption/signing.

---
> **Strategic Note:** Hardening is about reducing the "Attack Surface." By making SMB secure and invisible to unauthorized scanners, we have transformed a major liability into a protected corporate service.











