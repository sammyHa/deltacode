# Repo Launch Checklist (delete this file before making repo public)

## Tonight (30 min)
- [ ] Create GitHub repo: `deltacode-lab` (public)
- [ ] Push this structure: `git init && git add . && git commit -m "DeltaCode lab portfolio" && git push`
- [ ] Fix LinkedIn/portfolio links at top of README.md
- [ ] Pin the repo on your GitHub profile

## This weekend (2-3 hrs)
- [ ] Add SVG architecture diagram to /docs
- [ ] Correct the VLAN table in docs/architecture.md
- [ ] Paste real Wazuh rules 100300-100306 into detections/wazuh-rules/
- [ ] Fill in the 10-scenario matrix in phishing-validation-suite/
- [ ] Write ONE incident report using the template

## Next week
- [ ] Add 3-5 Sigma rules + the dedup fix writeup
- [ ] Add wazuh_to_thehive.py (KEYS STRIPPED + .env.example)
- [ ] Add GitHub Actions workflow + coverage report sample
- [ ] Search all TODO markers and clear them: grep -rn "TODO (Sam)" .

## Security check before public
- [ ] No API keys anywhere (grep -ri "api_key\|apikey\|password\|token" . )
- [ ] No real/public IPs you care about (lab RFC1918 is fine)
- [ ] git history clean (if keys were EVER committed, rotate them)
