# UEBA Baselining — fill from 7 days of YOUR logs (ueba/baselining.md starter)
## Per-User Normal
- User: e.g., analyst01 — logon 09:00-18:00 IST, hosts: WIN11-01, IPs: 192.168.56.x
- Processes normally seen: explorer.exe→chrome.exe, vscode.exe
## Per-Host Normal
- WIN11-01: PowerShell rare (1x/week, admin only), no mimikatz, no encoded commands
## Risk Scoring (implement in user-risk-scoring.py)
score = rarity(0-40) + sensitivity(0-30) + volume(0-30)
- First-seen IP +40? Off-hours +20? Admin target +30? Encoded PS +30?
## Anomaly Rules (ueba/anomaly-rules/ — 3 required)
1. impossible-time-logon.md
2. first-seen-parent-child.md
3. beaconing-periodic-outbound.md
## Tuning Log
| Date | Rule | FP cause | Fix (allowlist/threshold) |
|------|------|----------|---------------------------|
|      |      |          |                           |
Must show 1 TP + 1 FP tuned.
