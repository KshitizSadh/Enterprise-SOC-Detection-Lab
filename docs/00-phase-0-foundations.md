# Phase 0 — SOC Foundations (2-3 days)

Goal: speak like L1 analyst in interviews. End with `mitre/attack-mapping.md` filled + `architecture/architecture.png` drawn.

Do in order: 1 → 2 → 3 → tasks → self-test.

---

## 1. SOC Tiers — what each does

**L1 Triage Analyst (you are targeting this)**
- Monitors queue, triages alerts in <15 min, classifies TP/BP/FP
- Enriches: who (user/host), what (process/commandline), where (IP/asset), when
- Escalates with timeline + evidence, never closes unclear P1 alone
- Tools: SIEM dashboard, SOAR playbook, ticketing

**L2 Investigator**
- Deep dive: process tree, network, persistence, scope (1 host or 10?)
- Correlates across log sources, maps to MITRE, decides containment
- Tunes noisy rules

**L3 / Detection Engineer / IR Lead**
- Writes detections (Sigma → Wazuh/SPL/AQL), threat hunts, leads incidents
- Owns playbooks, UEBA models, post-mortems

Interview line: "L1 buys time with fast triage; L2 proves scope; L3 prevents recurrence with better detections."

### Your lab role-play
Every alert you get in Phase 2+: act as L1 first (triage in template), then L2 (scope + root cause), then L3 (improve rule). Save all three views in `investigations/`.

---

## 2. NIST IR Lifecycle (SP 800-61) — memorize this order

```
Preparation → Detection & Analysis → Containment → Eradication → Recovery → Lessons Learned
```

Map to YOUR lab flow:
- Preparation = Phase 1 (Sysmon, auditd, baselines in `ueba/baselining.md`)
- Detection & Analysis = Phase 2-4 (Wazuh/Splunk/QRadar) + Phase 6 (UEBA)
- Containment/Eradication/Recovery = `soar/playbooks/` (block IP, kill process, reset creds, rescan)
- Lessons Learned = `docs/lessons-learned.md` + rule tuning (every FP → rule fix)

For each incident you write, label which NIST phase each timeline entry belongs to.

---

## 3. MITRE ATT&CK — only what you need for this lab

Structure: Tactic (why) → Technique (how) → Sub-technique → Procedure (your exact command).

Example:
- Tactic: Execution → Technique T1059.001 PowerShell → Procedure: `powershell -enc SQBFAFg...`

You will cover 9 techniques (pre-filled in `mitre/attack-mapping.md`). Learn these 4 deeply now, rest in Phase 2:

| ID | Name | Your lab procedure | Key logs |
|----|------|-------------------|----------|
| T1110.001 | Brute Force: Password Guessing | 10x failed RDP/SMB + SSH | 4625, auth.log, Wazuh 5712 |
| T1059.001 | PowerShell | `powershell -enc`, `IEX`, `-bypass` | Sysmon 1, 4688, ScriptBlock 4104 |
| T1547.001 | Persistence: Registry Run Keys | `reg add HKCU\...\Run` | Sysmon 13, 4688 |
| T1078 | Valid Accounts | login with guessed creds, then `whoami /priv` | 4624 type 10/3, anomaly |

Detection pattern for each: `Trigger condition → Log source → Rule → Severity → Response`. You will write this for all 9 in Phase 2.

CIM/QRadar note (for later): same technique = different language (Wazuh XML vs SPL vs AQL). Concept stays identical.

---

## Tasks (definition of done)

- [ ] Read this file + `mitre/attack-mapping.md`, can explain all 9 techniques in 30 sec each
- [ ] Fill `mitre/attack-mapping.md` column "My Lab Scenario" with YOUR hostnames/usernames (e.g., WIN11-01, analyst01)
- [ ] Draw `architecture/architecture.png` per `architecture/ARCHITECTURE-GUIDE.md` (Windows+Ubuntu → Wazuh → Dashboard + Splunk/QRadar branch → Analyst)
- [ ] Write 1 paragraph in `docs/lessons-learned.md`: "What L1/L2/L3 would each do for a brute-force alert?"
- [ ] Pass self-test below without notes

## Self-test (5 min, no notes)
1. L1 vs L2 vs L3 — who tunes a noisy rule?
2. NIST order after Containment?
3. T1059.001 vs T1110.001 — which log IDs prove each?
4. Sysmon EID 1 vs 13 — what does each see?
5. Explain your architecture diagram in 60 seconds (record yourself).

Answers: 1=L2 proposes, L3 approves/deploys 2=Eradication→Recovery→Lessons 3=4688/Sysmon1+4104 vs 4625/auth.log 4=process create vs registry 5=do it.

## Next
Phase 1 telemetry. Keep this mental model: every log you enable must map to ≥1 of your 9 techniques, or don't collect it yet.
