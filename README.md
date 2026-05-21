# MICROSOFT-SENTINEL-SOC-LAB
A hands-on Security Operations Center (SOC) home lab built on Microsoft Azure and Microsoft Sentinel, designed to simulate real-world threat detection, incident response, and threat hunting in a cloud-native environment.

## Overview
This lab replicates core SOC workflows using industry-standard tools. It serves as both a learning environment and a practical demonstration of blue team skills including SIEM configuration, KQL based detection engineering, log analysis, and incident response.
The lab is built incrementally each phase introduces new capabilities, attack simulations, and detection coverage.

Phase 1 — Honeypot Deployment & Live Attack Monitoring
Phase 2 - Threat Detection & KQL Alert Rules
Phase 3 - Incident Response Simulation
Phase 4 - Threat Hunting
Phase 5 - Automation & Playbooks

## Skills Demonstrated

Azure infrastructure deployment (VM, VNet, NSG, Resource Groups)
Log forwarding via Azure Monitor Agent and Data Collection Rules
KQL (Kusto Query Language) for detection and investigation
MITRE ATT&CK framework mapping
Microsoft Sentinel analytics rule creation and tuning
Security incident triage using a structured IR methodology
Hypothesis driven threat hunting and detection gap analysis
SOAR concepts, Logic App playbook design and automation rule implementation


## MITRE ATT&CK Coverage

| Tactic                | Technique                           | Rule or Hunt              |
|-----------------------|-------------------------------------|---------------------------|
| Credential Access     | T1110.001 Password Guessing         | Brute Force rule          |
| Credential Access     | T1110.003 Password Spraying         | Spray detection rule      |
| Initial Access        |  T1078 Valid Accounts               | Brute Force Success rule  |
| Privilege Escalation  | T1098 Account Manipulation          | Added to Admins rule      |
| Persistence           | T1078.002 Local Accounts            | Local account abuse rule  |
| Lateral Movement      | T1021 Remote Services               | New IP admin login rule   |
| Discovery             | T1087 Account Discovery             | Enumeration rule          |
| Discovery             | T1082 System Information Discovery  | Recon commands hunt       |
| Persistence           |  T1136.001 Create Local Account     | Dormant accounts hunt     |


## Phase 2  Threat Detection & KQL Analytics Rules

Five custom Sentinel analytics rules mapped to MITRE ATT&CK. Each runs on a schedule and raises incidents automatically.

| Rule                 | Severity     | Technique    |
|----------------------|--------------|--------------|
| Brute Force          | Medium       | T1110.001    |
| Brute Force Success  | High         | T1078        |
| Privilege Escalation | High         | T1098        |
| Lateral Movement     | High         | T1021        |
| Account Enumeration  | Medium       | T1087        |


## Phase 3 Incident Response Simulation

Three attack scenarios simulated end to end with structured IR reports.

| Scenario     | Type                                 | MITRES          |
|--------------|--------------------------------------|-----------------|
| Scenario A   | Credential stuffing / brute force    | T1110.001       |
| Scenario B   | Successful login after brute force   | T1078           |
|Scenario C    | Privilege escalation via admin group | T1098           |


## Phase 4 Threat Hunting

Five hypothesis driven hunts using KQL. Two detection gaps promoted to new rules.

| Hunt                      | Hypothesis                        | Outcome                | 
|---------------------------|-----------------------------------|------------------------|
| H1 — Off-hours logins     | Attackers log in at night         | Detection gap found    |
| H2 — Dormant accounts     | Backdoor accounts never used      | Gap confirmed          | 
| H3 — Password spray       | Spray missed by brute force rule  | New rule created       |
| H5 — Recon commands       | Discovery commands post-access    | Detection gap found    |

## Phase 5 — Automation & Playbooks
Two playbooks for automated incident response. Deployed via ARM templates. 

| Playbook                     | Trigger                  | Action                    |
|------------------------------|--------------------------|---------------------------|
| SOC-Notify-On-Incident       | High severity incident   | Email notification        |
| SOC-Auto-Close-Informational | Informational incident   | Auto-close + tag          |
