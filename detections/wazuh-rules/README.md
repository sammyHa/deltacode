# Custom Wazuh Rules

Custom detection rules written for the DeltaCode environment.

## Mail Stack Rules (100300–100306)

Detection coverage for the iRedMail stack (Postfix / Dovecot / Amavis / SpamAssassin / ClamAV), covering phishing delivery, malware attachments, and authentication abuse. Validated against the [Phishing Detection Validation Suite](../../phishing-validation-suite/).

> 🔧 **TODO (Sam):** Export your actual rules from `/var/ossec/etc/rules/local_rules.xml` on the Wazuh manager, sanitize any lab-specific IPs if desired, and paste them into `mail_stack_rules.xml` in this folder. Then delete this TODO block.

## Alert Deduplication (Frequency Correlation)

Fixed a duplicate-alert problem by rewriting noisy rules as frequency-correlated composite rules — alerting once per burst window instead of once per event, preserving detection fidelity while cutting case noise in TheHive.

> 🔧 **TODO (Sam):** Add the before/after rule XML and a one-paragraph explanation of the tuning decision. This is one of your strongest "I've lived alert fatigue" artifacts — it deserves its own file: `alert_dedup_fix.xml` + `alert_dedup_writeup.md`.

## Rule Design Principles I Follow

1. **Map every rule to MITRE ATT&CK** — technique ID in the rule metadata, carried through to TheHive tags
2. **Tune against real noise** — every rule is run against benign traffic before promotion
3. **Frequency over spam** — burst events correlate into single alerts
4. **Severity means something** — levels reserved so a level-12 alert is always worth waking up for
