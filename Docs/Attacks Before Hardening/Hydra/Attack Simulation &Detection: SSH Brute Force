## Attack Simulation & Detection: SSH Brute Force (Hydra)
 
# Objective
Simulate a real-world brute force attack from an attacker machine (Kali Linux) and validate detection capabilities using the Wazuh SIEM platform.

# Lab Environment
Role     SystemIP                     Address
SIEM      Wazuh Manager (Ubuntu)      192.168.56.104
Target    Cloud Lab (Ubuntu)          192.168.56.108
Attacker  Kali Linux                   192.168.56.106

#Attack Simulation
 
A brute-force SSH attack was launched from the Kali machine using Hydra.
Step 1 — Prepare Wordlist

Due to missing default wordlists in Kali, the rockyou.txt file was manually downloaded:
Bash
wget https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt

Step 2 — Execute Attack

Bash
hydra -l root -P rockyou.txt -t 4 ssh://192.168.56.108
Attack Behavior Observed
Continuous login attempts against SSH (port 22)
High volume of failed authentication requests
Parallel brute-force attempts
Detection in Wazuh
Wazuh successfully detected the attack and generated multiple alerts:
Key Log Events:
sshd: authentication failed
PAM: Multiple failed logins in a small period of time
Maximum authentication attempts exceeded
User authentication failure

# Detection Summary
Event Type                        Description
SSH Authentication Failures       Multiple failed login attempts
PAM Alerts                        Rapid login attempts detected
Brute Force Indicators            Repeated access attempts over short time

# Key Takeaways
-- Detection depends on actual generated traffic, not tool execution alone
-- Missing dependencies (wordlists) can prevent attack simulation
-- Wazuh effectively correlates authentication failures into meaningful alerts
-- This scenario simulates a real-world brute force attack (MITRE ATT&CK T1110)

# Troubleshooting Lessons
-- Hydra failed initially due to missing rockyou.txt
-- Kali package wordlists did not include the expected file
-- Manual download resolved the issue
-- Wazuh agent connectivity and logging confirmed before testing

# Future Improvements
-- Implement alert severity tuning for brute force detection
-- Add active response (e.g., IP blocking)
-- Integrate pfSense logs for network-level visibility
-- Deploy Security Onion for network detection comparison
