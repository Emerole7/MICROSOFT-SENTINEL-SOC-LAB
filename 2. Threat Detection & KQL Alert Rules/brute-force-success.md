# Rule: Brute Force Success — Valid Account Compromise

| Field | Value |
| --- | --- |
| MITRE Tactic | Initial Access, Defense Evasion, Persistence |
| MITRE Technique | T1078 — Valid Accounts |
| Severity | High |
| Run frequency | Every 5 minutes |
| Lookup window | Last 1 hour |

## Description
Detects a successful login (Event ID 4624) from an IP that previously generated 5+ failed logins within the same hour. Indicates a brute force attempt may have succeeded. This is a chained detection — two events correlated together for high confidence.

## KQL

```kql
let BruteForceIPs = SecurityEvent
    | where EventID == 4625
    | where TimeGenerated > ago(1h)
    | summarize FailedCount = count() by IpAddress
    | where FailedCount >= 5;
SecurityEvent
| where EventID == 4624
| where LogonType == 10 or LogonType == 3
| join kind=inner BruteForceIPs on IpAddress
| project
    TimeGenerated, Account, IpAddress, FailedCount,
    LogonType, Computer,
    MITRE_Technique = "T1078 - Valid Accounts (post brute force)"
```

## Entity Mapping

| Entity | Identifier | Column |
| --- | --- | --- |
| IP | Address | IpAddress |
| Account | Name | Account |
| Host | HostName | Computer |

## Why This Matters
A brute force alone is noisy and common. A brute force FOLLOWED by a successful login is a critical, high-confidence signal that the attacker gained access.
