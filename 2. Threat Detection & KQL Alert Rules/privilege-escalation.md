# Rule: Privilege Escalation — User Added to Administrators Group

| Field | Value |
| --- | --- |
| MITRE Tactic | Privilege Escalation, Persistence |
| MITRE Technique | T1098 Account Manipulation / T1078.002 Local Accounts |
| Severity | High |
| Run frequency | Every 5 minutes |
| Lookup window | Last 1 hour |

## Description
Detects Event ID 4732 — a member was added to a security-enabled local group. Filtered to the Administrators group. Attackers who gain low-privilege access commonly escalate by adding a backdoor account to local Administrators.

## KQL

```kql
SecurityEvent
| where EventID == 4732
| where TargetUserName has "Administrators"
| project
    TimeGenerated, SubjectUserName, MemberName,
    TargetUserName, Computer,
    MITRE_Technique = "T1098 - Account Manipulation / T1078.002 - Local Accounts"
| order by TimeGenerated desc
```

## Entity Mapping

| Entity | Identifier | Column |
| --- | --- | --- |
| Account | Name | SubjectUserName |
| Account | Name | MemberName |
| Host | HostName | Computer |

## Simulation Commands
```cmd
net user labattacker Password123! /add
net localgroup Administrators labattacker /add
```
Cleanup: `net user labattacker /delete`
