#  NIST Incident Report — Network Segmentation Implementation

## 1. Incident Summary

During testing of a flat network architecture, unrestricted lateral movement was observed between hosts. The attacker (Kali Linux) successfully accessed services on internal systems, including SMB and SSH.

This lack of segmentation created a high-risk environment susceptible to internal compromise and ransomware propagation.

---

## 2. Detection & Analysis

### Observed Behavior:
- Direct connectivity between all hosts
- Open SMB port (445) on Windows
- Successful SSH pivoting

### Root Cause:
- Flat network architecture
- No enforced traffic routing through firewall
- Lack of segmentation controls

---

## 3. Containment Actions

To mitigate lateral movement risks:

- Implemented network segmentation using pfSense
- Created isolated subnet for attacker simulation:
192.168.20.0/24

- Forced all traffic through pfSense gateway
- Configured routing and interface assignments

---

## 4. Eradication & Recovery

- Removed direct host-to-host communication
- Enforced firewall-based inspection
- Validated segmentation via connectivity testing and port scanning

---

## 5. Post-Implementation Validation

### Test Performed:

nmap -p 445 192.168.56.120 -Pn
Result:
Host unreachable or port filtered
Interpretation:
Lateral movement blocked
Firewall successfully enforcing segmentation

### 6. Lessons Learned

Flat networks significantly increase attack surface
Segmentation must be enforced at the network level, not just endpoints
Proper routing is critical for firewall effectiveness
Detection and prevention improve when traffic is centralized

### 7. Recommendations

Implement segmentation in all small business environments
Use firewall gateways for traffic inspection
Combine segmentation with SIEM monitoring (e.g., Wazuh)
Regularly test for lateral movement exposure
