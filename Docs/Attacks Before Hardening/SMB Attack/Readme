# SMB Enumeration Attack (Lateral Movement Simulation)

##  Objective
Simulate an SMB enumeration attack from Kali Linux against a Windows 10 machine to demonstrate lateral movement techniques and detection using Wazuh SIEM.

---

##  Attack Description
The attacker (Kali Linux) attempts to enumerate SMB shares on a Windows system using `smbclient`. This simulates a common post-compromise lateral movement technique used by attackers after gaining initial access.

---

##  Lab Environment

| System | IP Address | Role |
|--------------|----------------|------|
| Kali Linux | 192.168.56.106 | Attacker |
| Windows 10 | 192.168.56.120 | Target |
| Wazuh SIEM | 192.168.56.104 | Monitoring |

---

##  Attack Execution

### Step 1: Verify Connectivity
```bash
ping 192.168.56.120
Step 2: Scan SMB Port
Bash
nmap -p 445 192.168.56.120
Step 3: Enumerate SMB Shares
Bash
smbclient -L //192.168.56.120 -U guest
Step 4: Attempt Authentication
Bash
smbclient -L //192.168.56.120 -U hnolbtg

## Observed Behavior
Multiple failed login attempts
Successful connection showing:
ADMIN$
C$
IPC$
Indicates access to administrative shares

## Wazuh Detection

Wazuh generated logs showing:
Logon failures (invalid credentials)
Successful remote logon events
NTLM authentication activity
Example alerts:
Logon failure - Unknown user or bad password
Successful Remote Logon Detected

## Security Impact

SMB allows attackers to:
Enumerate network shares
Attempt credential reuse
Move laterally across systems
Exposure of administrative shares increases risk of full compromise

## Mitigation (To be implemented in Hardening Phase)

Disable SMBv1
Restrict SMB access via firewall (port 445)
Enforce strong password policies
Limit administrative share access
Enable account lockout policies

## Conclusion

This attack demonstrates how attackers can leverage SMB to move laterally within a network. Detection through Wazuh highlights the importance of monitoring authentication events and network activity.

