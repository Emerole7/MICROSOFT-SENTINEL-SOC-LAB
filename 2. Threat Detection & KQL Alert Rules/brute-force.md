# Rule: Brute Force Multiple Failed Logins from Single IP

| Field | Value |
| --- | --- |
| MITRE Tactic | Credential Access |
| MITRE Technique | T1110.001 — Password Guessing |
| Severity | Medium |
| Run frequency | Every 5 minutes |
| Lookup window | Last 10 minutes |
| Threshold | 10+ failed attempts from same IP |

## Description
Detects when a single IP generates 10 or more failed RDP or network login attempts within a 10 minute window. Characteristic of automated brute force tools (Hydra, Medusa, custom scripts).

## KQL

```kql
SecurityEvent
| where EventID == 4625
| where TimeGenerated > ago(10m)
| summarize
    FailedAttempts = count(),
    FirstAttempt = min(TimeGenerated),
    LastAttempt = max(TimeGenerated),
    AccountsTargeted = dcount(TargetUserName),
    AttackedHost = any(Computer)
    by IpAddress
| where FailedAttempts >= 10
| extend
    AttackDuration = datetime_diff('minute', LastAttempt, FirstAttempt),
    MITRE_Technique = "T1110.001 - Password Guessing"
| project
    IpAddress, FailedAttempts, AccountsTargeted,
    AttackedHost, FirstAttempt, LastAttempt,
    AttackDuration, MITRE_Technique
```

## Entity Mapping

| Entity | Identifier | Column |
| --- | --- | --- |
| IP | Address | IpAddress |
| Host | HostName | AttackedHost |

## Tuning Notes
- Threshold of 10 reduces false positives from misconfigured services
- Pair with the Brute Force Success rule to catch successful compromises
