###Incident Response Report: Network Reconnaissance
Incident ID: 2026-04-08-RECON

Status: Closed (Simulation)

Lead Analyst: Oscar Lopez

##1. Preparation
Infrastructure: Host-based monitoring via Wazuh SIEM on an Ubuntu Server (192.168.56.108).

Tools: Nmap (Network Mapper) via Kali Linux (192.168.56.106).

Objective: Validate if the existing HIDS (Wazuh) can detect TCP-based port scanning.

##2. Detection & Analysis
Discovery
Reconnaissance activity was identified through manual review of attacker logs after the SIEM failed to trigger an automated alert.

Incident Details
Attack Vector: Network Service Discovery (MITRE ATT&CK T1046).

Technical Details: A TCP Connect Scan (nmap -sT) was executed against the target.

Findings:

Open Ports: 22 (SSH), 80 (HTTP), 443 (HTTPS).

Filtered Ports: Majority of the port range.

SIEM Performance: ❌ No Alerts Generated. Analysis confirms that because the scan did
not result in a login attempt or OS-level log entry (e.g., in /var/log/auth.log), the 
host-based agent remained silent.

##3. Containment, Eradication, & Recovery
Containment: As this was a controlled simulation, no immediate isolation was required. 
In a production environment, the attacker IP (192.168.56.106) would be temporarily 
throttled or blocked at the firewall.

Eradication: Not applicable (no persistence or malware was deployed).

Recovery: Service integrity remains intact; however, the attack surface (ports 22, 80, 443)
is now confirmed as "mapped" by an external party.

##4. Post-Incident Activity (Lessons Learned)
Root Cause Analysis
The visibility gap exists because Wazuh is a Host-Based IDS (HIDS). It lacks visibility into 
network-layer traffic that does not interact with the system's logging daemon or authentication modules.

##Recommended Improvements
Deploy NDR: Implement Security Onion for network-level visibility.  High  Security team
Firewall Logging: Enable and export logs from pfSense to the SIEM.  Medium Network Admin
Service Hardening: Review the necessity of exposed ports (SSH/HTTP) to reduce the attack surface. Medium System Admin

##Conclusion
This simulation successfully demonstrated a critical "blind spot" in a host-only monitoring strategy. Comprehensive 
security requires the correlation of both host-based logs and network-based traffic analysis.

