# Integration Scripts

Custom automation glue for the DeltaCode stack.

## Wazuh → TheHive Integration (Python)

Forwards Wazuh alerts into TheHive as cases with MITRE ATT&CK tags carried through from rule metadata, triggering automatic Cortex enrichment (VirusTotal, AbuseIPDB, MaxMind, URLScan, Shodan).

> 🔧 **TODO (Sam):** Add `wazuh_to_thehive.py` — **strip all API keys first** and replace with environment variable references (`os.environ["THEHIVE_API_KEY"]`). Add a `.env.example` file showing required variables. Never commit real keys; if any were ever in the file's history, rotate them.

## Other Automation

> 🔧 TODO: Filebeat/Zeek deployment notes, swaks wrappers (or link to phishing-validation-suite), Proxmox orchestration (or link to detection-as-code).
