# Phishing Detection Validation Suite

10 scripted attack scenarios (`swaks`-based) run against the DeltaCode iRedMail stack to validate email-security detection coverage end-to-end — from delivery through Amavis/SpamAssassin/ClamAV verdicts to Wazuh alerting (custom rules 100300–100306) and TheHive case creation.

## Scenario Matrix

| # | Scenario | MITRE ATT&CK | Detected By | Status |
|---|---|---|---|---|
| 1 | 🔧 TODO | T1566.001 | | |
| 2 | 🔧 TODO | T1566.002 | | |
| … | | | | |

> 🔧 **TODO (Sam):** Fill in your actual 10 scenarios with technique mappings and which control caught each one. Add the `swaks` scripts to this folder (sanitize recipient domains if desired). The scenario matrix showing detection coverage — including anything that *wasn't* caught and what you did about it — is the story hiring managers want.

## Why This Exists

Writing detection rules isn't the job — *proving they fire* is. This suite is repeatable regression testing for the mail security stack: every rule change gets re-validated against all 10 scenarios.
