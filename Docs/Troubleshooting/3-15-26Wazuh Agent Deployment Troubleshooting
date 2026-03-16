## Lab Notes

#Lab Environment

Component	          IPAddress	        Role
Wazuh Manager	 192.168.56.107	      SIEM / Log Analysis
Kali Linux	   192.168.56.106	      Attacker Machine
Windows 10	   192.168.56.20	      Victim Endpoint
Ubuntu Server	 192.168.56.108	      Nextcloud / Target Server

The objective was to deploy a Wazuh agent on the Ubuntu cloud-lab server to allow centralized monitoring and log collection from the host.

#Issue 1 — Network Misconfiguration
Problem

The VM was initially connected to a NAT network instead of the host-only lab network.

Because of this, the Wazuh agent could not communicate with the Wazuh manager.

Solution

The VM network adapter was switched to the host-only lab network (192.168.56.0/24) so the agent could reach the SIEM.

#Issue 2 — Agent Version Incompatibility
Problem

The default installation pulled the newest Wazuh agent version:

4.14.x

However the Wazuh manager in the lab environment was running:

4.7.x

Wazuh requires:

Agent version ≤ Manager version

Attempting to register the agent produced the error:

ERROR: Agent version must be lower or equal to manager version
Solution

The incompatible agent version was removed:

sudo apt remove wazuh-agent

A compatible agent version (4.7.3) was downloaded manually.

#Issue 3 — Incorrect Package Download URL
Problem

The initial curl command downloaded only 111 bytes instead of the expected ~13 MB package.

This occurred because the repository URL path was incorrect.

Incorrect path example:

.../pool/main/w/wazuh-agent_4.7.3...

Correct path required an additional directory.

Solution

The correct download command was used:

curl -LO https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.7.3-1_amd64.deb

Verification:

ls -lh wazuh-agent_4.7.3-1_amd64.deb

Expected size:

~13 MB

#Issue 4 — Agent Registration

After installing the compatible agent version, the agent needed to authenticate with the manager.

Command used:

sudo /var/ossec/bin/agent-auth -m 192.168.56.107

Successful output:

INFO: Valid key received

This confirms the agent is authorized to communicate with the manager.

#Issue 5 — Agent Configuration Error
Problem

The Wazuh service failed to start with the error:

Invalid server address found: 'MANAGER_IP'
No client configured. Exiting.

This indicated that the configuration file still contained the placeholder:

MANAGER_IP
Solution

The configuration file was edited:

sudo nano /var/ossec/etc/ossec.conf

The server address was corrected:

<address>192.168.56.107</address>
Agent Startup

After correcting the configuration:

sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent

Verification:

sudo systemctl status wazuh-agent

Expected output:

Active: active (running)
Final Result

The Ubuntu cloud-lab server successfully registered with the Wazuh SIEM.

The Wazuh dashboard now shows:

Agent	Status
Kali Linux	Active
Windows Guest	Disconnected
Cloud Lab Server	Active

The SIEM is now capable of collecting logs and monitoring activity from the lab server.

#Lessons Learned

This troubleshooting process highlighted several important operational considerations when deploying SIEM agents:

Ensure agent and manager versions are compatible

Verify correct repository paths when downloading packages

Confirm network segmentation allows communication between agents and manager

Validate configuration files after reinstalling agents

