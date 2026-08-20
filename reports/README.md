## Incident Reports

Full-lifecycle incident reports from DeltaCode lab exercises, written to production SOC standards: executive summary, detection detail, timeline, forensic analysis, IOCs, CVSS scoring, NIST SP 800-61r2 response phases, and lessons learned.

Every report corresponds to a verified alert chain in the lab environment — telemetry captured across **Elastic**, **Suricata**, and **Zeek**, cases managed in **TheHive**, triaged against standardized playbooks, and closed with actionable remediation.

| Report | Scenario | MITRE ATT&CK | Detection Artifacts | Severity |
| :--- | :--- | :--- | :--- | :--- |
| [`IR-2026-08-18-iis-webshell-persistence.md`](./IR-2026-08-18-iis-webshell-persistence.md) | Unauthorized SMB Access & RCE | [`T1021.002`](https://attack.mitre.org/techniques/T1021/002/), [`T1505.003`](https://attack.mitre.org/techniques/T1505/003/), [`T1059.001`](https://attack.mitre.org/techniques/T1059/001/), [`T1547.001`](https://attack.mitre.org/techniques/T1547/001/), [`T1571`](https://attack.mitre.org/techniques/T1571/), [`T1027.002`](https://attack.mitre.org/techniques/T1027/002/) | • [`Sigma: File Dropped into Startup by IIS Worker`](https://github.com/sammyHa/detection-as-code/blob/main/detections/windows/file_event/file_event_win_iis_w3wp_startup_persistence.yml)</br>• [`Sigma: Suspicious IIS Worker Child Process`](https://github.com/sammyHa/detection-as-code/blob/main/detections/windows/process_creation/proc_creation_win_iis_w3wp_susp_child_process.yml) | `CRITICAL` |
