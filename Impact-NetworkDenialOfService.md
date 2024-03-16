# Impact-NetworkDenialOfService

## Cited Resources:
[Tutorial: Azure DDoS Protection simulation testing](https://learn.microsoft.com/en-us/azure/ddos-protection/test-through-simulations) <br />

## Assumptions:

A simulated DDoS attack using a Microsoft approved testing partner was ran against the VM hosting a website on port 80.

## Validate:

Verify the following is configured
- DDoS Network Protection is enabled on the public IP address - _pip-lab-demo_
- Azure DDoS Protection solution is installed from the Sentinel Content hub
- The public ip address diagnostic settings have been configured to send DDoSProtectionNotifications, DDoSMitigationFlowLogs, and DDoSMitigationReports to the Sentinel Log Analytics workspace.


## Scenario 1:

The SOC is responding to a DDoS Attack incident in Sentinel.  Currently no services impacted.

- [Azure DDoS Solution for Microsoft Sentinel](https://techcommunity.microsoft.com/t5/azure-network-security-blog/azure-ddos-solution-for-microsoft-sentinel/ba-p/3732013)
- [View Azure DDoS Protection alerts in Microsoft Defender for Cloud](https://learn.microsoft.com/en-us/azure/ddos-protection/ddos-view-alerts-defender-for-cloud)

## Scenario 2:

Service is starting to be impacted.

When to engage [Azure DDoS Rapid Response](https://learn.microsoft.com/en-us/azure/ddos-protection/ddos-rapid-response#when-to-engage-drr)

## Discovery

DDoS Protection Notifications

```kusto
AzureDiagnostics
| where Category == "DDoSProtectionNotifications"
```

DDoS Mitigation Reports

```kusto
AzureDiagnostics
| where Category == "DDoSMitigationReports"
```

DDoS Mitigation Flow Logs

```kusto
AzureDiagnostics
| where Category == "DDoSMitigationFlowLogs"
```



## Post Condition:

Reflect on the following questions:
- What is your organications current process for DDoS attacks?

> [!Note]
> Hint: [Azure DDoS Protection fundamental best practices](https://learn.microsoft.com/en-us/azure/ddos-protection/fundamental-best-practices)

Add description here

1. Under Sentinel > Analytics - Create a Scheduled query rule
2.
3. Review new incident


