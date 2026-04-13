# Host Discovery Scan

To identify active systems within the baseline network, a host discovery scan was performed from the Kali Linux attacker machine.

Command used:

nmap -sn 192.168.56.0/24


| Host                 | IP             | Description             |
| -------------------- | -------------- | ----------------------- |
| pfSense              | 192.168.56.1   | Gateway / Firewall      |
| Windows Host Adapter | 192.168.56.5   | VirtualBox Host Adapter |
| Wazuh                | 192.168.56.107 | SIEM Server             |
| Nextcloud            | 192.168.56.108 | Private Cloud Server    |
| Kali                 | 192.168.56.106 | Attacker Machine        |

# Observations

All hosts on the network are visible and discoverable from the attacker machine, 
indicating that no segmentation or discovery restrictions exist in the baseline configuration.




nmap -sn 192.168.56.0/24                                                 Starting Nmap 7.95 ( https://nmap.org ) at 2026-03-14 22:13 EDT
Nmap scan report for pfSense.home.arpa (192.168.56.1)
Host is up (0.00084s latency).
MAC Address: 08:00:27:96:84:95 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Nmap scan report for 192.168.56.5
Host is up (0.00019s latency).
MAC Address: 0A:00:27:00:00:0A (Unknown)
Nmap scan report for 192.168.56.107
Host is up (0.00070s latency).
MAC Address: 08:00:27:BC:D6:2B (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Nmap scan report for 192.168.56.108
Host is up (0.00079s latency).
MAC Address: 08:00:27:03:D9:D9 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Nmap scan report for 192.168.56.106
Host is up.
Nmap done: 256 IP addresses (5 hosts up) scanned in 1.91 seconds
                                                        
