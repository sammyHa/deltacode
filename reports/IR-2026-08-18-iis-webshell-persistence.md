**1. Executive Summary**
- **Overview:** A critical-severity incident involving unauthorized SMB access, remote code execution via an ASPX web shell, and persistent malware installation on the IIS web server.
    
- **Impact:** Adversary gained interactive command execution under `w3wp.exe` privilege and established persistent persistence in the system startup directory.
- **Current Status:** Contained; malicious files isolated, network callbacks blocked, and compromised credentials rotated.
    
**2. Attack Timeline & MITRE ATT&CK Mapping**

| **Timestamp (UTC)**        | **Phase / MITRE ATT&CK**           | **Observed Activity / Evidence**                                    |
| -------------------------- | ---------------------------------- | ------------------------------------------------------------------- |
| `2024-09-10T05:47:11.234Z` | Initial Recon (T1021.002)          | SMB `Tree Connect` probes against `IPC$` and `C$` shares            |
| `2024-09-10T05:48:52.938Z` | Persistence / Delivery (T1505.003) | Upload of `shell.aspx` into `\inetpub\wwwroot\`                     |
| `2024-09-10T05:49:51.952Z  | Execution & C2 (T1059 / T1571)     | HTTP trigger spawning reverse shell to `10.0.2.4:4443`              |
| `2024-09-10T06:08:23.000Z` | Host Persistence (T1547.001)       | `w3wp.exe` (PID `4332`) dropped `updatenow.exe` into Startup folder |
| `=`                        | Defense Evasion (T1027.002)        | Execution of packed binary matched to `AgentTesla`                  |

**3. Technical Investigation & Evidence**

- **Network Analysis (Wireshark):**
    - Identified consecutive SMB2 Tree Connect requests to hidden administrative shares.
    - Extracted file transfer metadata confirming the drop of `shell.aspx`.
    - Correlated HTTP execution request with the subsequent outbound TCP connection over non-standard port `4443`.
        
- **Host Memory Forensics (Volatility 3):**
    - Extracted kernel base address (`0xf80079213000`) via `windows.info`.
    - Mapped process hierarchy with `windows.pstree`, identifying `w3wp.exe` (PID `4332`) initiating outbound sockets (`windows.netscan`).
    - Discovered unauthorized persistence artifact at `C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup\updatenow.exe`.
        
- **Malware & Threat Intelligence:**
    - Analyzed `updatenow.exe` headers to reveal packing/obfuscation characteristics.
    - Correlated file hashes with VirusTotal intelligence to attribute the binary to `AgentTesla`.
    - Queried Threat Intel feeds for the callback FQDN (`cp8nl[.]hyperhost[.]ua`).
        

**4. Indicators of Compromise (IoCs)**

| **Type**               | **Indicator**                        | **Context / Notes**                     |
| ---------------------- | ------------------------------------ | --------------------------------------- |
| **Network (IP:Port)**  | `10.0.2.4:4443`                      | Reverse Shell C2 Listener               |
| **Network (FQDN)**     | `cp8nl[.]hyperhost[.]ua`             | Identified via Threat Intel correlation |
| **File (Name/Path)**   | `\inetpub\wwwroot\shell.aspx`        | Uploaded ASPX Web Shell                 |
| **File (Persistence)** | `...\Programs\Startup\updatenow.exe` | Packed Implant Executable               |
| **Hash (SHA-256)**     | ` c25a6673a2...`                     | Associated with `AgentTesla`            |
|                        |                                      |                                         |

**5. Detection & Remediation Engineering**

- **Detection Rule:** 
	-  [**Sigma Rule**](https://github.com/sammyHa/detection-as-code/blob/main/detections/windows/file_event/file_event_win_iis_w3wp_startup_persistence.yml) targeting `w3wp.exe` spawning command interpreters (`cmd.exe`, `powershell.exe`) or dropping files into `Startup` directories.
	- [**Companion Detection**](https://github.com/sammyHa/detection-as-code/blob/main/detections/windows/process_creation/proc_creation_win_iis_w3wp_susp_child_process.yml) IIS Spawning Command Interpreters or Dropped Executables
	  
	    
- **Remediation Steps:**
    1. Restrict administrative SMB shares (`C$`, `ADMIN$`) via network segmentation and firewall rules.
    2. Implement strict egress filtering to drop non-standard outbound TCP ports ( `4443`).
    3. Enforce AppLocker / Software Restriction Policies on `%ProgramData%\Microsoft\Windows\Start Menu\Programs\Startup\`.
