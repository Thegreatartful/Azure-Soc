
# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I built a mini-honeynet in Azure and ingested logs from various resources into a Log Analytics workspace. This information was then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured security metrics in an insecure environment for 24 hours. Security controls were appllied to harden the environment. After which, a second measurement was captured for an additional 24 hours. The results of these findings are shown below. 

To Include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into the honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini-honeynet in Azure consisted of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, initial resources were deployed and exposed to the internet as a baseline. The Virtual Machines had both of their Network Security Groups (including built-in firewalls) "wide open". All other resources were deployed with public endpoints visible to the Internet. Private endpoints were not used.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic, with the exception of my admin workstation. All other resources were protected by their built-in firewalls as well as private endpoints.

## Attack Maps Before Hardening / Security Controls
Respectively, from the top image down: 1. mssql-auth-fail-allowed-in; 2. nsg-malicious-allowed-in; 3. rdp-authfail-windows; 4. syslog-ssh-auth-fail.
![(bfr)mssql-auth-fail-allowed-in](https://github.com/Thegreatartful/Azure-Soc/assets/139084546/e73e7997-3e4a-4578-9061-23b2afdaf451)<br>
![(bfr)nsg-malicious-allowed-in](https://github.com/Thegreatartful/Azure-Soc/assets/139084546/67f8d5d1-69cd-405f-ba03-8092ca380c79)<br>
![(bfr)rdp-authfail-windows](https://github.com/Thegreatartful/Azure-Soc/assets/139084546/61fbb053-4028-4bed-aceb-47ab31f96ae6)<br>
![(bfr)syslog-ssh-suth-fail](https://github.com/Thegreatartful/Azure-Soc/assets/139084546/90022400-54b8-4d80-aa52-5c11329691e2)<br>


## Metrics Before Hardening / Security Controls

The following table shows the metrics measured in the insecure environment for 24 hours:
Start Time 2023-07-25 23:25 EST
Stop Time 2023-07-26 23:25 EST

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 15312
| Syslog                   | 4595
| SecurityAlert            | 1
| SecurityIncident         | 2
| AzureNetworkAnalytics_CL | 0

## Attack Maps after Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```
(Maps cannot be generated without malicious activity. Imagine a map with NO hotspots in the same / previous aforementioned categories as the "Before Hardening / Security Controls attack maps.)

## Metrics After Hardening / Security Controls

The following table shows the metrics measured in the environment for another 24 hours, but WITH applied security controls:
Start Time 2023-07-26 23:40 EST
Stop Time	2023-07-27 23:40 EST

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 0
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini-honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced, to the point of being nonexistant, after the security controls were applied, demonstrating their effectiveness. 

Additionally, as more time passes with the security measures in place, it is likely that more security events and alerts will be generated (threat actors will require a greater effort to effect them). Also, if the resources within the network were heavily utilized by regular users, more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls, as well.
