# Rule: Lateral Movement — Admin Login from New IP

| Field | Value |
| --- | --- |
| MITRE Tactic | Lateral Movement |
| MITRE Technique | T1021 — Remote Services |
| Severity | High |
| Run frequency | Every 15 minutes |
| --- | --- |

## Description
Detects an admin account logging in from an IP not seen in the past 30 days. Baseline-deviation approach catches stolen credentials and lateral movement even when the login itself looks legitimate.

## KQL

```kql
let KnownAdminIPs = SecurityEvent
    | where EventID == 4624
    | where LogonType == 10 or LogonType == 3
    | where Account has "Administrator" or Account has "admin"
    | where TimeGenerated between (ago(30d) .. ago(1h))
    | summarize KnownIPs = make_set(IpAddress) by Account;
SecurityEvent
| where EventID == 4624
| where LogonType == 10 or LogonType == 3
| where Account has "Administrator" or Account has "admin"
| where TimeGenerated > ago(1h)
| join kind=leftouter KnownAdminIPs on Account
| where not(IpAddress in (KnownIPs))
| where isnotempty(IpAddress)
| project
    TimeGenerated, Account, IpAddress, Computer,
    MITRE_Technique = "T1021 - Remote Services (new source IP)"
```

## Entity Mapping

| Entity | Identifier | Column |
| --- | --- | --- |
| IP | Address | IpAddress |
| Account | Name | Account |
| Host | HostName | Computer |

## Tuning Notes
- The 30-day baseline requires 30 days of log data to be meaningful
- Expect false positives early in deployment as the baseline builds
- Consider whitelisting your own admin IP during initial testing
