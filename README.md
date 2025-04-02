# Azure-SocProject
# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

This project demonstrates the construction of a miniature honeynet within Microsoft Azure. It involves integrating log data from diverse Azure resources into a Log Analytics workspace. This workspace subsequently feeds Microsoft Sentinel, which is utilized for generating attack visualizations, triggering security alerts, and creating incident tickets. Baseline security metrics were captured over a 24-hour period within the initial, unsecured environment. Following this, security controls were implemented to harden the infrastructure, and metrics were recorded for an additional 24 hours post-hardening. The comparative results are presented below. The key metrics tracked include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows permitted into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The Azure honeynet architecture comprises the following core components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 Windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the 'BEFORE' metrics (baseline), all resources were initially deployed with direct exposure to the public internet. The Virtual Machines operated with permissive Network Security Group (NSG) rules and disabled host-based firewalls. Furthermore, other Azure services utilized publicly accessible endpoints, deliberately avoiding Private Endpoints for this initial phase.

For the 'AFTER' metrics (post-hardening), NSGs were configured restrictively, blocking all inbound traffic except from a designated administrative workstation. Additionally, host-based firewalls on the VMs were enabled, and other Azure services were secured using their built-in firewall capabilities and transitioned to utilize Private Endpoints.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The table below presents the security metrics collected over a 24-hour period in the unsecured environment:
Start Time 2025-04-01 17:04:29
Stop Time 2025-04-01 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 15294
| Syslog                   | 2095
| SecurityAlert            | 10
| SecurityIncident         | 342
| AzureNetworkAnalytics_CL | 842

## Attack Maps After Hardening / Security Controls

```Map queries executed during the post-hardening phase yielded no results, indicating the absence of detected malicious activity within this 24-hour observation window.```

## Metrics After Hardening / Security Controls

This table displays the metrics gathered during the subsequent 24-hour period, following the implementation of security controls:
Start Time 2025-04-01 15:37
Stop Time	2025-04-01 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8781
| Syslog                   | 32
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

This project successfully demonstrated the setup of a miniature honeynet in Microsoft Azure, channeling log data into a Log Analytics workspace. Microsoft Sentinel leveraged this data effectively to generate security alerts and incidents. Comparative security metrics were captured before and after the application of hardening measures. Significantly, the volume of security events, alerts, and incidents decreased dramatically post-hardening, clearly illustrating the effectiveness of the implemented security controls.

It should be considered that if the network resources had experienced substantial legitimate user activity during the post-hardening observation period, it is plausible that a higher count of security events (potentially including benign alerts) might have been generated.
