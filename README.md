# Design and Security Hardening of a Hybrid Private Cloud Architecture
**Academic Research Project | Columbus State University Tower Day 2026** **Architects:** Oscar Lopez-Bolanos & Patrick Cassibry  

---

## 1. Executive Summary
This project demonstrates the engineering and validation of a **Zero Trust** hybrid cloud environment designed for small business resilience. By integrating a centralized **Wazuh SIEM (SOC)** with a **pfSense-managed network**, we have built an architecture that not only defends against modern threats but provides full forensic visibility into every stage of the cyber kill chain.

## 2. The Core Architecture
Our environment is built on three fundamental pillars of security engineering:
1. **Network Authority:** A pfSense gateway enforcing micro-segmentation and ICMP suppression.
2. **Endpoint Integrity:** Hardened Linux and Windows nodes utilizing FIM (File Integrity Monitoring).
3. **Identity Defense:** Multi-factor authentication (MFA) and SSH key enforcement to neutralize credential-based attacks.



---

## 3. Security Validation Index (Deep Dive)
To explore the technical standards and NIST-aligned incident reports, navigate to the specific domains below:

### **🛡️ Phase 1: Defensive Hardening**
*Standards for building a resilient baseline.*
* [**Windows SMB Hardening**](./Docs/Network-Hardening/Windows%20Hardening/Windows%20SMB%20Hardening/) - Engineering standards for secure file sharing.
* [**Linux System Hardening**](./Docs/Network-Hardening/Linux-Hardening/) - Hardening the "Cloud-Lab" management node.

### **⚔️ Phase 2: Vulnerability Validation**
*Simulated exploits and forensic detection reports.*
* [**SSH Brute-Force (Hydra)**](./Docs/Vulnerability-Assesment/Hydra/) - Detection of high-velocity identity attacks.
* [**SMB Exploitation**](./Docs/Vulnerability-Assesment/SMB-Attack/) - Validating the risks of misconfigured administrative shares.
* [**Lateral Movement (SSH Pivot)**](./Docs/Vulnerability-Assesment/SSH%20Pivot%20(Lateral%20Movement)/) - Monitoring internal network hopping.
* [**Network Discovery (Nmap)**](./Docs/Vulnerability-Assesment/nmap/) - Identifying reconnaissance signatures.

### **🧠 Phase 3: The SOC Operations**
*The "Brain" of the architecture.*
* [**Wazuh Manager Configuration**](./Docs/Wazuh-Manager/) - Custom decoders, rulesets, and active response logic.

---

## 4. Professional Impact
This project follows the **NIST SP 800-61 Rev. 2** incident response framework and utilizes the **MITRE ATT&CK** matrix to categorize threats. It serves as a proof-of-concept for how small-to-medium businesses can achieve enterprise-level security visibility using open-source tooling.

---

## 🛡️ Glossary of Architectural Principles
*To assist stakeholders across all technical levels, we have defined the core philosophies of this architecture:*

* **Zero Trust:** A security model that assumes the "perimeter" is already breached. It requires every user and device to be verified, even if they are already inside the network.
* **Defense-in-Depth:** A layered defense strategy. If the firewall fails, the host security catches it; if the host security fails, the MFA stops it.
* **The CIA Triad:** The fundamental goals of security: **Confidentiality** (keeping data secret), **Integrity** (ensuring data isn't tampered with), and **Availability** (ensuring systems stay running).
* **Cyber Kill Chain:** A framework for identifying the stages of a cyberattack. This project validates detections for Reconnaissance, Exploitation, and Lateral Movement.
* **Principle of Least Privilege (PoLP):** The practice of limiting access rights for users to the bare minimum permissions they need to perform their work.
* **Micro-segmentation:** Dividing a network into small, isolated zones to prevent an intruder from moving "sideways" through the environment.
* **SIEM (Security Information & Event Management):** The "Central Brain" (Wazuh) that collects and analyzes logs from every computer to find signs of an attack.
Why this is the "Final Touch":
