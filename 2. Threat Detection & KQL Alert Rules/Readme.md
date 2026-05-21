## Phase 2 Threat Detection & KQL Alert Rules

## Objective
we write custom Microsoft Sentinel analytics rules using KQL to automatically detect attacks and raise incidents. Map every rule to MITRE ATT&CK.

Detection Rules Created

| Rule                 | MITRE Technique                        | Severity            |  Event IDs  |
|----------------------|----------------------------------------|---------------------|-------------|
| Brute Force          | T1110.001 Password Guessing            | Medium              |  4625       |
| Privilege Escalation | T1098 Account Manipulation             | High                |  4732       |
| Account Enumeration  | T1087 Account Discovery                | Medium              |  4625       |

How Sentinel Analytics Rules Work
Each rule runs a KQL query on a schedule. If the query returns rows, Sentinel creates an alert and groups it into an incident.

Scheduled KQL query runs every N minutes

       |
       v
Results returned?

       |
      YES --> Alert created --> Incident opened --> Analyst notified
      
Entity mapping tells Sentinel which columns represent security entities (IPs, accounts, hosts).
This powers the investigation graph and cross-incident correlation.
 
 Key Event IDs

| Event ID    |   Description                             |
|-------------|-------------------------------------------|
| 4625        | Failed logon attempt                      |
| 4624        | Successful logon                          |
| 4720        | User account created                      |
| 4688        | Process creation (requires audit policy)  |

MITRE ATT&CK Coverage After Phase 2

| Tactic               | Technique        | Rule                |
|----------------------|------------------|---------------------|
| Credential Access    | T1110.001        | Brute Force         |
| Credential Access    | T1110.003        | Password Spray      |
| Initial Access       | T1078            | Brute Force Success |
| Privilege Escalation | T1098            | Added to Admins     |
| Persistence          | T1078.002        | Local Account Abuse |
| Lateral Movement     | T1021            | Admin Login New IP  |
| Discovery            | T1087            | Account Enumeration |
