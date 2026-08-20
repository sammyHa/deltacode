# Incident Reports

Full-lifecycle incident reports from DeltaCode lab exercises, written to production SOC standards: executive summary, detection detail, timeline, analysis, IOCs, CVSS scoring, NIST 800-61 response phases, and lessons learned.

**Every report here corresponds to a real alert chain in the lab** — detection fired in Wazuh/Suricata/Zeek, case managed in TheHive, triaged against a playbook, and closed with documented remediation.

| Report | Scenario | ATT&CK | Severity |
|---|---|---|---|
| [*IR-2026-08-18-iis-webshell-persistence.md*](IR-2026-08-18-iis-webshell-persistence.md) | Unauthorized SMB Access RCE | T1021.002, T1505.003, T1059/T1571, T1547.001, T1027.002 | HIGH |

> 🔧 **TODO (Sam):** Your first 2–3 reports are the highest-value items in this entire repo. Pick your best lab incidents — a phishing scenario from the validation suite and an Atomic Red Team detection are ideal — and write them using [TEMPLATE_incident_report.md](TEMPLATE_incident_report.md). Each finished report is also a LinkedIn post.

Template: [TEMPLATE_incident_report.md](TEMPLATE_incident_report.md)
