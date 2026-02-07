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
- Active Directory: Identity and access management using Kerberos
- Splunk: SIEM for log ingestion and security analytics
- VirtualBox: Virtualized lab environment and network configuration
- Kali Linux: Attacker simulation and adversary tooling
- Atomic Red Team: Attack simulation and telemetry generation

## Steps

### 1. Created and planned the overall structure of the project

<img width="581" height="749" alt="Active Directory Project" src="https://github.com/user-attachments/assets/9624db99-75f2-441e-8b33-f9a2fab278e9" />

### 2. Virtual machines were set up 
<img width="1221" height="779" alt="Virtual-Machines" src="https://github.com/user-attachments/assets/0c088cb9-c28b-424b-83eb-9fb9a904ba9a" />
Four virtual machines in total were set up. Windows 10 for the client, Kali for the attacker, Windows Server 2022 for the Active Directory, and Ubuntu Server 24.04.3 for Splunk.  
All of the machines were put on the same NAT Network, to keep the traffic between them private. The Network IPv4 was set to 192.168.10.0/24.

### 3. Splunk Server was set up
<img width="1920" height="1080" alt="Splunk-Fixed-IP" src="https://github.com/user-attachments/assets/eb970c67-c2c5-458b-9225-82c8e7e905f3" />
The static IP was set to 192.168.10.10/24 in the 50-cloud-init.yaml file.  
Splunk was installed on the VM by setting up a shared folder from the VM to my own host machine and mounting it to the Ubuntu instance. I downloaded the Splunk installer on my host and put it in the shared folder, which allowed me to install onto the VM.

### 4. Target Machine and ADDC Server were set up to forward logs to the Splunk server

<img width="1021" height="775" alt="FIxed-IP-Target" src="https://github.com/user-attachments/assets/18f99fc4-53ee-4861-aa58-53e64564180b" />

The target machine was also given a fixed IP, and preemptively set up the DNS server to point towards the ADDC server's address: 192.168.10.7 

Next I installed the Splunk Universal Forwarder onto the target machine and set the receiving indexer IP to be 192.168.10.10:9997, which is our Splunk server's IP. I set up Sysmon with the Olaf config on the machine as well. 

<img width="1027" height="769" alt="inputs conf file" src="https://github.com/user-attachments/assets/a4331853-bbc7-4333-b281-20a161504d99" />

Next the inputs.conf file was created in the SplunkUniversalForwarder folder, to let the forwarder know what logs to send over to the Splunk server. Here we named the index to be endpoint, so on the Splunk dashboard, they will show up where index="endpoint"    
The SplunkUniversalForwarder service was restarted to update the configuration and changed to a local service.   

<img width="1021" height="766" alt="ADDC-IP" src="https://github.com/user-attachments/assets/cccac673-4523-4af8-8b53-1ad262d56dbb" />

The ADDC server was set up with the Splunk Universal Forwarder in the same fashion, the only difference being its fixed IP set to 192.168.10.7 and DNS set to 8.8.8.8


<img width="1021" height="773" alt="splunk-index-added" src="https://github.com/user-attachments/assets/6d06887e-c28e-496e-a712-e7a723825ec6" />
<img width="1023" height="770" alt="splunk-receiving" src="https://github.com/user-attachments/assets/3e8fd0af-fc42-486d-bcad-ecdfea9c0c7b" />

The Splunk server itself was then configured to receive the data and have the index named "endpoint".

### 5. Active Directory Domain Services was set up on the Server Manager. The target machine was registered as a member of the new domain.

<img width="1021" height="769" alt="ADDS" src="https://github.com/user-attachments/assets/e5dc8899-4518-4253-9e7b-c7a1de7873f1" />

ADDS was set up using the Server Manager and a new forest and root domain name was created: apmaud.local. Then two new groups and users were created as an example.

<img width="1026" height="768" alt="domain-registration" src="https://github.com/user-attachments/assets/83ac9576-e4ad-4d71-9af0-46a413bb5669" />

The target machine was registered to this newly created domain. The target machine was able to "find" the ADDC server because its DNS settings had pointed to 192.168.10.7 from before.
