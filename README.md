# Enterprise SOC & Detection Lab

> Portfolio-grade security engineering project — Answer in interviews: What problem? What did you build? How did you test? What measurable result?

## Problem
Small orgs lack visibility into endpoint attacks (brute-force, PowerShell abuse, persistence, lateral movement). Alerts are noisy, unmapped, and slow to triage.

## What I Built
Multi-endpoint SOC lab with centralized telemetry, custom detections, multi-SIEM analytics (Wazuh + Splunk + QRadar concepts), SOAR triage automation, and UEBA baselining.

**Stack:** Wazuh | Sysmon | Windows 11 | Ubuntu | Splunk (search/CIM/ES) | QRadar (AQL/CRE) | Python | Sigma | MITRE ATT&CK | Shuffle/SOAR concepts | UEBA

## Architecture
```
Windows 11 (Sysmon) ─┐
                     ├─> Wazuh Agent ─> Wazuh Manager ─> Wazuh Dashboard ─> Analyst
Ubuntu (auditd) ─────┘                         │
                                               ├─> Splunk (forwarded logs, CIM, correlation)
                                               └─> QRadar concepts (AQL, CRE, reference sets)
```

See `architecture/` — add your `architecture.png`, `network-diagram.png`, `data-flow.png`.

## Detections (goal: 12+)
- `detections/windows/` — brute-force.xml, powershell-abuse.xml, suspicious-process.xml
- `detections/linux/` — ssh-bruteforce.xml, privilege-escalation.xml
- `detections/sigma/` — Sigma rules converted to Wazuh/Splunk/QRadar
- `siem/splunk/correlation-searches/` — SPL equivalent of each Wazuh rule
- `siem/qradar/cre-rules/` — CRE rule logic + AQL
- `ueba/anomaly-rules/` — impossible-time logon, first-seen process, beaconing

Each detection must document: Trigger → Log source → Rule → MITRE → Severity → Response.

## SOAR + Automation
- `automation/` + `soar/automation/` — alert-parser.py, alert-triage.py, report-generator.py
- `soar/playbooks/` — brute-force-response.md, phishing-triage.md, malware-containment.md
- Goal: Wazuh Alert → Python parser → Enrich (IP/user/process) → MITRE map → LLM-assist summary → Analyst decision (never auto-close as benign).

## Investigations
Use template in `investigations/`:
Incident ID, Date, Host, Severity, Detection, Hypothesis, Evidence, Timeline, MITRE, Root cause, Containment, Remediation, Lessons.

Goal: 3+ full incident reports.

## Results (fill with REAL numbers)
- X custom detections covering Windows/Linux, mapped to Y MITRE techniques
- Splunk: X saved searches, Y dashboards, Z correlation searches (CIM-compliant)
- QRadar: X AQL use-cases, Y CRE rules documented
- SOAR: triage time reduced from X min → Y min (measure it)
- UEBA: X baselines, Y true-positive anomalies validated

## How to Learn (in order)
1. Read `LEARNING-PATH.md`
2. Track work in `PROGRESS-CHECKLIST.md`
3. Build in phases: Wazuh → Splunk → QRadar → SOAR → UEBA
4. Collect evidence per `docs/lessons-learned.md`

## Screenshots (60-second rule)
architecture → attack → alert → investigation → fix → verification. No 50-screenshot dumps.
