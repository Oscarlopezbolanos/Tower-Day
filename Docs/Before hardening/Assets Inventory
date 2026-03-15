## Lab Environment Overview

This lab environment simulates a small-business private cloud infrastructure with monitoring and attack simulation capabilities. The following virtual machines are deployed within the 192.168.56.0/24 network managed by pfSense.

# Virtual Machine Inventory

# Nextcloud Server

Attribute	Value
VM Name	Cloud Clone
Role	Private Cloud Storage Server
IP Address	192.168.56.108/24
Operating System	Ubuntu Server (64-bit)
Memory	4000 MB
CPU	2 vCPUs

This server hosts the Nextcloud private cloud platform, which represents the small-business service being protected and monitored.

# Kali Linux 

Attribute	Value
VM Name	Kali
Role	Threat Actor / Attack Simulation
IP Address	192.168.56.106/24
Operating System	Kali Linux (Ubuntu-based)
Memory	3000 MB
CPU	2 vCPUs

Kali Linux is used to simulate attacker behavior such as:

reconnaissance scanning

service enumeration

vulnerability probing

These activities generate security telemetry captured by Wazuh.

# Wazuh SIEM Environment

Attribute	Value
VM Name	Ubuntu Server 1
Role	Security Information and Event Management (SIEM)
IP Address	192.168.56.107/24
Operating System	Ubuntu Server (64-bit)
Memory	8000 MB
CPU	4 vCPUs

The Wazuh platform provides:

centralized log monitoring

security event detection

attack telemetry collection

This system is used to observe attacker activity during the baseline and hardened phases of the project.

# pfSense Firewall

Attribute	Value
VM Name	pfSense
Role	Firewall / Router / DHCP Server
IP Address	192.168.56.1
Operating System	pfSense
Memory	2040 MB
CPU	2 vCPUs

pfSense provides:

network routing

DHCP services

firewall policy enforcement

network segmentation

This device forms the security boundary for the lab environment.

Network Summary
Network	Description
192.168.56.0/24	VirtualBox Host-Only Lab Network
Gateway	pfSense (192.168.56.1)
DHCP	pfSense DHCP Server
O
