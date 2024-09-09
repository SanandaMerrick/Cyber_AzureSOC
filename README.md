# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I deployed a mini honeynet in Azure, integrating log sources from multiple resources into a Log Analytics workspace. This data is leveraged by Microsoft Sentinel to create attack maps, trigger alerts, and generate incidents. I conducted an initial 24-hour analysis of security metrics in the unsecured environment, followed by the implementation of security controls inline with NIST 800-53 to enhance its defenses. A subsequent 24-hour monitoring period captured the impact of these controls. The following metrics will be presented:

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

For the "BEFORE" metrics, all resources were initially deployed and fully exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls set to allow unrestricted access, and all other resources were configured with public endpoints accessible from the internet—meaning no Private Endpoints were utilized.

For the "AFTER" metrics, Network Security Groups were hardened inline with NIST 800-53 to deny all traffic, except from my designated admin workstation. Furthermore, all other resources were fortified through a combination of built-in firewalls and the implementation of Private Endpoints for enhanced protection. 

## Attack Maps Before Hardening / Security Controls
![image](https://github.com/user-attachments/assets/8f7bfb20-6060-42bb-bb8b-f8c79cd27b23)
![image](https://github.com/user-attachments/assets/4a63c8ff-12b8-4b4a-a46a-bb6294ccb861)

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-09-01 20:27:04
Stop Time 2024-09-02 20:27:04

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 48018
| Syslog                   | 10328
| SecurityAlert            | 16
| SecurityIncident         | 383
| AzureNetworkAnalytics_CL | 1636


## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-09-08 00:41:23
Stop Time 2024-09-09 00:41:23

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8329
| Syslog                   | 37
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

### Failed Login Query After Hardening 
![image](https://github.com/user-attachments/assets/8b560cbf-da32-4f34-85a6-e168bf310252)


## Conclusion

In this project, a mini honeynet was deployed in Microsoft Azure, with log sources integrated into a Log Analytics workspace. Microsoft Sentinel was used to trigger alerts and generate incidents based on the collected logs. Security metrics were recorded both before and after the application of security controls. Notably, the number of security events and incidents significantly decreased following the implementation of these controls, highlighting their effectiveness.

It is important to mention that if the network resources had been heavily utilized by regular users, it is possible that more security events and alerts might have been generated within the 24-hour period after the security measures were applied.
