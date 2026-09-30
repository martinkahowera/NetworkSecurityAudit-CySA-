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

<h2>Program walk-through:</h2>

<p align="center">

<img src="https://imgur.com/a/HCq10lC.png" width="80%" alt="Disk Sanitization Steps" />
<br />### Task 1.1: SRV01 Network & Hostname Setup

**What I Did**
--I configured the static IP parameters and hostname on the newly installed Windows Server 2022 instance. As shown in the terminal output above, I set the host name to SRV01, assigned a static IPv4 address of 172.31.20.10 with a subnet mask of 255.255.255.0, and configured the preferred DNS servers to point to 172.31.20.10 (and loopback 127.0.0.1).

**How I Did It**
1. Renamed the computer to SRV01 through System Properties and rebooted the server to apply the change.
2. Opened Network Connections, accessed the properties for Ethernet0, and configured Internet Protocol Version 4 (TCP/IPv4).
3. Switched from DHCP to manual static configuration, entering the required IP address, subnet mask, and DNS server details.
4. Opened Command Prompt as Administrator and ran `ipconfig /all` followed by `hostname` to verify all parameters were applied properly.

**Why I Did It**
A server acting as a core infrastructure node—especially a Domain Controller—cannot use dynamic (DHCP) IP addressing because its IP must remain fixed so other machines on the network can reliably find it. Setting the local IP address as the preferred DNS server is a prerequisite before promoting the machine to a Domain Controller, as Active Directory relies heavily on DNS for domain resolution, authentication, and service location across the network.
<br />
Select the disk:  <br/>
<img src="https://i.imgur.com/tcTyMUE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Enter the number of passes: <br/>
<img src="https://i.imgur.com/nCIbXbg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Confirm your selection:  <br/>
<img src="https://i.imgur.com/cdFHBiU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Wait for process to complete (may take some time):  <br/>
<img src="https://i.imgur.com/JL945Ga.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
