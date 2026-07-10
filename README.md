<<<<<<< HEAD
# DeltaCode — Enterprise Security Operations Lab
=======
<<<<<<< HEAD
# DeltaCode: Enterprise Security Operations Lab
>>>>>>> c328c0b (inital)

**A production-grade SOC environment built from bare metal to prove one thing: I can do the job before I have the title.**

Built and operated by **Samim Hakimi** — Detection Engineering & Security Operations | Security+ | CySA+ | Splunk Core Certified User | U.S. Army Veteran (HUMINT) | Clearance Eligible

<<<<<<< HEAD
📍 San Diego, CA · [LinkedIn](https://linkedin.com/in/YOUR-PROFILE) · [Portfolio Site](https://YOUR-USERNAME.github.io)
=======
📍 San Diego, CA · [LinkedIn](https://linkedin.com/in/samimhakimi) · [Portfolio Site](https://sammyHa.github.io)
=======
# DeltaCode — Enterprise Security Operations Lab

**A production-grade SOC environment built from bare metal to prove one thing: I can do the job before I have the title.**

Built and operated by **Samim Hakimi** — Detection Engineering & Security Operations | Security+ | CySA+ | Splunk Core Certified User | U.S. Army Veteran (HUMINT) | Clearance Eligible

📍 San Diego, CA · [LinkedIn](https://linkedin.com/in/YOUR-PROFILE) · [Portfolio Site](https://YOUR-USERNAME.github.io)
>>>>>>> a1ce2ab (Initial commit)
>>>>>>> c328c0b (inital)

---

## What This Is

<<<<<<< HEAD
DeltaCode (`deltacode.local`) is a fully segmented enterprise network I designed, built, and operate daily on physical hardware. It runs the same detection, triage, and incident response workflows as a production SOC — SIEM, EDR, NSM, SOAR, case management, email security, and adversary emulation — mapped end-to-end to MITRE ATT&CK and NIST 800-61.
=======
<<<<<<< HEAD
DeltaCode (`deltacode.local`) is a fully segmented enterprise network I designed, built, and operate daily on physical hardware. It runs the same detection, triage, and incident response workflows as a production SOC SIEM, EDR, NSM, SOAR, case management, email security, and adversary emulation. Mapped end-to-end to MITRE ATT&CK and NIST 800-61.
=======
DeltaCode (`deltacode.local`) is a fully segmented enterprise network I designed, built, and operate daily on physical hardware. It runs the same detection, triage, and incident response workflows as a production SOC — SIEM, EDR, NSM, SOAR, case management, email security, and adversary emulation — mapped end-to-end to MITRE ATT&CK and NIST 800-61.
>>>>>>> a1ce2ab (Initial commit)
>>>>>>> c328c0b (inital)

Every alert in this lab follows the full incident lifecycle:

```
Detection (Wazuh / Suricata / Zeek)
        │
        ▼
Enrichment (Cortex analyzers: VirusTotal, AbuseIPDB, MaxMind, URLScan, Shodan)
        │
        ▼
<<<<<<< HEAD
Case Management (TheHive 5 — auto-created cases w/ MITRE ATT&CK tags)
=======
<<<<<<< HEAD
Case Management (TheHive 5 auto-created cases w/ MITRE ATT&CK tags)
=======
Case Management (TheHive 5 — auto-created cases w/ MITRE ATT&CK tags)
>>>>>>> a1ce2ab (Initial commit)
>>>>>>> c328c0b (inital)
        │
        ▼
Triage & Analysis (playbook-driven, Elastic Stack pivots, Sysmon process trees)
        │
        ▼
<<<<<<< HEAD
Containment → Eradication → Recovery (NIST 800-61)
=======
<<<<<<< HEAD
Containment -> Eradication -> Recovery (NIST 800-61)
=======
Containment → Eradication → Recovery (NIST 800-61)
>>>>>>> a1ce2ab (Initial commit)
>>>>>>> c328c0b (inital)
        │
        ▼
Incident Report (executive summary, IOCs, CVSS, remediation, lessons learned)
```

---

## Infrastructure

| Component | Details |
|---|---|
| **Hypervisor** | Dell PowerEdge R740xd · 128GB RAM · Proxmox VE 8.x |
| **Network** | Arista 7150 10GbE switch · 7 segmented VLANs |
| **Firewall / VPN** | pfSense · WireGuard remote access |
| **SIEM / XDR** | Wazuh (custom rules + active response) |
<<<<<<< HEAD
| **Log Analytics** | Elastic Stack 8.x (Elasticsearch, Kibana, Logstash — dual-pipeline architecture) |
=======
<<<<<<< HEAD
| **Log Analytics** | Elastic Stack 8.x (Elasticsearch, Kibana, Logstash dual-pipeline architecture) |
=======
| **Log Analytics** | Elastic Stack 8.x (Elasticsearch, Kibana, Logstash — dual-pipeline architecture) |
>>>>>>> a1ce2ab (Initial commit)
>>>>>>> c328c0b (inital)
| **NSM** | Suricata (IDS) + Zeek (network metadata) on dedicated sensor, shipped via Filebeat |
| **SOAR / Case Mgmt** | TheHive 5 + Cortex (Docker) with 5 enrichment analyzers |
| **Email Security** | iRedMail (Postfix, Dovecot, Amavis, SpamAssassin, ClamAV) on CORP VLAN |
| **Endpoints** | Sysmon-instrumented Windows clients, Linux servers, Kali Linux |
| **Adversary Emulation** | Atomic Red Team · custom attack scripts |
| **CI/CD** | GitHub Actions self-hosted runner for Detection-as-Code pipeline |

📐 **[Full network architecture diagram →](docs/architecture.md)**

---

## What I Built (Highlights)

### 🔍 Detection Engineering
<<<<<<< HEAD
- **Custom Wazuh rules 100300–100306** for the iRedMail stack — phishing, malware attachments, auth abuse ([see rules](detections/wazuh-rules/))
=======
<<<<<<< HEAD
- **Custom Wazuh rules 100300–100306** for the iRedMail stack, phishing, malware attachments, auth abuse ([see rules](detections/wazuh-rules/))
>>>>>>> c328c0b (inital)
- **Sigma rules** mapped to MITRE ATT&CK, validated through a CI/CD pipeline ([see detections](detections/sigma/))
- **Duplicate-alert suppression** using frequency-correlated Wazuh rules — cut alert noise without losing fidelity

### ⚙️ Detection-as-Code Pipeline
<<<<<<< HEAD
Five-phase pipeline: Sigma rule authoring → GitHub Actions CI validation → Proxmox snapshot/revert test orchestration → ATT&CK Navigator coverage layers → auto-generated coverage reports with false-positive rate tracking. ([details](detection-as-code/))
=======
Five-phase pipeline: Sigma rule authoring -> GitHub Actions CI validation -> Proxmox snapshot/revert test orchestration -> ATT&CK Navigator coverage layers -> auto-generated coverage reports with false-positive rate tracking. ([details](detection-as-code/))
=======
- **Custom Wazuh rules 100300–100306** for the iRedMail stack — phishing, malware attachments, auth abuse ([see rules](detections/wazuh-rules/))
- **Sigma rules** mapped to MITRE ATT&CK, validated through a CI/CD pipeline ([see detections](detections/sigma/))
- **Duplicate-alert suppression** using frequency-correlated Wazuh rules — cut alert noise without losing fidelity

### ⚙️ Detection-as-Code Pipeline
Five-phase pipeline: Sigma rule authoring → GitHub Actions CI validation → Proxmox snapshot/revert test orchestration → ATT&CK Navigator coverage layers → auto-generated coverage reports with false-positive rate tracking. ([details](detection-as-code/))
>>>>>>> a1ce2ab (Initial commit)
>>>>>>> c328c0b (inital)

### 📧 Phishing Detection Validation Suite
10 scripted attack scenarios (`swaks`-based) mapped to MITRE ATT&CK, run against the iRedMail stack to validate detection coverage end-to-end. ([see suite](phishing-validation-suite/))

### 🔗 SOAR Integration
Custom Python integration forwarding MITRE-tagged Wazuh alerts into TheHive as cases, enriched automatically by Cortex analyzers. ([see scripts](scripts/))

### 📝 Incident Reports
Full-lifecycle incident reports written to professional standards — executive summary, timeline, IOCs, CVSS scoring, remediation, lessons learned. ([see reports](reports/))

---

## Why This Matters to a Hiring Manager

<<<<<<< HEAD
=======
<<<<<<< HEAD
- **I triage real alerts daily**: no tutorials, no CTF write-ups. Alerts my own detections generate against my own attack emulation.
- **I've felt the pain points**: alert fatigue, index sync failures, permission issues, integration breakage and fixed them.
- **I document like an analyst**: every incident produces a report a CISO could read.
- **Complementary background**: U.S. Army HUMINT and federal aviation security investigations: collection, triage, and reporting under pressure is the job I've already done, in a different domain.
=======
>>>>>>> c328c0b (inital)
- **I triage real alerts daily** — not tutorials, not CTF write-ups. Alerts my own detections generate against my own attack emulation.
- **I've felt the pain points** — alert fatigue, index sync failures, permission issues, integration breakage — and fixed them.
- **I document like an analyst** — every incident produces a report a CISO could read.
- **Complementary background** — U.S. Army HUMINT and federal aviation security investigations: collection, triage, and reporting under pressure is the job I've already done, in a different domain.
<<<<<<< HEAD
=======
>>>>>>> a1ce2ab (Initial commit)
>>>>>>> c328c0b (inital)

---

## Beyond the Lab

- 40+ completed HackTheBox / Proving Grounds machines
<<<<<<< HEAD
- Active in San Diego security community: BSidesSD, DC858
=======
<<<<<<< HEAD
<!-- Active in San Diego security community: BSidesSD, DC858 -->
=======
- Active in San Diego security community: BSidesSD, DC858
>>>>>>> a1ce2ab (Initial commit)
>>>>>>> c328c0b (inital)
- Currently completing CompTIA CySA+ (CS0-003)

---

*All IPs, hostnames, and identifying details in this repo are from an isolated lab environment. Reports are sanitized.*
