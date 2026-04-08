# pfSense Network Hardening Attempt (Phase 1)

## 📌 Objective
Implement firewall rules using pfSense to prevent lateral movement within a flat network environment.

---

##  Background

The lab environment initially consisted of a flat network where all systems were connected within the same subnet:

- Network: 192.168.56.0/24
- pfSense LAN IP: 192.168.56.1

### Systems:
| System | IP Address | Role |
|--------------|----------------|------|
| pfSense | 192.168.56.1 | Firewall / Router |
| Wazuh SIEM | 192.168.56.104 | Monitoring |
| Nextcloud VM | 192.168.56.105 | Private Cloud |
| Kali Linux | 192.168.56.106 | Attacker |
| Windows 10 | 192.168.56.120 | Target |

---

##  Firewall Rules Implemented

Firewall rules were created under:

`Firewall → Rules → LAN`

### Block Rules:

| Rule | Protocol | Port | Description |
|------|----------|------|-------------|
| SMB | TCP | 445 | Block SMB lateral movement |
| SSH | TCP | 22 | Block SSH lateral movement |
| RDP | TCP | 3389 | Block RDP lateral movement |

### Configuration Details:

- Source: `Any`
- Destination: `Any`
- Rules placed **above default allow rule**
- Changes applied successfully

---

##  Testing After Hardening

### Commands executed from Kali Linux:

```bash
nmap -p 22 192.168.56.120
nmap -p 445 192.168.56.120
nmap -p 3389 192.168.56.120

## Results
Port  Service  State
22    SSH      Filtered
3389  RDP      Filtered
445   SMB      OPEN ❌
## Key Finding
Despite firewall rules being correctly configured, SMB traffic (port 445) remained accessible.
This indicates that:
Network traffic was not being routed through pfSense.

## Root Cause Analysis

The lab was operating in a flat network architecture using a VirtualBox Host-Only network.
Traffic Flow:

Kali → Windows (direct communication)
Instead of:

Kali → pfSense → Windows
Because of this:
pfSense firewall rules were bypassed
Systems communicated directly at Layer 2
No enforcement of security policies

## Security Implication
This configuration represents a common real-world issue:
Flat networks allow unrestricted lateral movement
Firewalls are ineffective if traffic does not pass through them
Attackers can bypass segmentation controls
## Lessons Learned
Firewall rules alone are not sufficient
Proper network architecture is critical
Traffic must be forced through security controls
Segmentation is required to prevent lateral movement

## Next Steps (Phase 2)

To properly enforce firewall rules:
Configure all systems to use pfSense as the default gateway (192.168.56.1)
Ensure traffic is routed through pfSense
Retest blocked ports (SMB, SSH, RDP)
Document "Before vs After" behavior

## Conclusion

This phase demonstrated that:
A firewall is only effective if it is placed in the actual traffic path.
This finding highlights the importance of proper network design in cybersecurity.
