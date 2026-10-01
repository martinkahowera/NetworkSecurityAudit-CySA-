# Enterprise Network Security Audit, Active Directory Hardening & IDS Deployment

This project provides an end-to-end network security audit and threat monitoring solution built across an enterprise Active Directory environment. Divided into 5 distinct assessment sections, the lab combines proactive vulnerability assessments using Tenable Nessus with defensive Group Policy Object (GPO) hardening to eliminate system misconfigurations. It also features offline password hash auditing using Hashcat to identify weak domain credentials and test access controls before an actual adversary can exploit them.

To defend against active network threats, the environment incorporates a custom-configured Snort Intrusion Detection System (IDS) paired with Wireshark packet analysis. Across all 5 phases, the setup actively monitors network traffic, detects unauthorized ICMP reconnaissance, and flags stealthy Nmap TCP FIN scans in real time. Designed around CompTIA CySA+ objectives, the project bridges host-level system administration with network-level threat hunting and incident triage.stration with network-level threat hunting and incident triage.
<br />
## 📋 Engagement Context & Scenario

As an IT Security Auditor assigned to **Metdata**, the primary objective is to evaluate, harden, and verify the organization's security architecture. The Senior Security Compliance Officer directed an audit of network security postures, script-based hardening controls, service optimization at boot, and vulnerability remediation strategies.

To prevent disruption to production infrastructure, all security enhancements, vulnerability scans, and intrusion detection deployments are engineered, tested, and validated within a dedicated virtualized staging environment mirroring the enterprise production network.

<h2>Languages and Utilities Used</h2>



### Security & Audit Utilities
- Tenable Nessus Essential (Vulnerability Scanner)
- Snort IDS (Intrusion Detection System & Custom Rules Engine)
- Wireshark (Network Packet Analyzer)
- Nmap (Network Mapper & Port Scanner)
- Hashcat (Password Hash Cracking & Audit Tool)

### System Administration & Management
- Active Directory Domain Services (AD DS)
- Group Policy Management Console (GPMC)
- Windows Defender Firewall with Advanced Security

<h2>Environments Used </h2>

### Operating Systems & Environments
- Windows Server 2022 Standard (Domain Controller / SRV01)
- Kali Linux (Attacker / Security Audit Workstation)
- VMware Workstation / Oracle VirtualBox (Virtualization Platform)
- 
## Task 1: System Configurations

Before diving into packet analysis and security testing, I needed to set up the lab environment. In this section, I deployed two virtual machines—`SRV01` (Windows Server 2022) and `Kali02` (Kali Linux)—configured their static IP addresses on the same subnet, promoted `SRV01` to a Domain Controller for `metdata.com`, and verified network connectivity between both hosts.

<h2>Task walk-through:</h2>

<p align="center">

![image alt](https://github.com/martinkahowera/NetworkSecurityAudit-CySA-/blob/main/Screenshot%202026-09-30%20025321.png?raw=true)
<br />
### Task 1.1: SRV01 Network & Hostname Setup

**What I Did:**
I configured the static IP parameters and hostname on the newly installed Windows Server 2022 instance. As shown in the terminal output above, I set the host name to SRV01, assigned a static IPv4 address of 172.31.20.10 with a subnet mask of 255.255.255.0, and configured the preferred DNS servers to point to 172.31.20.10 (and loopback 127.0.0.1).

**How I Did It:**
1. Renamed the computer to SRV01 through System Properties and rebooted the server to apply the change.
2. Opened Network Connections, accessed the properties for Ethernet0, and configured Internet Protocol Version 4 (TCP/IPv4).
3. Switched from DHCP to manual static configuration, entering the required IP address, subnet mask, and DNS server details.
4. Opened Command Prompt as Administrator and ran `ipconfig /all` followed by `hostname` to verify all parameters were applied properly.

**Why I Did It:**
A server acting as a core infrastructure node—especially a Domain Controller—cannot use dynamic (DHCP) IP addressing because its IP must remain fixed so other machines on the network can reliably find it. Setting the local IP address as the preferred DNS server is a prerequisite before promoting the machine to a Domain Controller, as Active Directory relies heavily on DNS for domain resolution, authentication, and service location across the network.
<br />

![image_alt](https://github.com/martinkahowera/NetworkSecurityAudit-CySA-/blob/main/Screenshot%202026-09-30%20040350.png?raw=true)
### Task 1.2: Active Directory Domain Controller Promotion (`metdata.com`)

**What I Did:**
I installed Active Directory Domain Services (AD DS) on SRV01 and promoted the server to the Primary Domain Controller for a new forest named `metdata.com`. As demonstrated in the `systeminfo` output above, the system's OS Configuration reflects **Primary Domain Controller**, assigned to the domain **metdata.com**, with **\\SRV01** acting as the logon server.

**How I Did It:**
1. Opened Server Manager, navigated to **Add Roles and Features**, and selected the **Active Directory Domain Services (AD DS)** role along with its management tools.
2. Completed the wizard installation and launched the **Active Directory Domain Services Configuration Wizard** via the Server Manager notification banner.
3. Selected **Add a new forest**, configured the Root domain name as `metdata.com`, set the DSRM password, and kept standard forest/domain functional levels.
4. Executed the prerequisite checks and initiated the installation, allowing the system to automatically restart to complete the promotion.
5. Ran `systeminfo` in Command Prompt post-reboot to verify domain role assignment, domain naming, and logon server details.

**Why I Did It:**
Establishing an Active Directory Domain Controller provides centralized identity, access management, and policy enforcement (GPOs) across the entire `metdata.com` enterprise. Promoting `SRV01` as the root domain controller forms the foundational directory infrastructure required to manage domain accounts, enforce security controls, and audit access across endpoints and services in subsequent phases of this lab.

![image_alt](https://github.com/martinkahowera/NetworkSecurityAudit-CySA-/blob/main/Screenshot%202026-09-30%20041825.png?raw=true)
### Task 1.3: Windows Defender Firewall — File and Printer Sharing Rule

**What I Did:**
I updated the Windows Defender Firewall rules on SRV01 to permit File and Printer Sharing traffic across all network location profiles (Domain, Private, and Public). As shown in the screenshot, the primary service entry and all three profile checkboxes are explicitly enabled.

**How I Did It:**
1. Opened Control Panel on SRV01 and navigated to **System and Security** > **Windows Defender Firewall**.
2. Clicked **Allow an app or feature through Windows Defender Firewall** on the left navigation pane.
3. Located **File and Printer Sharing** in the list of allowed applications.
4. Enabled the main service check box alongside the **Domain**, **Private**, and **Public** profile check boxes.
5. Clicked **OK** to commit the firewall rule changes to the system.

**Why I Did It:**
File and Printer Sharing uses SMB (Server Message Block) protocols (ports 139 and 445) and NetBIOS/RPC services required for remote management, file distribution, and network service enumeration. Opening this service across all profiles ensures that administrative and audit traffic from security workstations (such as Kali02) can reach required host interfaces during testing and compliance scans.

![image_alt](https://github.com/martinkahowera/NetworkSecurityAudit-CySA-/blob/main/Screenshot%202026-10-01%20014509.png?raw=true)

### Task 1.4: Kali02 Network Configuration & Connectivity Verification
**What I Did**
I set up the static IP address on my Kali Linux machine (Kali02) so it's on the same subnet (172.31.20.0/24) as my server. Then I tested the connection to make sure Kali02 can talk to SRV01 (172.31.20.10) properly.

**How I Did It**

1. Opened the terminal on Kali02.
2. Ran sudo ip addr add 172.31.20.30/24 dev eth0 to set my static IP and brought the link up.
3. Ran ping -c 4 172.31.20.10 to send 4 test packets to SRV01.
4. Ran ip a to double check that 172.31.20.30 was actually bound to my eth0 interface.

**Why I Did It**
I need Kali02 on a static IP address in the network so it stays consistent when I start running vulnerability scans and network tests. Ping testing confirms that the virtual network is connected and that SRV01 isn't blocking basic connection attempts from Kali02.

## End of Task 1: Environment Setup Complete

All the core setup for the lab is finished and verified:
- **SRV01** has a static IP (`172.31.20.10`), is promoted as the Domain Controller for `metdata.com`, and has Windows Firewall configured to allow File and Printer Sharing.
- **Kali02** is set up with a static IP (`172.31.20.30`) and can successfully ping `SRV01`.

---

### Task 2: Reconnaissance & Wireshark Packet Analysis

In this section, I'll be looking at how attackers gather information on a network during the reconnaissance phase. I'll start Wireshark on `SRV01` to capture live traffic, then head over to `Kali02` to run Nmap ping and full port scans against the network and server. After the scans, I'll analyze the captured packets in Wireshark and apply specific filters to isolate TCP Push and FIN flags coming from the Kali machine.
![image_alt](https://github.com/martinkahowera/NetworkSecurityAudit-CySA-/blob/main/Screenshot%202026-10-02%20014321.png?raw=true)
