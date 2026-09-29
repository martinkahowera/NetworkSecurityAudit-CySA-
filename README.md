<h1> NetworkSecurityAudit-CySA-</h1>

<h2>Description</h2>
This project provides an end-to-end network security audit and threat monitoring solution built across an enterprise Active Directory environment. The lab combines proactive vulnerability assessments using Tenable Nessus with defensive Group Policy Object (GPO) hardening to eliminate system misconfigurations. It also features offline password hash auditing using Hashcat to identify weak domain credentials and test access controls before an actual adversary can exploit them.

To defend against active network threats, the environment incorporates a custom-configured Snort Intrusion Detection System (IDS) paired with Wireshark packet analysis. The setup actively monitors network traffic, detects unauthorized ICMP reconnaissance, and flags stealthy Nmap TCP FIN scans in real time. Designed around CompTIA CySA+ objectives, the project bridges host-level system administration with network-level threat hunting and incident triage.
<br />


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
Launch the utility: <br/>
<img src="https://i.imgur.com/62TgaWL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
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
