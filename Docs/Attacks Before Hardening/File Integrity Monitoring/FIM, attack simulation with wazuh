## File Integrity Monitoring (FIM) Attack Simulation with Wazuh

# Objective

This lab demonstrates how Wazuh HIDS detects file integrity violations by simulating malicious file activity on a monitored Linux endpoint. The goal is to validate detection of file creation, modification, and deletion events using checksum-based monitoring.

# Lab Environment

SIEM: Wazuh Manager
Agent: Kali Linux (cloud-lab)
Monitoring Module: Syscheck (FIM)
Network: VirtualBox Host-Only (192.168.56.0/24)

# Configuration
FIM was enabled in the agent configuration:
XML
<syscheck>
  <disabled>no</disabled>
  <frequency>60</frequency>
  <scan_on_start>yes</scan_on_start>

  <directories check_all="yes">/etc,/usr/bin,/usr/sbin</directories>
  <directories check_all="yes">/bin,/sbin,/boot</directories>
  <directories check_all="yes">/tmp</directories>
</syscheck>

 #
 
 Attack Simulation Steps

1. Create baseline file
Bash
touch /tmp/fim_test.txt
⏳ Wait ~2 minutes for baseline scan
2. Modify file (simulate tampering)
Bash
echo "malicious change" >> /tmp/fim_test.txt
3. Modify again
Bash
echo "malicious change 2" >> /tmp/fim_test.txt
4. Delete file
Bash
rm /tmp/fim_test.txt

# Detection Results (Wazuh Dashboard)

Wazuh successfully detected:
-- Integrity checksum changed
-- File deleted
-- Rootcheck activity

# Analysis

Wazuh FIM works by:
Creating a baseline (initial file hash)
Comparing future scans against this baseline
Generating alerts when differences are detected

#Challenges & Troubleshooting

Issue
Resolution
No alerts initially
FIM requires baseline before detection
Changes not detected
Needed to wait between scans
Timing issues
Slower attack simulation fixed detection

# Key Takeaways

FIM is not real-time (scan-based detection)
Baseline creation is critical
Timing affects detection accuracy
Wazuh effectively detects file tampering attacks
# MITRE ATT&CK Mapping
T1565.001 – Data Manipulation
T1070.004 – Indicator Removal (File Deletion)
 
# Next Steps

Expand monitoring to critical system files
Tune alert thresholds
Integrate with Security Onion (NDR)
Simulate additional attack scenarios (SSH brute force, privilege escalation)
