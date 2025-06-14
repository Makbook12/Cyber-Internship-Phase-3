  # Advanced Cybersecurity Interview Questions – Phase 3

This document contains advanced detection and analysis answers based on Sysmon, Winlogbeat, ELK Stack, and Threat Hunting techniques used in APT simulations.

---
## Objective
 Objective:
 To simulate complex attack scenarios mimicking Advanced Persistent Threats (APT) and train
 candidates to detect, analyze, and respond using a layered defense strategy

## 1. Detecting Fileless Malware with Sysmon/Winlogbeat

- Fileless malware runs in memory using PowerShell, WMI, or MSHTA without touching disk.
- Detection with Sysmon:
  - Event ID 1: Parent/child process anomalies (e.g., winword.exe → powershell.exe)
  - Event ID 3: Outbound network connections
  - Event ID 7: DLLs loaded like System.Management.Automation.dll
- ELK Query: Search for obfuscated strings like `-enc`, `FromBase64String`, `iex`

---

## 2. Explain and Detect DNS Tunneling

- DNS Tunneling encodes data into DNS queries for exfiltration or command-and-control.
- Detection:
  - Sysmon Event ID 22: Long, encoded dns.question.name
  - Use Zeek or Wireshark to inspect DNS payloads
  - Sigma rules to flag:
    ```yaml
    QueryName|re: '^.{50,}\.domain\.com$'
    ```
- Red Flags: Base64 strings, unresolvable domains, high query volume

---

## 3. Indicators of Lateral Movement

- Signs of lateral movement:
  - Logon events (4624 type 3 or 10)
  - Remote PowerShell or WMI commands
  - psexec, wmic, net use, quser
  - Access to ADMIN$ or C$ shares
- Detection: Correlate login, service creation, and process events

---

## 4. Common Persistence Methods in Windows

- Techniques:
  - Registry keys: Run, RunOnce (Sysmon Event ID 13)
  - Scheduled tasks (schtasks)
  - WMI Event Subscriptions
  - Startup folder and Winlogon keys
  - DLL hijacking or COM hijacking
- Tools: Sysmon, Autoruns, Process Monitor, Sigma

---

## 5. Detecting Mimikatz and Credential Dumping

- Detection Points:
  - Sysmon Event ID 10: Process access to lsass.exe
  - procdump.exe -ma lsass.exe, mimikatz.exe
  - PowerShell with -enc, FromBase64String
  - .dmp files in temp/system folders
- Network Exfiltration: Monitor large HTTPS uploads post-access

---

## 6. Key Sysmon Event IDs for PowerShell

| Event ID | Description                            |
|----------|----------------------------------------|
| 1        | Process creation (e.g., powershell.exe) |
| 3        | Network connection (e.g., C2)          |
| 7        | DLL Load (e.g., System.Management.*)   |
| 10       | Process access (lsass.exe)             |
| 11       | File creation (e.g., dropped script)   |
| 22       | DNS query (for DNS tunneling)          |

---

## 7. PowerShell Payloads and EDR Evasion

- Techniques Used:
  - Base64 encoded payloads: -enc
  - In-memory execution via iex, Reflection.Assembly
  - ExecutionPolicy bypass: -ExecutionPolicy Bypass
  - LOLBins: regsvr32, mshta
- Detection: Monitor command-line parameters and obfuscation patterns

---

## 8. Brute-force RDP Detection Strategies

- Detect failed logons:
  - Event ID 4625 (multiple rapid attempts)
  - Watch for same username, source IP
- Successful brute-force:
  - Spike in 4625 followed by 4624
- Network indicators:
  - RDP port scanning
  - Event ID 3: network connection to port 3389

---

## 9. Honeypots for Lateral Movement

- Deploy fake resources to catch attackers:
  - Decoy RDP, SMB shares, fake admin accounts
- Tools: Canarytokens, Modern Honey Network
- Alerting: Access to honeypot triggers detection and investigation

---

## 10. Correlating SIEM Logs in APT Scenarios

- Combine multiple sources:
  - Winlogbeat → Windows Events
  - Sysmon → Process/network activity
  - Suricata/Zeek → Network layer
- APT Chain Example:
  - Phishing (Office macro)
  - powershell.exe -enc (Sysmon ID 1)
  - DNS tunneling (ID 22)
  - Mimikatz (lsass.exe access – ID 10)
  - HTTPS exfiltration (Zeek logs)

- Tools: ELK dashboards + Sigma rules + threat intelligence

---

Prepared for Red Team, Blue Team, and SIEM Analyst roles  
Lab tested using ELK + Sysmon + Winlogbeat on Kali & Windows VMs
