# Phase 5  Automation & Playbooks

## Objective
Design and document automated response playbooks using Azure Logic Apps and native Sentinel Automation Rules. Understand the SOAR (Security Orchestration, Automation and Response) workflow and implement what is available on the free subscription tier.

## Subscription Note
Logic Apps (Consumption plan) requires a Pay As You Go subscription. On a free Azure subscription, the two playbooks below are fully designed and documented with ARM templates provided for deployment. The native Sentinel Automation Rules (which require no Logic App) were fully implemented and are live.

---

# Native Sentinel Automation Rules

These rules run directly inside Microsoft Sentinel with no Logic App required.
Fully functional on free Azure subscriptions.

---

## Rule 1: Auto-Close Informational Incidents

**Location in Sentinel:** Configuration > Automation > Automation rules

| Setting | Value |
| --- | --- |
| Name | Auto-Close Informational Incidents |
| Trigger | When incident is created |
| Condition | Incident severity Equals Informational |
| Action 1 | Change status → Closed |
| Action 2 | Change classification → Benign Positive |
| --- | --- |
| Order | 1 (runs first) |

**Purpose:** Filters out low-value informational events so analysts focus on Medium and High severity incidents.

---

## Rule 2: Auto-Assign High Severity Incidents

| Setting | Value |
| --- | --- |
| Name | Auto-Assign High Severity to Analyst |
| Trigger | When incident is created |
| Condition | Incident severity Equals High |
| Action 1 | Assign owner > send email/notifies owner |
| --- | --- |
| Order | 2 |

**Purpose:** Ensures no High severity incident sits in an unassigned state. Immediate ownership assignment for SLA compliance.

---

## How to Create These Rules

1. In Sentinel, go to **Configuration** > **Automation**
2. Click **+ Create** > **Automation rule**
3. Fill in the settings from the tables above
4. Click **Apply**

The rule is immediately active — no deployment or connection authorization needed.


---

## Documented Playbooks (ARM Templates)

### Playbook 1: Email Notification on High Severity
**File:** [playbooks/SOC-Notify-On-Incident.json](playbooks/SOC-Notify-On-Incident.json)

**Workflow:**
```
Sentinel incident created
       |
       v
Condition: Severity == High?
       |
      YES
       |
       v
Send email:
  To: analyst@company.com
  Subject: [HIGH ALERT] {incident title}
  Body: severity, status, time, description, portal link
```

**Trigger:** Sentinel incident created
**Connector required:** Office 365 Outlook or Gmail


---

### Playbook 2: Auto-Block Attacker IP in NSG
**File:** [playbooks/SOC-Block-IP-In-NSG.json](playbooks/SOC-Block-IP-In-NSG.json)

**Workflow:**
```
Sentinel incident created (Brute Force rule)
       |
       v
Parse incident entities — extract IP addresses
       |
       v
For each IP entity:
       |
       v
Create NSG inbound DENY rule for that IP
       |
       v
Add comment to Sentinel incident:
  "Automated response: IP {x.x.x.x} blocked in NSG"
```

**Trigger:** Sentinel incident created, filtered to rule "Brute Force"
**Connector required:** Azure Resource Manager
**Permission required:** Logic App Managed Identity with Contributor on resource group



### Playbook 3: Auto-Close Informational (Logic App version)
**File:** [playbooks/SOC-Auto-Close-Informational.json](playbooks/SOC-Auto-Close-Informational.json)


---

## How to Deploy the Playbooks 

In azure CLI, login 

# Deploy a playbook ARM template
az deployment group create \
  --resource-group SOC-Honeypot-RG \
  --template-file playbooks/SOC-Notify-On-Incident.json

# After deployment, authorize the connections in the Logic App designer
# Then attach to Sentinel: Configuration > Automation > + Automation Rule
```

---

## SOAR Concepts Demonstrated

| Concept | Implementation |
| --- | --- |
| Alert-driven trigger | Sentinel incident creation fires the playbook |
| Entity extraction | Parse IP entities from incident JSON |
| Automated containment | Block IP in NSG without analyst action |
| Analyst notification | Email with incident context on High severity |
| --- | --- |
| Queue management | Low-value incidents auto-closed to reduce noise |
| Idempotency | NSG rule check before creating duplicate rules |

---

