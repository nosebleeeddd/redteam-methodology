# Red Team Operations Guide

A practical reference for understanding red team operations, adversary simulation, penetration testing methodologies, and offensive security tools.

This guide follows the MITRE ATT&CK framework and covers the major phases of an authorized red team engagement.

## Table of Contents

1. [Introduction](#introduction)
2. [Red Team Methodology](#red-team-methodology)
3. [Planning and Intelligence](#planning-and-intelligence)
4. [Reconnaissance](#reconnaissance)
5. [Weaponization](#weaponization)
6. [Delivery](#delivery)
7. [Exploitation](#exploitation)
8. [Installation and Persistence](#installation-and-persistence)
9. [Command and Control](#command-and-control)
10. [Actions on Objectives](#actions-on-objectives)
11. [Lateral Movement](#lateral-movement)
12. [Data Collection and Exfiltration](#data-collection-and-exfiltration)
13. [Covering Tracks and Detection](#covering-tracks-and-detection)
14. [Red Team Tools Arsenal](#red-team-tools-arsenal)
15. [Operational Security](#operational-security)
16. [Legal and Ethical Guidelines](#legal-and-ethical-guidelines)
17. [Resources](#resources)

---

## Introduction

Red Team Operations involve simulating adversarial behavior to evaluate an organization's security posture.

Unlike traditional vulnerability assessments, red team engagements evaluate how effectively an organization can prevent, detect, investigate, and respond to realistic attack scenarios.

### Key Objectives

- **Threat Emulation:** Simulate documented adversary techniques.
- **Detection Testing:** Evaluate SIEM, EDR, IDS, and monitoring capabilities.
- **Incident Response:** Assess security team response procedures.
- **Security Assessment:** Identify weaknesses across infrastructure and applications.
- **Reporting:** Provide actionable findings and remediation recommendations.

## Red Team Methodology

### 1. Cyber Kill Chain

The Cyber Kill Chain provides a high-level model for understanding the stages of an attack.

```text
Reconnaissance
      |
Weaponization
      |
Delivery
      |
Exploitation
      |
Installation
      |
Command & Control
      |
Actions on Objectives
```

### 2. MITRE ATT&CK Framework

MITRE ATT&CK categorizes adversary behavior into tactics and techniques.

| Tactic | Purpose |
|---|---|
| Initial Access | Obtain an initial foothold |
| Execution | Execute code or commands |
| Persistence | Maintain access |
| Privilege Escalation | Obtain additional permissions |
| Defense Evasion | Bypass or interfere with security controls |
| Credential Access | Attempt to access authentication material |
| Discovery | Identify systems and resources |
| Lateral Movement | Access other systems |
| Collection | Gather relevant information |
| Command and Control | Communicate with external infrastructure |
| Exfiltration | Transfer information outside an environment |
| Impact | Affect system availability or integrity |

Reference: https://attack.mitre.org/

## Planning and Intelligence

### Rules of Engagement (ROE)

Every engagement should have documented authorization and clearly defined boundaries.

Important components:

- Authorized IP addresses and domains.
- Approved testing methods.
- Testing schedule.
- Excluded systems.
- Data handling requirements.
- Emergency contacts.
- Reporting procedures.
- Incident escalation procedures.

### Target Profiling

Develop an understanding of the environment before testing.

Areas to investigate:

- Organization structure.
- Operating systems.
- Network infrastructure.
- Cloud environments.
- Public-facing applications.
- Authentication systems.
- Security monitoring.
- Business-critical services.

### Threat Intelligence

Threat intelligence helps identify adversary behaviors relevant to an organization.

Important concepts:

- APT groups.
- Tactics, Techniques, and Procedures (TTPs).
- Indicators of Compromise (IOCs).
- Malware families.
- Attack campaigns.
- Vulnerability intelligence.

## Reconnaissance

Reconnaissance involves gathering information about an authorized assessment target.

### Passive Reconnaissance

Collect information from publicly available sources without directly interacting with target infrastructure.

#### OSINT Tools

| Tool | Purpose |
|---|---|
| theHarvester | Public email and domain intelligence |
| Amass | Attack surface and domain enumeration |
| Subfinder | Subdomain discovery |
| Sherlock | Public username searches |
| Recon-ng | OSINT framework |
| Maltego | Relationship and infrastructure mapping |
| Shodan | Internet-connected device intelligence |
| Censys | Internet asset discovery |

#### Example Commands

```bash
# Domain enumeration
amass enum -d example.com

# Subdomain discovery
subfinder -d example.com

# Public email intelligence
theHarvester -d example.com -b all
```

Only investigate domains you own or are authorized to assess.

### Active Reconnaissance

Active reconnaissance involves directly interacting with authorized infrastructure.

#### Network Discovery

```bash
# Discover hosts on a lab network
nmap -sn 192.168.1.0/24

# Identify open ports on a lab machine
nmap -sV 192.168.1.100

# Identify common services
nmap -sC -sV 192.168.1.100
```

#### Web Application Enumeration

Tools:

- Gobuster
- FFUF
- Burp Suite
- OWASP ZAP

Example:

```bash
gobuster dir \
-u http://192.168.1.100 \
-w /usr/share/wordlists/dirb/common.txt
```

## Weaponization

Weaponization is the preparation of testing artifacts used during an authorized assessment.

### Payload Analysis

Understand the characteristics of executable testing artifacts:

- File format.
- Architecture.
- Dependencies.
- Execution permissions.
- Network behavior.
- Persistence behavior.
- Detection opportunities.

### Metasploit Framework

Metasploit is a penetration testing framework used for vulnerability validation and controlled exploitation.

Start Metasploit:

```bash
msfconsole
```

Search available modules:

```text
search type:exploit
```

Inspect a module:

```text
info <module_name>
```

View module options:

```text
show options
```

### Custom Testing Artifacts

Common areas of research:

- PowerShell scripting.
- Python tooling.
- C and C++ utilities.
- Windows API behavior.
- Linux process execution.
- Malware analysis.
- Static and dynamic analysis.

## Delivery

Delivery describes how a testing artifact or attack simulation reaches its intended environment.

### Email Security Testing

Areas to assess:

- SPF configuration.
- DKIM configuration.
- DMARC policies.
- Email filtering.
- Attachment scanning.
- URL reputation checks.
- User reporting procedures.

### Phishing Simulation

GoPhish is commonly used for authorized phishing awareness campaigns.

Important considerations:

- Obtain written authorization.
- Use approved test recipients.
- Avoid collecting real passwords.
- Use controlled landing pages.
- Record campaign metrics.
- Establish a clear stop procedure.

### Physical Security Assessment

Potential assessment areas:

- Physical access controls.
- Visitor management.
- USB device policies.
- Workstation security.
- Network port restrictions.
- Badge access systems.

## Exploitation

Exploitation involves validating whether an identified vulnerability can affect a system.

### Web Application Vulnerabilities

Common vulnerability classes:

| Vulnerability | Description |
|---|---|
| SQL Injection | Improper handling of database queries |
| XSS | Injection of client-side scripts |
| Command Injection | Unsafe operating system command execution |
| SSRF | Server-side requests to unintended resources |
| IDOR | Unauthorized access through object references |
| File Upload | Improper validation of uploaded files |
| Authentication Bypass | Weaknesses in authentication mechanisms |

### Testing Tools

- Burp Suite.
- OWASP ZAP.
- SQLMap.
- Nmap.
- Nikto.
- Metasploit.

### Network Service Assessment

Common services to investigate:

| Service | Port | Assessment Focus |
|---|---:|---|
| SSH | 22 | Authentication and configuration |
| FTP | 21 | Anonymous access and encryption |
| SMB | 445 | Shares and permissions |
| HTTP | 80 | Web application security |
| HTTPS | 443 | TLS and application security |
| RDP | 3389 | Authentication and exposure |
| DNS | 53 | Configuration and exposure |

Example:

```bash
# Identify SMB services on a lab host
nmap -p445 -sV 192.168.1.100

# Enumerate available SMB shares
smbclient -L //192.168.1.100
```

## Installation and Persistence

Persistence refers to mechanisms that allow software or processes to remain available after events such as logouts or reboots.

### Windows Persistence

Common mechanisms:

- Registry Run keys.
- Windows Scheduled Tasks.
- Windows Services.
- WMI event subscriptions.
- Startup folders.
- Authentication configuration.

Useful defensive commands:

```powershell
# Inspect scheduled tasks
Get-ScheduledTask

# Inspect services
Get-Service

# Inspect startup registry entries
Get-ItemProperty `
"HKCU:\Software\Microsoft\Windows\CurrentVersion\Run"
```

### Linux Persistence

Common mechanisms:

- Cron jobs.
- Systemd services.
- Systemd timers.
- SSH authorized keys.
- Shell initialization files.
- Startup scripts.

Useful inspection commands:

```bash
# Current user's cron entries
crontab -l

# Systemd timers
systemctl list-timers --all

# Running services
systemctl list-units --type=service

# Inspect SSH authorized keys
cat ~/.ssh/authorized_keys
```

### Advanced Persistence Concepts

- Rootkits.
- Bootkits.
- UEFI implants.
- Kernel modules.
- Active Directory Golden Tickets.

These mechanisms are particularly important in malware analysis and incident response.

## Command and Control

Command and Control (C2) describes communication between an external controller and software executing within an assessed environment.

### Common C2 Frameworks

| Framework | Language | Platform |
|---|---|---|
| Sliver | Go | Cross-platform |
| Covenant | C# | Primarily Windows |
| Empire | Python | Cross-platform |
| Cobalt Strike | Java | Cross-platform |
| Metasploit | Ruby | Cross-platform |

### Communication Protocols

- HTTP.
- HTTPS.
- DNS.
- TCP.
- WebSockets.
- Cloud service APIs.

### C2 Assessment Considerations

Evaluate:

- Outbound firewall rules.
- DNS monitoring.
- Network traffic inspection.
- TLS certificate validation.
- Unusual beaconing patterns.
- Unexpected external connections.
- Endpoint detection capabilities.

### Network Architecture

```text
Testing Infrastructure
        |
     Firewall
        |
   Test Network
        |
   Lab Endpoint
        |
   Monitoring / SIEM
```

C2 infrastructure should remain isolated from unauthorized networks and operate only within approved testing boundaries.

## Actions on Objectives

This phase evaluates whether an adversary could access or affect the resources relevant to an organization's security objectives.

### Intelligence Gathering

Potential assessment areas:

- User accounts.
- Network configuration.
- Installed applications.
- Database permissions.
- File access controls.
- Security configurations.
- Sensitive business information.

### Credential Security

Evaluate:

- Password policies.
- Multi-factor authentication.
- Credential storage.
- Privileged account management.
- Kerberos configuration.
- Local administrator permissions.
- Credential exposure risks.

### Active Directory Assessment

Useful tools:

| Tool | Purpose |
|---|---|
| BloodHound | Active Directory relationship analysis |
| SharpHound | Active Directory data collection |
| PowerView | Windows domain enumeration |
| Impacket | Network protocol assessment |
| PingCastle | Active Directory security assessment |

### Data Discovery

Use controlled test files and synthetic information to evaluate access controls.

Example Linux inventory:

```bash
find /home -type f \
\( -name "*.txt" -o -name "*.pdf" \) \
2>/dev/null
```

Avoid collecting actual sensitive information unless specifically authorized.

## Lateral Movement

Lateral movement describes accessing additional systems within an environment after obtaining an initial foothold.

### Windows Environments

Important concepts:

- SMB authentication.
- Windows Remote Management.
- WMI.
- Remote Desktop.
- Kerberos.
- Active Directory trust relationships.
- Privileged account reuse.

### Linux Environments

Important concepts:

- SSH authentication.
- SSH key management.
- Sudo permissions.
- SUID binaries.
- Shared accounts.
- Network file systems.
- Service permissions.

Useful Linux assessment commands:

```bash
# Current identity
id

# Available sudo permissions
sudo -l

# Network interfaces
ip addr

# Routing table
ip route

# Listening services
ss -tulpn
```

### Network Pivoting

Pivoting involves using an authorized system to reach another network segment.

Concepts to understand:

- Routing.
- SOCKS proxies.
- SSH tunnels.
- Network segmentation.
- Firewall rules.
- Internal DNS.

## Data Collection and Exfiltration

Data collection identifies information accessible to an adversary.

Exfiltration describes transferring that information outside the environment.

### Common Risk Areas

- Unrestricted outbound connections.
- Weak DLP policies.
- Excessive file permissions.
- Unmonitored cloud storage.
- Unauthorized removable media.
- Inadequate network segmentation.

### Detection Opportunities

Monitor:

- Unusual outbound traffic.
- Unexpected DNS queries.
- Large data transfers.
- Unusual cloud uploads.
- Abnormal authentication activity.
- Access to sensitive directories.

Use synthetic test data when validating data loss prevention controls.

## Covering Tracks and Detection

Understanding anti-forensics is important for identifying suspicious activity during incident response.

### Windows Artifacts

Investigate:

- Windows Event Logs.
- PowerShell logs.
- Scheduled Task history.
- Prefetch files.
- Registry modifications.
- Service installation events.

### Linux Artifacts

Investigate:

- `/var/log/auth.log`
- `/var/log/syslog`
- `/var/log/audit/`
- Shell history.
- Cron logs.
- Systemd journal.
- File modification timestamps.

### Useful Commands

```bash
# Review authentication logs
sudo journalctl -u ssh

# Review cron activity
sudo journalctl -u cron

# Review recent system events
sudo journalctl --since "24 hours ago"

# Inspect recent logins
last

# Inspect failed login attempts
lastb
```

### Defensive Monitoring

Recommended tools:

- Wazuh.
- OSSEC.
- AIDE.
- Auditd.
- Sysmon.
- Zeek.
- Suricata.
- Snort.

## Red Team Tools Arsenal

### Reconnaissance

| Tool | Purpose |
|---|---|
| Nmap | Network discovery |
| Amass | Domain enumeration |
| Subfinder | Subdomain discovery |
| theHarvester | OSINT collection |
| Shodan | Internet asset discovery |
| Recon-ng | Reconnaissance framework |

### Exploitation

| Tool | Purpose |
|---|---|
| Metasploit | Exploitation framework |
| Burp Suite | Web application testing |
| OWASP ZAP | Web security assessment |
| SQLMap | SQL injection testing |
| Nikto | Web server assessment |

### Post-Exploitation and Assessment

| Tool | Purpose |
|---|---|
| BloodHound | Active Directory analysis |
| Impacket | Network protocol testing |
| PowerView | Domain enumeration |
| LinPEAS | Linux privilege escalation auditing |
| WinPEAS | Windows privilege escalation auditing |
| Seatbelt | Windows security assessment |

### Detection and Monitoring

| Tool | Purpose |
|---|---|
| Wazuh | SIEM and endpoint monitoring |
| Zeek | Network security monitoring |
| Suricata | IDS/IPS |
| Snort | Network intrusion detection |
| Sysmon | Windows system monitoring |
| Auditd | Linux audit logging |

## Operational Security

Operational security (OPSEC) involves protecting the integrity, safety, and confidentiality of an authorized engagement.

### Best Practices

- Maintain written authorization.
- Use dedicated testing infrastructure.
- Keep testing environments isolated.
- Protect assessment credentials.
- Encrypt stored assessment data.
- Maintain accurate activity logs.
- Avoid unnecessary disruption.
- Establish emergency communication procedures.
- Document all testing activities.

### Infrastructure Security

Consider:

- Firewall restrictions.
- SSH key authentication.
- Access control.
- Network segmentation.
- TLS encryption.
- Centralized logging.
- Infrastructure cleanup.

## Legal and Ethical Guidelines

Red team operations must be conducted within clearly established legal boundaries.

### Required Documentation

- Signed authorization.
- Statement of Work (SOW).
- Rules of Engagement.
- Approved testing scope.
- Data handling agreements.
- Emergency contact information.
- Reporting requirements.

### Professional Responsibilities

- Do not test unauthorized systems.
- Do not access unrelated personal information.
- Avoid destructive testing without explicit approval.
- Preserve evidence.
- Report vulnerabilities responsibly.
- Provide actionable remediation recommendations.

## Resources

### Frameworks

- [MITRE ATT&CK](https://attack.mitre.org/)
- [Cyber Kill Chain](https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [OWASP](https://owasp.org/)

### Training

- [Hack The Box](https://www.hackthebox.com/)
- [TryHackMe](https://tryhackme.com/)
- [PortSwigger Web Security Academy](https://portswigger.net/web-security)
- [OverTheWire](https://overthewire.org/)
- [MITRE ATT&CK Training](https://attack.mitre.org/resources/learn-more-about-attack/)

### Books

- Red Team Field Manual — Ben Clark
- The Art of Deception — Kevin Mitnick
- Advanced Penetration Testing — Wil Allsopp
- Red Team Development and Operations — Joe Vest

---

## Conclusion

Red team operations combine reconnaissance, vulnerability assessment, exploitation research, network analysis, and adversary simulation.

A successful engagement does more than identify vulnerabilities. It demonstrates how security weaknesses affect an organization and provides practical recommendations for improving its defenses.

**The objective is to improve security through realistic, authorized adversary simulation.**

---

*For educational purposes, authorized penetration testing, and security research only.*
