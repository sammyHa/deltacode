# Incident Report: [IR-YYYY-NNN] — [Short Incident Title]

**Classification:** Lab Exercise — Sanitized
**Analyst:** Samim Hakimi
**Date Detected:** YYYY-MM-DD HH:MM (PT)
**Date Closed:** YYYY-MM-DD
**Severity:** Critical / High / Medium / Low
**Status:** Closed
**MITRE ATT&CK:** T####.### — [Technique Name]

---

## 1. Executive Summary

*Three to five sentences a non-technical executive can read. What happened, what was the impact (or prevented impact), what was done, what is the residual risk. No jargon.*

## 2. Detection

| Field | Value |
|---|---|
| Alert Source | Wazuh rule ###### / Suricata SID / Zeek notice |
| Alert Severity | Level ## |
| Detection Time | YYYY-MM-DD HH:MM |
| Time to Triage | ## minutes |
| TheHive Case | #### |

*What fired, why it fired, and the first thing the analyst checked.*

## 3. Timeline

| Time (PT) | Event |
|---|---|
| HH:MM | Initial access — [detail] |
| HH:MM | Alert generated |
| HH:MM | Case created in TheHive; triage began |
| HH:MM | Containment action taken |
| HH:MM | Eradication verified |

## 4. Analysis

*The investigative narrative: pivots in Kibana, Zeek conn/dns/http evidence, Sysmon process trees, Cortex enrichment results. What confirmed true positive. Screenshots welcome (sanitized).*

## 5. Indicators of Compromise (IOCs)

| Type | Value | Context |
|---|---|---|
| IP | x.x.x.x | C2 / source |
| Hash (SHA256) | ... | Payload |
| Domain | ... | Delivery |

## 6. Severity Scoring

**CVSS 3.1 Base Score:** #.# ([vector string])
*One paragraph justifying the score in context of the affected asset.*

## 7. Response Actions (NIST 800-61)

- **Containment:** [network isolation, account disable, etc.]
- **Eradication:** [artifact removal, credential rotation, reimage]
- **Recovery:** [restoration steps, validation monitoring period]

## 8. Lessons Learned & Remediation

| # | Finding | Remediation | Status |
|---|---|---|---|
| 1 | [Detection gap / process gap] | [Rule tuned / control added] | Done / Planned |

## 9. Detection Improvements Made

*The rule you wrote or tuned as a result. Link to it in `/detections`.*
