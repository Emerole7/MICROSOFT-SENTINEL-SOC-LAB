# Phase 4 — Threat Hunting

## Objective
Proactively search for attacker activity using hypothesis driven hunts. Unlike Phase 2 (reactive alerts), hunting assumes the attacker may already be present and looks for evidence using KQL. Map all findings to MITRE ATT&CK and document detection gaps.

## Hunting Methodology

```
Hypothesis
    |
    v
KQL Query in Sentinel Hunting workspace
    |
    v
Findings — evidence for or against the hypothesis
    |
    v
Conclusion — threat present / not present / inconclusive
    |
    v
Detection Gap? --> Promote to new analytics rule
```

## Hunts Completed

| Hunt | Hypothesis | Technique | Gap Found? |
| --- | --- | --- | --- |
| [Hunt 1](hunt-01-off-hours-logins.kql) | Attackers log in at night when real users don't | T1078 | Yes — no off-hours rule |
| --- | --- | --- | --- |
| [Hunt 3](hunt-03-high-volume-connections.kql) | Compromised machine connecting to many IPs | T1071 | Yes — needs network data |
| --- | --- | --- | --- |
| [Hunt 5](hunt-05-recon-commands.kql) | Recon commands run post-compromise | T1082 | Yes — needs 4688 logging |

## Detection Gap Analysis

| Hunt | Observation | Gap | Remediation |
| --- | --- | --- | --- |
| Off-hours logins | Logins at 2-4am from attacker IPs | No alert for time based anomaly | Create alert: logins between 00:00-05:00 |
| Dormant accounts | labattacker account created, never logged in | No alert for unused created accounts | Alert on accounts created but unused >24h |
| --- | --- | --- | --- |
| Password spray | Spray pattern distinct from brute force | Original brute force rule misses spray | New rule T1110.003 created |
| Recon commands | Commands ran, no alert fired | 4688 events not collected | Enable process creation audit + DCR update |

## Key Outcome
**1 new detection rule promoted from hunting:** Password Spray (T1110.003) 

