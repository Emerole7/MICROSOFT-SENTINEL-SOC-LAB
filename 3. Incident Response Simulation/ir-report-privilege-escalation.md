# IR Report  Privilege Escalation Simulation

| Field | Value |
| --- | --- |
| Date | May 20th, 2026 |
| Analyst | Prosper |
| Incident ID | 62 |
| Severity | High |
| Status | Closed |
| Classification | True Positive Simulated Attack |

## 1. Summary
A simulated privilege escalation was performed on the honeypot VM. A new local user account (`labattacker`) was created and immediately added to the local Administrators group. Microsoft Sentinel detected this within approximately 2 minutes via Event ID 4732.

## 2. Detection
- **Rule triggered:** Privilege Escalation, User Added to Administrators Group
- **Alert time:** May 20th 2026 5:38:56 PM EST
- **MITRE techniques:** T1098 Account Manipulation, T1078.002  Local Accounts

## 3. Evidence

- **Subject account (who performed the action):** [your admin account]
- **Member added:** labattacker
- **Group modified:** Administrators
- **Target host:** PAYROLL-DB-EAST
- **Event IDs observed:** 4720 (account created), 4732 (added to group)

**Timeline:**
- May 20th, 2026 5:26:12 PM EST — Command `net user labattacker /add` executed (Event ID 4720)
- May 20th, 2026 5:26:12 PM EST — Command `net localgroup Administrators labattacker /add` executed (Event ID 4732)
- May 20th 2026 5:38:56 PM EST  — Alert fired in Sentinel
- May 20th 2026 5:36:16 PM EST  — Incident created

## 4. Investigation Findings

Investigation confirmed this was a controlled simulation. The subject account performing the escalation was the authenticated lab user, not an external attacker. No prior brute force activity preceded this event.

In a real incident, the next investigative step would be:
1. Identify how the subject account was initially compromised
2. Check for any actions taken under the `labattacker` account after it was created
3. Determine if the account was used to persist or move laterally

## 5. Scope

- Contained to one host: **Yes**
- Account used for further activity after creation: **No (lab simulation)**
- Evidence of lateral movement: **No**

## 6. Response Actions Taken

- Confirmed as lab simulation
- Test account deleted: `net user labattacker /delete`
- Incident closed as True Positive — Simulated Attack

## 7. Recommendations

- In production: immediately disable the new account and the compromised parent account
- Investigate how the subject account was able to perform this action
- Review all actions taken between account creation and discovery
