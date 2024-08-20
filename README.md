
# Building a SOC + Honeynet in Azure (Live Traffic)
![image](https://github.com/user-attachments/assets/a3cffafd-3d6e-4502-9c5f-87de6dabb7f9)


## Introduction

In this project, I designed and implemented a mini honeynet in Azure, where I ingested log sources from various resources into a Log Analytics workspace. This workspace was then integrated with Microsoft Sentinel to build attack maps, trigger alerts, and create incidents.
To evaluate the security of the environment, I measured specific security metrics in an intentionally insecure environment for a 24-hour period. Afterward, I applied a series of security controls to harden the environment and conducted a second 24-hour measurement to assess the effectiveness of the controls. The security metrics used to measure the environment's performance are detailed below.


- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://github.com/user-attachments/assets/59a1aa75-f40b-4e69-90e1-4bfa7cec9837)
## Architecture After Hardening / Security Controls
![Architecture Diagram](https://github.com/user-attachments/assets/5132295f-08c2-4103-9c43-8c89aa2a1f33)
)

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
![NSG Allowed Inbound Malicious Flows]![image](https://github.com/user-attachments/assets/6308ed92-9698-4395-84be-19dc238c10f5)
<br>
![Linux Syslog Auth Failures](![image](https://github.com/user-attachments/assets/2a8a7f5c-0686-44d7-aceb-4787caad8dcd)
<br>
![Windows RDP/SMB Auth Failures](![image](https://github.com/user-attachments/assets/d00028a1-4828-4978-93de-3875435023da)
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-08-07 00:43:18
Stop Time 2024-08-08 00:43:18

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 95394
| Syslog                   | 3165
| SecurityAlert            | 4
| SecurityIncident         | 494
| AzureNetworkAnalytics_CL | 891

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-08-13 07:14
Stop Time	2024-08-14 07:14

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 11850
| Syslog                   | 5
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure, with log sources integrated into a Log Analytics workspace. Microsoft Sentinel was then provisioned to trigger alerts and create incidents based on the ingested logs. Security metrics were measured in the insecure environment before and after implementing security controls. The results showed a significant reduction, and in some cases, the complete elimination of security events and incidents, demonstrating the effectiveness of the applied controls.
It's important to note that if the resources within the network had been heavily utilized by regular users, it is likely that more security events and alerts would have been generated during the 24-hour period following the implementation of the security controls.
