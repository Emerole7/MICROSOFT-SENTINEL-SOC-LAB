## Azure SOC Honeypot Lab showing Attack map

A hands on Azure security lab that deploys an intentionally exposed Windows VM (honeypot), forwards Windows Security Events to Microsoft Sentinel via the Azure Monitor Agent, enriches login attempts with geographic data, and visualizes global attack origins on a live attack map.

## Architecture

Internet Attackers
       │
       ▼
 NSG (allow all inbound)
       │
       ▼
 Windows 10 VM  ←── firewall disabled
       │
       │  Azure Monitor Agent (AMA)
       │  Data Collection Rule (DCR)
       ▼
 Log Analytics Workspace
       │
       ▼
 Microsoft Sentinel (SIEM)
       │
       ├── GeoIP Watchlist (54,000 IP ranges → lat/lon/country)
       │
       ▼
 Attack Map Workbook  (KQL + map visualization)

## Prerequisites
Azure subscription (free tier or paid)
Basic familiarity with the Azure portal
RDP client to connect to your VM

## Lab Parts
Part1: Create Azure subscription
Part2: Deploy the honeypot VM
Part3: Simulate attacks and inspect Event Viewer
Part4: Forward logs to Sentinel via AMA
Part5: Enrich logs with GeoIP watchlist
Part6: Build the live attack map workbook

## Quick Reference — KQL Queries
All queries are in the queries/ folder.

Basic failed login query:
kqlSecurityEvent
| where EventID == 4625

GeoIP-enriched attack map query:
kqllet GeoIPDB_FULL = _GetWatchlist("geoip");
let WindowsEvents = SecurityEvent
    | where EventID == 4625
    | order by TimeGenerated desc
    | evaluate ipv4_lookup(GeoIPDB_FULL, IpAddress, network);
WindowsEvents




