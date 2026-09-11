# PROGRESS CHECKLIST — SOC Lab
Start applying after this project is polished.

## Telemetry
- [ ] Wazuh manager running
- [ ] Windows agent + Sysmon flowing
- [ ] Ubuntu agent flowing

## Detections (12 goal)
- [ ] brute-force.xml
- [ ] powershell-abuse.xml
- [ ] suspicious-process.xml
- [ ] ssh-bruteforce.xml
- [ ] privilege-escalation.xml
- [ ] Sigma rules (3+)

## Splunk
- [ ] UF/syslog ingestion working
- [ ] 5+ saved SPL searches
- [ ] CIM mapping doc
- [ ] 1 dashboard
- [ ] 2 correlation searches

## QRadar
- [ ] 5+ AQL queries
- [ ] 3+ CRE rules documented
- [ ] Reference sets defined
- [ ] Wazuh vs Splunk vs QRadar comparison

## SOAR
- [ ] alert-parser.py works on real alert
- [ ] alert-triage.py enriches + summarizes
- [ ] 3 playbooks written
- [ ] Triage time measured (before/after)

## UEBA
- [ ] Baselining doc (7-day normal)
- [ ] user-risk-scoring.py runs
- [ ] 3 anomaly rules + 1 FP tuned

## Investigation + Report
- [ ] incident-001.md
- [ ] incident-002.md
- [ ] incident-003.md
- [ ] Executive summary
- [ ] 8-10 screenshots (architecture→attack→alert→investigate→fix→verify)
