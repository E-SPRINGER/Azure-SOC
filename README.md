# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.ibb.co/z7K8LYz/updated-MINI-SOC.png)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics I will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)



## Technologies, Azure Components, and Regulations Used

-	Azure Virtual Network (VNet)
-	Azure Network Security Groups (NSG)
-	Virtual Machines (Windows VM and Linux VM)
-	Log Analytics Workspace with Kusto Query Language (KQL) Queries
-	Azure Key Vault for Secure Secrets Management
-	Azure Storage Account for Data Storage
-	Microsoft Sentinel for Security Information and Event Management (SIEM)
-	Microsoft Defender for Cloud to Protect Cloud Resources
-	Windows Remote Desktop for Remote Access
-	[NIST SP 800-53 Revision 5](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final) For security controls.

-	[NIST SP 800-61 Revision 2](https://www.nist.gov/privacy-framework/nist-sp-800-61) For incident handling.




## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.ibb.co/nMvPxmV/Architecture-Before-Hardening-Security-Controls.jpg)

The purpose of this phase was to monitor the attack patterns utilized by malicious actors in a "real-world" setting.

This phase began with the deployment of the virtual environment and its subsequent exposure to the public internet with minimal security controls. The use of minimal security controls in this phase allowed malicious actors to discover the environment and attempt a variety of cyber attacks. The environment consisted of a Windows 10 Virtual Machine hosting a SQL database, a Linux Virtual Machine, a blob storage account, and a key vault. The purpose of having all of these components was to increase the attack surface and could monitor the different methods used by attackers.

In order to guarantee the environment would be discoverable, the virtual machines had firewalls disabled and the network security groups (NSGs) were configured to allow all inbound traffic. The storage account and key vault were deployed with public endpoints visible to the internet. After the creation of the environment, Log Analytics workspace was set up for log aggregation and Microsoft Sentinel was in turn used for incident creation. Lastly four workbooks with custom queries were created in Microsoft Sentinel to plot the recorded malicious activity on a world map, with the purpose of providing a visual aid for the attacks.

The results of these real-world attacks can be seen in the following section.



## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.ibb.co/F5qpGJ0/Architecture-After-Hardening-Security-Controls.jpg)

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
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-12-03 17:04:29
Stop Time 2023-12-04 17:07:18

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 25283
| Syslog                   | 19692 (Linux VM)
| SecurityAlert            | 5 (Windows Defender for Cloud)
| SecurityIncident         | 297
| AzureNetworkAnalytics_CL | 2782

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-12-20 15:37
Stop Time	2023-12-21 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 10332
| Syslog                   | 4
| SecurityAlert            | 1
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
