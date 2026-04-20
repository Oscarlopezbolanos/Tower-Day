# Technical Guide: Windows SMB Hardening
**Project:** Hybrid Private Cloud Architecture  
**Focus:** Mitigation of Unauthorized Data Access  
**Target:** Windows 10 Workstation (`192.168.56.20`)

---

## 1. The Vulnerability: Legacy SMB
> **Functional Purpose:** Server Message Block (SMB) is the "internal delivery service" Windows uses to share files and printers. By default, older versions (SMBv1) are highly insecure and act as a wide-open door for attackers to steal data or spread malware like ransomware.

* **Identified Risk:** SMBv1 lacks modern encryption and is susceptible to "Man-in-the-Middle" and "Brute Force" attacks.
* **Discovery Tool:** Security Validation was performed using **Hydra** and **Nmap** from the Kali Linux node.

---

## 2. Implementation: Disabling Legacy Protocols
> **Functional Purpose:** In the "Bare Metal" approach, if you don't need a service, you kill it. By disabling SMBv1, we remove the most common entry point used in major global cyberattacks (such as WannaCry).

### **Hardening Steps:**
1.  **Registry/Feature Removal:** - Navigated to `Turn Windows features on or off`.
    - Unchecked `SMB 1.0/CIFS File Sharing Support`.
2.  **PowerShell Verification:** - Executed `Get-SmbServerConfiguration | Select EnableSMB1Protocol` to ensure the status returned as `False`.

---

## 3. Configuration: Enforcing SMB Signing
> **Functional Purpose:** Think of "SMB Signing" like a digital wax seal on an envelope. It proves that the file hasn't been tampered with while moving across the network. Even if an attacker "grabs" the data, they cannot change it without breaking the seal.

### **Security Policy Adjustment:**
* **Tool:** Local Group Policy Editor (`gpedit.msc`)
* **Path:** `Computer Configuration \ Windows Settings \ Security Settings \ Local Policies \ Security Options`
* **Action:** Enabled `Microsoft network server: Digitally sign communications (always)`.

---

## 4. Firewall Integration (Network Level)
> **Functional Purpose:** Host-based security isn't enough. We also told our "Security Guard" (pfSense) to block any SMB traffic that doesn't come from a known, authorized administrator.

* **Port Block:** Port 445 (SMB) traffic is now restricted via the **pfSense Firewall** to prevent "East-West" lateral movement from compromised nodes.

---

## 5. Verification of Hardening
After these steps were implemented, a follow-up scan from the **Kali Linux** node was conducted:
* **Result:** SMBv1 connection refused.
* **Result:** Attempts to intercept traffic failed due to forced encryption/signing.
> **Strategic Note:** Hardening is about reducing the "Attack Surface." By making SMB secure and invisible to unauthorized scanners, we have transformed a major liability into a protected corporate service.











