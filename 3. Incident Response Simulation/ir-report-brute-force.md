# IR Report — Brute Force / Credential Stuffing

| Field | Value |
| --- | --- |
| Date | May 20th, 2026 |
| Analyst | Prosper |
| Incident ID | 55 |
| Severity | Medium |
| Status | Closed |
| Classification | True Positive — Suspicious Activity |

## 1. Summary
A brute force attack was detected against the honeypot VM. A single external IP address generated over 10 failed RDP login attempts within a 10-minute window, targeting multiple usernames. This matches automated credential stuffing tool behavior.

## 2. Detection
- **Rule triggered:** Brute Force — Multiple Failed Logins from Single IP
- **Alert time:** May 20th, 2026 5:45:26 PM EST
- **MITRE technique:** T1110.001 — Password Guessing

## 3. Evidence

- **Source IP:** 5.181.86.80
- **Target host:** PAYROLL-DB-EAST
- **Accounts targeted:** administrator, admin, employee, user, guest, service, backup, sysadmin, test, support
- **Event IDs observed:** 4625 (repeated)
- **Total failed attempts:** 57

**Timeline:**
- May 20th 2026, 5:41:17 PM EST — First failed login attempt observed
- May 20th 2026, 5:46:03 PM EST — 57th failed login attempt crosses threshold
- May 20th, 2026 5:45:26 PM EST — Alert fired in Sentinel
- May 20th, 2026 5:45:26 PM EST — Incident created and assigned

## 4. Investigation Findings

Pivot query run against the source IP confirmed all activity was failed login attempts (4625) only. No successful login (4624) from this IP was observed in the lookup window. No subsequent activity from this IP on other ports or systems.

Attack pattern consistent with an automated credential stuffing tool sequential username list, consistent timing intervals between attempts (~2-3 seconds per attempt).

## 5. Scope

- Contained to one host: **Yes**
- Evidence of successful login from attacker IP: **No**
- Evidence of lateral movement: **No**
- Evidence of data exfiltration: **No**

## 6. Response Actions Taken

- Incident reviewed and triaged in Sentinel
- Source IP noted for threat intelligence
- No blocking action taken (honeypot — blocking defeats the purpose)
- Incident closed as True Positive — Suspicious Activity

## 7. Detection Gaps Identified

- No alert if the attacker slows attempts below 10/10min threshold (low and slow brute force)
- No alert for password spray pattern (addressed in Phase 4 — new rule T1110.003 created)

## 8. Recommendations

- Add Password Spray rule (T1110.003) to detect low volume multi account attacks
- Consider off hours login alert (T1078) for production environments
