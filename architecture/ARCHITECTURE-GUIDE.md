# Architecture Guide — draw this in draw.io, export PNGs to this folder

Required files: `architecture.png`, `network-diagram.png`, `data-flow.png` (1 clean diagram each, no 50-screenshot dumps).

## What to draw (architecture.png)
```
[Windows 11: WIN11-01 + Sysmon] --Wazuh Agent 4.x--> \
                                                   +--> [Wazuh Manager] --> [Wazuh Dashboard] --> [SOC Analyst (you)]
[Ubuntu: ubuntu01 + auditd] ------Wazuh Agent ------> /            |
                                                                     +--syslog/UF--> [Splunk Free] (CIM, dashboards, correlation)
                                                                     +--concepts--> [QRadar CRE/AQL] (documented, no server needed)
Labels: IPs (192.168.56.0/24), ports (1514/1515/55000), log types per arrow (Sysmon EID1/3/13, 4624/4625/4688, auth.log)
```

## network-diagram.png
- Subnet, hostnames, manager IP, which host attacks which (attacker = your own Kali/Windows, lab-only)
- Firewall points where SOAR would block IP

## data-flow.png (most important for interviews)
```
ATTACK (controlled) → TELEMETRY (Sysmon/auditd) → DETECTION (Wazuh rule/SPL/AQL) → ALERT (JSON/notable/offense)
→ INVESTIGATION (timeline + MITRE) → RESPONSE (playbook) → LESSON → IMPROVED DETECTION
```
Annotate each arrow with YOUR file: e.g., Detection → `detections/windows/brute-force.xml`.

## Draw.io tips
- draw.io → Device + Network + AWS icons, 16:9, dark text on white, export 200dpi PNG
- Keep to 7±2 boxes. If recruiter can't grasp in 60 sec, simplify.
- Save source `.drawio` alongside PNG for future edits.

## Done when
- [ ] 3 PNGs in this folder, referenced from README Architecture section
- [ ] Every box maps to a real config in `deployment/` (no fantasy boxes)
- [ ] Can whiteboard it from memory in interview
