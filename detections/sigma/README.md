# Sigma Rules

Vendor-agnostic detection rules authored for DeltaCode, validated through the [Detection-as-Code pipeline](../../detection-as-code/) — every rule is tested against Atomic Red Team emulation before merge, with false-positive rates tracked per rule.

## Format

Each rule includes:
- MITRE ATT&CK technique mapping in tags
- `falsepositives` section based on actual tuning observations (not boilerplate)
- Validation status from CI pipeline

## Example: Suspicious Service Account Interactive Logon

See [`svc_account_interactive_logon.yml`](svc_account_interactive_logon.yml) — detects interactive logons (type 2/10) by service accounts that should only authenticate as services. Service accounts logging on interactively is a classic lateral-movement / credential-abuse signal (T1078.002).

> 🔧 **TODO (Sam):** Add 3–5 of your best Sigma rules from the Detection-as-Code pipeline. Prioritize ones with an interesting tuning story — those become LinkedIn posts too.
