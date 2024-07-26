
# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet + SOC](https://github.com/user-attachments/assets/96eb79d6-8336-4063-8eb2-d71a7cbc2b20)


## Introduction

In this project, I created a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace. This data is utilized by Microsoft Sentinel to generate attack maps, trigger alerts, and create incidents. Initially, I measured security metrics in the unsecured environment over a 24-hour period. After applying security controls to enhance the environment's security, I measured the metrics again for another 24 hours. The results are presented below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Before Hardening](https://github.com/user-attachments/assets/2f28f32f-42f6-4ae9-941f-c8825163b602)


## Architecture After Hardening / Security Controls
![Architecture After Hardening](https://github.com/user-attachments/assets/91a91b7e-3b9d-4ec8-833a-45f3ffae235b)

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
![nsg-malicious-allowed-in](https://github.com/user-attachments/assets/d9374df2-2979-4e36-b5e7-15fcac9f1966)
![linuz-syslog-ssh-auth-fail](https://github.com/user-attachments/assets/c93555b4-6b10-4ff0-8233-8710fa76efb9)
![windows-rdp-auth-fail](https://github.com/user-attachments/assets/b8ecee9e-f3d0-4dd9-a753-6aa46b1bad89)

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-07-23 7:20:16
Stop Time 2024-07-24 7:20:16

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 35735
| Syslog                   | 5092
| SecurityAlert            | 0
| SecurityIncident         | 268
| AzureNetworkAnalytics_CL | 1834

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-07-25 8:16:38
Stop Time	2024-07-26 8:16:38

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 14148
| Syslog                   | 44
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
