# Phase 3  Incident Response Simulation

## Objective
Simulate real attack scenarios that trigger Phase 2 detection rules, then triage the resulting Sentinel incidents end to end using a structured IR methodology.

## Scenarios Simulated

| Scenario | Attack Type | MITRE | Triggered Rule |
| --- | --- | --- | --- |
| [Scenario A](reports/ir-report-brute-force.md) | Credential Stuffing / Brute Force | T1110.001 | Brute Force rule |
| --- | --- | --- | --- |
| [Scenario C](reports/ir-report-recon.md) | Reconnaissance Commands | T1082 | No rule fired (gap) |

## IR Triage Workflow

Every incident was triaged using this structured process:

```
1. Assign incident to self, set status Active
2. Review alert details: rule name, evidence, entities
3. Open Investigation Graph: visualize IP/Account/Host relationships
4. Run pivot queries: build full timeline around the alert
5. Determine scope: successful login? lateral movement? exfiltration?
6. Close with classification and findings comment
```

## Pivot Queries Used During Investigation

All investigation queries are in investigation-pivots.kql

## Key Findings

- Real brute force traffic began within **10 minutes** of the VM going live
- Top attacking countries observed: Russia, China, Netherlands, United States, Brazil
- Most commonly attempted usernames: administrator, admin, user, guest, test
- Privilege escalation simulation fired alert within **2 minutes** of the `net localgroup` command
- Scenario C (recon commands) produced **no alert** — documented as detection gap, addressed in Phase 4

