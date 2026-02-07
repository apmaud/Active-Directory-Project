# Active-Directory-Project

## Objective

The Active Directory Lab Project was designed to build a controlled Active Directory environment for simulating user management activities and collecting security logs related to account-based incidents and threats. The lab environment was deployed using multiple virtual machines within VirtualBox, all connected through a shared NAT network.  

Splunk was implemented as the Security Information and Event Management (SIEM) platform to ingest and analyze logs from the Active Directory Domain Controller and a client workstation. To generate realistic security telemetry, an attacker machine running Kali Linux was introduced, and Atomic Red Team was executed on the client system to simulate adversary behaviors.  

This project provided hands-on experience with Active Directory and SIEM-based monitoring, demonstrating how authentication events, system logs, and attack techniques can be correlated and analyzed to identify and investigate potential threats.  

### Skills Learned
- Advanced understanding of Active Directory architecture, user management, and domain-based authentication workflows
- Proficiency in configuring Splunk as a SIEM to ingest, normalize, and analyze logs from Active Directory domain controllers and client endpoints
- Ability to generate and analyze security telemetry using adversary simulation tools such as Kali Linux and Atomic Red Team
- Enhanced knowledge of virtualized lab environments, including multi-machine network configuration within VirtualBox
- Strengthened threat detection and analytical skills through correlation of authentication events, attack behaviors, and system logs


### Tools Used
- Active Directory: Identity and access management
- Splunk: SIEM for log ingestion and security analytics
- VirtualBox: Virtualized lab environment and network configuration
- Kali Linux: Attacker simulation and adversary tooling
- Atomic Red Team: Attack simulation and telemetry generation

## Steps

### 1. Created and planned the overall structure of the project
