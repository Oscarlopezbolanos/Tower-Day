#  Network Segmentation Implementation (pfSense)

##  Overview
This phase focuses on implementing network segmentation using pfSense to prevent lateral movement between systems in a hybrid private cloud lab environment.

Previously, all systems operated in a flat network, allowing unrestricted communication between hosts. This made lateral movement attacks (such as SMB exploitation and SSH pivoting) trivial.

To address this, a segmented architecture was designed and enforced using pfSense as a central firewall.

---

##  Architecture Before Segmentation (Flat Network)

- All hosts in same subnet: `192.168.56.0/24`
- Direct communication between:
  - Kali (attacker)
  - Windows (target)
  - Ubuntu (Nextcloud server)
- No traffic inspection or filtering

###  Result:
- SMB (port 445) accessible
- SSH pivoting possible
- Lateral movement successful

---

##  Architecture After Segmentation

| Network | Purpose |
|--------|--------|
| 192.168.56.0/24 | Internal / User Network |
| 192.168.20.0/24 | Segmented Attack Network |

- Kali moved to separate subnet: `192.168.20.10`
- pfSense acts as gateway: `192.168.20.1`
- All traffic forced through pfSense

---

##  Configuration Steps

### 1. VirtualBox Network Setup
- Added second Host-Only adapter:
  - Network: `192.168.20.0/24`

### 2. pfSense Interface Assignment
- Assigned new interface for segmented network
- Configured IP:
192.168.20.1/24

### 3. Kali Network Configuration

sudo ip addr flush dev eth0
sudo ip addr add 192.168.20.10/24 dev eth0
sudo ip route add default via 192.168.20.1
4. Connectivity Validation
Bash
ping 192.168.20.1

### Successful communication with pfSense

🔍 Validation Testing
Test: SMB Access from Kali → Windows
Bash
nmap -p 445 192.168.56.120 -Pn

### Result:

Host not reachable OR port filtered

### Security Impact

Control
Before
After
Network Visibility
None
Full (pfSense)
Lateral Movement
Allowed
Restricted
Traffic Filtering
No
Yes
Attack Surface
High
Reduced

### Key Takeaways

Network segmentation is critical for preventing lateral movement
Firewall rules are ineffective without proper architecture
Forcing traffic through a controlled gateway enables detection and enforcement
Even simple segmentation significantly improves security posture

### Evidence

Kali IP configuration
pfSense interface setup
Successful ping to gateway
Nmap scan showing filtered/blocked access
