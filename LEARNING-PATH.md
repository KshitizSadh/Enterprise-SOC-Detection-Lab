# LEARNING PATH — Enterprise SOC Lab (Month 1, do FIRST)

Follow in order. Don't skip. Check off in PROGRESS-CHECKLIST.md. Each phase ends with evidence.

## Phase 0 — Foundations (2-3 days)
- [ ] Learn: SOC tiers (L1/L2/L3), NIST IR lifecycle, MITRE ATT&CK (T1059, T1110, T1547, T1078)
- [ ] Do: Fill `mitre/attack-mapping.md` with 9 techniques you WILL cover
- [ ] Output: architecture.png in `architecture/` (draw.io — Windows+Ubuntu → Wazuh → Dashboard → Analyst)

## Phase 1 — Build Telemetry (1 week)
- [ ] Deploy Wazuh manager (Docker or VM) — doc steps in `deployment/wazuh-manager.md`
- [ ] Enroll Windows 11 agent — `deployment/windows-agent.md`
- [ ] Install Sysmon + config (`deployment/sysmon-config.xml` — use SwiftOnSecurity base, explain each rule)
- [ ] Enroll Ubuntu agent — `deployment/ubuntu-agent.md` (auditd)
- [ ] Verify: `agent_control -l` shows both agents, logs flowing
- Learn: Event IDs 4624/4625/4688, Sysmon 1/3/11/13, syslog auth.log

## Phase 2 — Attack → Detect (Wazuh) (1 week)
For EACH in `attacks/` (brute-force.md, powershell.md, suspicious-process.md, persistence.md, ssh-attack.md):
1. Hypothesize logs → 2. Run controlled attack → 3. Find raw logs → 4. Write Wazuh rule → 5. Re-run → 6. Capture alert
- [ ] Scenario 1 brute-force: generate 10+ failed RDP/SMB logons, rule on >5 fails/2min
- [ ] Scenario 2 PowerShell: `powershell -enc ...`, detect parent process + commandline + Sysmon EID 1
- [ ] Scenario 3 persistence: registry run key / scheduled task
- [ ] Scenario 4 Linux SSH brute + sudo abuse
- Save rules in `detections/windows|linux/` + Sigma in `detections/sigma/`

## Phase 3 — Splunk Depth (4-5 days) — in `siem/splunk/`
- [ ] Install Splunk Free / Docker, forward Wazuh logs via syslog/UF — doc in `splunk-setup.md`
- [ ] Learn SPL: `index=* | stats count by ...`, `transaction`, `tstats`
- [ ] Convert each Wazuh rule to SPL saved search in `searches/` (e.g., `splunk-bruteforce.spl`)
- [ ] Map to CIM (Authentication, Process_Creation) — `cim-mapping.md`
- [ ] Build 1 dashboard (failed logons by user/IP + PowerShell timeline) — export to `dashboards/`
- [ ] Create 2 correlation searches (ES-style: notable + risk score)
- Interview line: "Normalized to CIM, built correlation searches with risk-based alerting"

## Phase 4 — QRadar Depth (3-4 days) — in `siem/qradar/`
- [ ] No free QRadar? Use concepts: document CRE logic + AQL in `qradar-setup.md`
- [ ] Write AQL for each use-case in `aql-queries/` (e.g., `SELECT ... FROM events WHERE ...`)
- [ ] Define CRE rules in `cre-rules/` (BB:Building Block + rule + reference set)
- [ ] Define reference sets (malicious IPs, admin users) in `reference-sets/`
- [ ] Write `siem/comparison/wazuh-vs-splunk-vs-qradar.md` (data model, rule language, strengths)
- Interview line: "Can translate any detection across Wazuh/SPL/AQL"

## Phase 5 — SOAR + Automation (3-4 days) — in `soar/` + `automation/`
- [ ] `alert-parser.py`: parse Wazuh JSON → extract IP/user/process/timestamp/rule/severity
- [ ] `alert-triage.py`: enrich (abuseIPDB mock + MITRE map) → LLM summary (Ollama optional, analyst-assist only)
- [ ] `soar-webhook.py`: send to Shuffle/Slack webhook mock
- [ ] Write 3 playbooks in `soar/playbooks/`: brute-force-response.md, malware-containment.md, phishing-triage.md (trigger → steps → containment → evidence → SLA)
- [ ] Measure: time to summarize 10 alerts manually vs with script → record % reduction
- Learn: SOAR = enrichment + orchestration + human-in-loop, not auto-block everything

## Phase 6 — UEBA (2-3 days) — in `ueba/`
- [ ] `baselining.md`: define normal per user/host (logon hours, processes, IPs) from 7 days logs
- [ ] `user-risk-scoring.py`: score = rarity + sensitivity + volume (e.g., first-seen IP + admin + off-hours = high)
- [ ] Add 3 anomaly rules in `anomaly-rules/`: impossible-time logon, first-seen parent-child, beaconing (periodic outbound)
- [ ] Validate: must show 1 true-positive + 1 false-positive tuned out — document tuning
- Interview line: "Layered signature + anomaly to catch unknown, tuned FP rate"

## Phase 7 — Investigate + Report (3 days)
- [ ] Complete 3 incidents in `investigations/incident-00X.md` using full template
- [ ] Generate `reports/incident-reports/` + 1-page executive summary
- [ ] Update `docs/detection-engineering.md`, `incident-response.md`, `lessons-learned.md`
- [ ] Collect 8-10 screenshots max per evidence strategy

**Done = 12+ detections, 3 SIEMs covered, 3 playbooks, 3 incidents, real metrics. Then start applying.**
