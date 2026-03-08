# Tower-Day
Design and Security Hardening of a Hybrid Private Cloud Architecture for Small Business Environments. This project designs and hardens a hybrid “private cloud” environment that delivers secure file sharing and administrative remote desktop while remaining feasible for small IT teams.
## Tower Day Lab Build Documentation
pfSense Network + Wazuh + Nextcloud + Kali Lab
Author Oscar Lopez-Bolanos
Columbus State University 
--
## Cybersecurity Nexus Program

## 1. Objectives

The objective of this lab is to build a segmented private cloud security environment using a firewall and SIEM monitoring to demonstrate enterprise security architecture for small business infrastructure.

The environment includes:

pfSense firewall and DHCP server

Wazuh SIEM monitoring platform

Nextcloud server (private cloud storage)

Kali Linux attacker machine

Windows 11 host management system

The lab network is isolated using VirtualBox Host-Only networking and managed through pfSense firewall rules and DHCP services.

## 2. Lab Architecture

Windows Host

                  (Management PC)
                         │
                         │
        VirtualBox Host-Only Network (192.168.56.0/24)
                         │
                     pfSense
                  192.168.56.1
                         │
       ┌─────────────────┼─────────────────┐
    Wazuh SIEM        Nextcloud VM        Kali Linux
    192.168.56.104    192.168.56.105      192.168.56.106

## 3. Infrastructure Components

Role
pfSense	Firewall, DHCP server
Wazuh	Security monitoring / SIEM
Nextcloud	Private cloud storage
Kali Linux	Attacker simulation
Windows Host	Management interface

## 4. Network Configuration

Network
192.168.56.0/24
pfSense
LAN IP: 192.168.56.1
DHCP Range: 192.168.56.100 – 192.168.56.200
DNS Server: 192.168.56.1

## 5. Major Problems Encountered

During the deployment several configuration issues prevented communication between systems.

Problem 1 — Windows Host Could Not Reach pfSense
Symptom

Attempting to access pfSense WebGUI:

https://192.168.56.1

Resulted in:

ERR_CONNECTION_REFUSED
Root Cause

Windows was not properly configured to communicate with the VirtualBox Host-Only network.

The VirtualBox Host-Only Adapter existed but did not have a valid IPv4 configuration.

Solution

Navigate to:

Control Panel
Network and Sharing Center
Change Adapter Settings

Open:

VirtualBox Host-Only Ethernet Adapter

Configure IPv4 manually:

IP Address: 192.168.56.5
Subnet Mask: 255.255.255.0
Gateway: (blank)

This allowed the Windows host to communicate with the pfSense LAN network.

Problem 2 — Ubuntu Servers Not Receiving DHCP
Symptom

Running:

ip a

showed static IP configuration:

192.168.56.10/24

Even after enabling DHCP on pfSense, the servers did not request new addresses.

Root Cause

Ubuntu Server was configured with a static network configuration using Netplan.

Example configuration:

dhcp4: no
addresses:
 - 192.168.56.10/24

This prevented the machine from requesting DHCP leases.

Important Discovery

Attempting to run:

dhclient

resulted in:

command not found

This revealed that Ubuntu Server 24.04 uses Netplan, not dhclient directly.

Solution — Netplan Configuration

Edit configuration file:

sudo nano /etc/netplan/50-cloud-init.yaml

Original configuration:

network:
 version: 2
 renderer: networkd
 ethernets:
  enp0s3:
   dhcp4: no
   addresses:
    - 192.168.56.10/24
   nameservers:
    addresses: [8.8.8.8]
Updated configuration
network:
 version: 2
 renderer: networkd
 ethernets:
  enp0s3:
   dhcp4: true

Apply configuration:

sudo netplan apply
Result

Machines successfully obtained DHCP addresses from pfSense.

Example:

Wazuh     192.168.56.104
Nextcloud 192.168.56.105
Kali      192.168.56.106

## 6. Final Working Environment

Machine	IP
pfSense	192.168.56.1
Wazuh	192.168.56.104
Nextcloud	192.168.56.105
Kali	192.168.56.106

## 7. Network Flow Diagram

Windows Management Host

                     │
                     │
       VirtualBox Host-Only Adapter
              192.168.56.5
                     │
                     │
                pfSense
              192.168.56.1
                     │
        ┌────────────┼─────────────┐
        │            │             │
     Wazuh        Nextcloud       Kali
    192.168.56.104 192.168.56.105 192.168.56.106
  
        │
        │
    Security Monitoring
  

## 8. Lessons Learned

1. Host Networking Must Be Verified First

Even if virtual machines are correctly configured, the host must have access to the host-only network.

2. Ubuntu Server Uses Netplan

Modern Ubuntu servers do not rely on traditional network tools like dhclient.

Network configuration is handled through Netplan YAML files.

3. DHCP Will Not Override Static Configuration

If a static IP exists in Netplan, DHCP will not function until the configuration is changed.

## 9. Next Steps

Future work will include:

Firewall segmentation rules in pfSense

Attack simulations from Kali Linux

Log analysis using Wazuh SIEM

Detection comparison before and after firewall hardening

## 10. Conclusion

This lab demonstrates the deployment of a segmented virtual infrastructure secured with pfSense and monitored using Wazuh SIEM. Through troubleshooting networking and configuration issues, the environment now provides a stable platform for cybersecurity attack simulation and detection research.

## Troubleshooting Timeline

1. pfSense web interface unreachable
2. Investigated host network adapter
3. Fixed VirtualBox host-only IPv4 configuration
4. Enabled pfSense DHCP
5. Ubuntu static IP prevented DHCP
6. Identified Netplan configuration
7. Modified YAML to enable DHCP
8. Applied configuration
9. Network successfully operational
