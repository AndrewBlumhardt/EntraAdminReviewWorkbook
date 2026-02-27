# Entra Admin User Review Workbook

Workbook for identifying, reviewing, and correlating administrative user accounts in Microsoft Entra ID using Microsoft Sentinel sign-in, audit, and Azure activity logs. This workbook is designed to help organizations audit privileged accounts, validate activity, and identify potential contact user accounts for admin identities that do not have mailboxes.

## Why is this useful?

It is common practice to separate standard user accounts from privileged admin accounts. Users perform daily work with one account and use a separate admin identity only for elevated tasks. This reduces risk and supports stronger auditing and conditional access controls.

Problems arise when the admin account is not clearly linked to its primary user. Because admin accounts often lack mailboxes and may not list a contact owner in their properties, it can be difficult to determine who actually uses them.

This workbook supports admin activity review through sign-in, PIM, and Azure activity data, while also helping identify likely primary user accounts based on common naming patterns. It provides a practical way to improve visibility when admin accounts are not formally mapped to their owners.

![Workbook Sample](https://github.com/AndrewBlumhardt/EntraAdminReviewWorkbook/blob/main/workbookSample.png)

---

## Download

Import the workbook JSON file into Azure Monitor or Microsoft Sentinel:

AzureAdminReviewWorkbook.json :contentReference[oaicite:0]{index=0}

---

## Overview

Administrative accounts frequently:

- Follow naming conventions such as `first.last.admin@domain.com`
- Do not have an associated mailbox
- Are used only for privileged operations
- May not be centrally tracked for contact purposes

This workbook:

- Identifies active admin accounts based on sign-in activity  
- Correlates admin accounts to likely standard user accounts  
- Surfaces sign-in, PIM, and Azure activity history  
- Provides a downloadable list for remediation or contact updates  

Results are based on Microsoft Entra sign-in logs. Admin accounts without activity during the selected time range will not appear.

---

## Workbook Tabs

### Admin Review

Provides interactive dashboards including:

- Most recent successful admin sign-in  
- Sign-in history table and time chart  
- Location, device, OS, browser, and IP details  
- Potential correlated non-admin user account  
- PIM activation history  
- Azure activity history  

### Admin User Lookup

Provides a downloadable table showing:

- Admin display name and UPN  
- Correlated user accounts based on naming match  
- Count of potential matches  
- Sample correlated UPN  

Designed for export to Excel to support bulk review or update processes.

---

## Requirements

- Microsoft Sentinel enabled  
- Entra sign-in logs ingested into Log Analytics  
- AuditLogs table available for PIM history  
- AzureActivity table available for subscription actions  
- Reader access to the selected Log Analytics workspace  

---

## Deployment Instructions

### 1. Download the Workbook File

Download the JSON file:

AzureAdminReviewWorkbook.json :contentReference[oaicite:1]{index=1}

### 2. Import into Azure

1. Navigate to Microsoft Sentinel or Azure Monitor.  
2. Select **Workbooks**.  
3. Choose **+ New**.  
4. Open **Advanced Editor**.  
5. Switch to **JSON** view.  
6. Paste the contents of `AzureAdminReviewWorkbook.json`.  
7. Save the workbook.  

### 3. Configure Parameters

The workbook includes the following parameters:

- **Workspace** – Select your Log Analytics workspace  
- **TimeGenerated** – Choose the analysis time range  
- **Search Term** – Default is `.admin@`, adjust if needed  
- **Show Help** – Optional overview guidance  

If your admin naming convention differs significantly, update the KQL parsing logic accordingly.

---

## Naming Convention Assumptions

The default logic assumes:

- Admin accounts contain `.admin` in the UPN  
- First and last names are separated by a period  
- Standard user accounts do not contain the word `admin`  

If your organization uses different patterns such as `adm-firstlast`, `firstlast-adm`, or a separate admin domain, the KQL queries should be adjusted.

---

## Security Considerations

- The workbook reflects only activity present in the selected log retention window.  
- Dormant admin accounts without recent sign-in activity will not appear.  
- Correlated user accounts are based on naming similarity, not authoritative identity mapping.  
- Results should be validated before performing bulk updates.

---

## Cost Considerations

The workbook does not deploy additional Azure resources. Costs are limited to:

- Log ingestion  
- Log retention  
- Query execution within Log Analytics  

---

## Use Cases

- Quarterly privileged account review  
- Admin contact remediation  
- PIM activity validation  
- Azure activity auditing  
- Governance and compliance reporting  

---

## License

Provided as-is for operational and community use. Review and validate queries prior to production deployment.
