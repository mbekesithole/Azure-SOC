# Building a SOC + Honeynet in Azure (Live Traffic)
![Azure_honeynet](https://github.com/mbekesithole/Azure-SOC/assets/35656656/a67b9edc-8f29-4b23-a293-7bd6e273d3b7)


## Introduction
In this project, I constructed a miniature honeynet within the Azure framework. I collected log data from diverse sources and centralized it within a Log Analytics workspace. This aggregated data was subsequently leveraged by Microsoft Sentinel to generate attack visualizations, trigger alerts, and manage security incidents.

The methodology involved measuring various security metrics within the vulnerable environment over a period of 24 hours. Subsequently, I implemented a series of security controls to hardern the environment. Following this enhancement, another 24-hour metric measurement phase was conducted. The findings of these measurements are detailed below. The metrics to be presented include:
- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
<br>![(before)_linux_ssh_auth_fail](https://github.com/mbekesithole/Azure-SOC/assets/35656656/99dfbf16-5631-4dd0-a538-dd45d882972a)<br>
<br>![(before)_mssql_auth_fail](https://github.com/mbekesithole/Azure-SOC/assets/35656656/2306e18d-b1be-4e41-9811-6a5cb1db41da)<br>
<br>![(before)_nsg_malicious_allowed_in](https://github.com/mbekesithole/Azure-SOC/assets/35656656/7a57490d-6170-44a9-bc9f-ddbd79a87768)<br>
<br>![(before)_windows_rdp_auth_fail](https://github.com/mbekesithole/Azure-SOC/assets/35656656/a0505ad9-1bfd-4076-83ee-0cfdf08543bb)<br>


## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-03-17 21:48
Stop Time 2024-03-18 21:48

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 24250  
| Syslog                   | 1106
| SecurityAlert            | 1
| SecurityIncident         | 227
| AzureNetworkAnalytics_CL | 3417

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-03-24 12:33
Stop Time	2024-03-25 12:33

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 687
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion
Within this project, a compact honeynet was established on Microsoft Azure, with log sources seamlessly integrated into a Log Analytics workspace. The deployment of Microsoft Sentinel facilitated swift alert triggering and incident management based on the ingested logs.

Preceding the application of security measures, an initial assessment of metrics was conducted within the vulnerable environment. Following this, security controls were implemented, prompting a subsequent metric evaluation to gauge their impact.

Significantly, there was a notable reduction in both security events and incidents subsequent to the implementation of security controls, demonstrating their efficacy in bolstering system defenses.

In environments with substantial user activity, it's plausible to expect an increase in security events and alerts within the 24-hour period following the implementation of security controls.
