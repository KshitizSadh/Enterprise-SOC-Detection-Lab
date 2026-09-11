# MITRE ATT&CK Mapping — Enterprise SOC Lab

Fill column "My Lab Scenario" with YOUR actual host/user/command before Phase 2. 9 techniques = 12+ detections.

| # | Technique | Tactic | Lab Scenario (YOU fill) | Log Sources | Detection Location | Severity | Status |
|---|-----------|--------|-------------------------|-------------|-------------------|----------|--------|
| 1 | T1110.001 Brute Force: Password Guessing | Credential Access | RDP/SMB 10x fails on WIN11-01 as analyst01 | Win 4625, Wazuh 5712 | `detections/windows/brute-force.xml` + SPL + AQL | High | TODO |
| 2 | T1110.003 Brute Force: Password Spraying | Credential Access | 1 password vs 5 users | Win 4625 aggregated by src_ip | same as above, spray variant | High | TODO |
| 3 | T1059.001 PowerShell | Execution | `powershell -enc ...`, `IEX`, `-nop -w hidden` | Sysmon 1, 4688, 4104 | `detections/windows/powershell-abuse.xml` | High | TODO |
| 4 | T1059.003 Windows Command Shell | Execution | `cmd /c whoami & net user` from suspicious parent | Sysmon 1 (parent winword/explorer) | `detections/windows/suspicious-process.xml` | Medium | TODO |
| 5 | T1547.001 Registry Run Keys | Persistence | `reg add HKCU\Software\Microsoft\Windows\CurrentVersion\Run` | Sysmon 13, 4688 | persistence rule | High | TODO |
| 6 | T1053.005 Scheduled Task | Persistence/Execution | `schtasks /create /tn Updater /tr evil.exe /sc onlogon` | Win 4698, Sysmon 1 | persistence rule | High | TODO |
| 7 | T1078 Valid Accounts | Defense Evasion/Persistence | Login after brute-force success, `4624 LogonType 10` | Win 4624, 4672 | correlation: fail burst → success | Critical | TODO |
| 8 | T1110 SSH Brute (Linux) + T1078 | Credential Access | `hydra -l root -P list ssh://ubuntu01` | auth.log Failed password | `detections/linux/ssh-bruteforce.xml` | High | TODO |
| 9 | T1068 / T1548.003 Sudo Abuse (Linux privesc) | Privilege Escalation | `sudo -l`, `sudo vim -c '!sh'`, SUID find | auth.log sudo, auditd | `detections/linux/privilege-escalation.xml` | High | TODO |

## Per-detection doc (copy into each rule file header)
```
Detection: <name>
MITRE: <ID>
Trigger: <e.g., >5x 4625 from one IP in 2m>
Log source: <Sysmon EID / Win EID / syslog>
Severity: <Critical/High/Med + why>
Response: <playbook in soar/playbooks/ + containment>
Splunk: <search file> QRadar: <AQL file> UEBA: <anomaly if any>
```

## Coverage checklist
- [ ] Each technique has ≥1 Wazuh rule + ≥1 SPL + ≥1 AQL (Phase 3-4)
- [ ] Each has 1 attack procedure in `attacks/` with raw log sample
- [ ] Each maps to ≥1 incident in `investigations/` (need 3+ total)
- [ ] UEBA covers T1078 + T1110 (impossible-time, first-seen)

References: MITRE ATT&CK Enterprise matrix, SigmaHQ rules (convert in `detections/sigma/`).
