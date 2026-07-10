# Detection-as-Code Pipeline

A five-phase CI/CD pipeline treating detection rules like production software: version-controlled, automatically validated, tested against live adversary emulation, and measured.

## Pipeline Phases

```
1. AUTHOR      Sigma rules written + ATT&CK-mapped (this repo)
        │
2. VALIDATE    GitHub Actions CI — syntax, schema, metadata checks
        │       (self-hosted runner for private lab access)
3. TEST        Proxmox snapshot → Atomic Red Team execution →
        │       verify alert fires → revert snapshot
4. MEASURE     False-positive rate tracked per rule against benign baseline
        │
5. REPORT      Auto-generated ATT&CK Navigator coverage layers +
                coverage reports
```

## Why Self-Hosted Runner

The lab is private (no inbound exposure), so GitHub-hosted runners can't reach it. A self-hosted runner inside the lab polls GitHub outbound-only — CI gets lab access without opening the perimeter.

## Artifacts

> 🔧 **TODO (Sam):** Add to this folder:
> - Your GitHub Actions workflow YAML (`.github/workflows/` at repo root, or a sanitized copy here)
> - The Proxmox snapshot/revert orchestration script
> - A sample auto-generated coverage report
> - An exported ATT&CK Navigator layer JSON (screenshot of the layer makes a great README image and LinkedIn post)
