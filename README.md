# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

For this project, I constructed a small honeynet within Azure and collected log data from diverse sources into a Log Analytics workspace. This workspace is subsequently utilized by Microsoft Sentinel to construct attack maps, activate alerts, and generate incidents. I conducted security metric assessments within the vulnerable environment for a duration of 24 hours, implemented security measures to fortify the environment, and conducted another 24-hour metric assessment. The metrics will be based on:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

In the "BEFORE" metrics assessment, all resources were exposured to the internet. The Virtual Machines had their Network Security Groups and built-in firewalls configured with broad access, while all other resources were deployed with public endpoints accessible to the internet, thus eliminating the need for Private Endpoints.

In the "AFTER" metrics analysis, Network Security Groups were hardened by restricting ALL traffic except for access from my admin workstation. Additionally, all other resources were shielded by both their built-in firewalls and Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://github.com/tayahd/Azure-SOC/assets/162353295/6e72cda0-0444-408d-b11f-80836f016743)
<br>
![Linux Syslog Auth Failures](https://github.com/tayahd/Azure-SOC/assets/162353295/e5544561-ff10-4b00-b2c1-707f05cf104c)
<br>
![Windows RDP/SMB Auth Failures](https://github.com/tayahd/Azure-SOC/assets/162353295/94ec82a1-7977-40e6-b11c-daece4a42ee4)
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 3/3/2024 21:25:19
Stop Time 3/4/2024 21:25:19

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 31492
| Syslog                   | 11241
| SecurityAlert            | 4
| SecurityIncident         | 210
| AzureNetworkAnalytics_CL |2236

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to little to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 43326
| Syslog                   | 1
| SecurityAlert            | 1
| SecurityIncident         | 9
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security 
