# Playbook: Brute-Force Response (example — copy for others in soar/playbooks/)
## Trigger
Wazuh rule 5712 / Splunk correlation `bruteforce` / QRadar CRE `BruteForce BB` fires, severity High.

## Enrich (auto — alert-triage.py)
1. Parse: src_ip, username, host, count, window
2. Enrich: abuseIPDB (mock), asset criticality, prior fails for user
3. MITRE: T1110.001 → attach

## Decide (analyst)
- If >20 fails + success 4624 after → escalate to P1 incident
- If <10 fails, no success → monitor + block IP at FW if external

## Contain
- [ ] Block src_ip at firewall / Wazuh active-response
- [ ] Force password reset if success suspected
- [ ] Isolate host if lateral movement seen

## Evidence
Save alert JSON + SPL/AQL + timeline to investigations/ + evidence/

## SLA
Triage <15min, contain P1 <1h. Measure before/after automation in README Results.

## SOAR notes
Human-in-loop: never auto-close. Log all actions for audit.
