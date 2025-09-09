# Azure-Sentinel-Mini-SOC
Built an end-to-end Microsoft Sentinel detection pipeline: onboarded a Linux VM via AMA (DCR) to ingest Syslog, enabled Azure Activity via policy, wrote KQL parsers, created two Scheduled Analytics (SSH brute force &amp; Azure delete), verified incidents in Defender, and built a workbook dashboard. Includes JSON exports &amp; KQL queries.

---------

Azure Sentinel Mini-SOC (Azure Activity + Linux Syslog)

End-to-end detection pipeline built on Microsoft Sentinel:

Onboarded a Linux VM (Ubuntu) via Azure Monitor Agent (AMA) using a Data Collection Rule (DCR) to ingest Syslog

Connected Azure Activity logs to the workspace using Azure Policy (no agents needed)

Authored KQL to normalize sshd failures (parse username & source IP)

Created two Scheduled Analytics rules

SSH Brute Force (Syslog)

Azure Resource Delete (AzureActivity)

Verified alerts → incidents in the new Defender portal

Built a Workbook dashboard (Top IPs, Timeline, Account+IP) and pinned tiles to the Azure Portal dashboard

Packaged KQL and exported Analytics/Workbook JSON for version control

Portal note: This README uses the new Defender (unified) navigation. When pinning charts/tables, they go to your Azure Portal dashboard, not a Defender-only dashboard.

Architecture (high level)

Azure Activity (subscription) → Export Activity Logs → Log Analytics Workspace (LAW) ⇄ Microsoft Sentinel
Linux VM → AMA + DCR (Syslog) → LAW ⇄ Microsoft Sentinel
Sentinel → Analytics Rules → Alerts → Incidents
Sentinel/Monitor → Workbook → Azure Portal Dashboard (pinned tiles)

Prerequisites

Azure subscription with permissions:

Owner on subscription (to export Activity Logs)

Contributor on resource group(s)

Microsoft Sentinel Contributor on the workspace

A Log Analytics Workspace (e.g., law-sentinel-demo) with Microsoft Sentinel enabled

One Linux VM (Ubuntu) with outbound internet and a public IP (for demo)

(Optional) Entra ID P1/P2 for Sign-in logs; not needed for this project

Quick Start

Enable/Confirm Sentinel: Microsoft Sentinel → + Create → select your LAW (e.g., law-sentinel-demo).

Connect Azure Activity (no agent): Subscription → Activity log → Export Activity Logs →

Destination: Send to Log Analytics workspace = law-sentinel-demo

Logs: All categories → Save
(Alternatively via Sentinel’s Azure Activity connector wizard + remediation.)

Onboard Linux Syslog via AMA/DCR: Azure Monitor → Data Collection Rules → + Create

Source: add your Linux VM

Syslog data source: facilities auth, authpriv, syslog; severities Info+

Destination: LAW (law-sentinel-demo) → Create

Validate data (Logs):

AzureActivity | take 5

Syslog | take 5

Create analytics rules (below)

Create workbook (below) and pin visuals to an Azure Portal dashboard

Tuning & hardening (thresholds, allow-lists, NSG/SSH config)

KQL (ready to use)
A) SSH brute force – base query (IP only)
