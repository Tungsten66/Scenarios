# Impact-NetworkDenialOfService

## Cited Resources:
[Tutorial: Azure DDoS Protection simulation testing](https://learn.microsoft.com/en-us/azure/ddos-protection/test-through-simulations) <br />

## Assumptions:

A simulated DDoS attack using a Microsoft approved testing partner was ran against the VM hosting a website on port 80.

## Validate:

Verify the following is configured
- DDoS Network Protection Plan is created and the public IP address is a protected resource - _pip-lab-demo_
- _Azure DDoS Protection_ solution is installed from the Microsoft Sentinel Content hub
   - Analytic rules are enabled
- The public ip address diagnostic settings have been configured to send DDoSProtectionNotifications, DDoSMitigationFlowLogs, and DDoSMitigationReports to the Sentinel Log Analytics workspace.


## Scenario 1:

The SOC is responding to a DDoS Attack incident in Sentinel.  Currently no services impacted.

### Questions:
- Where can you review DDoS attack incidents? 
- When did the attack start?
- What are the source IP addresses of the attack?
- Country and city source of attack?
- What is the public IP address and resource affected?
- How many incidents do you see in the last 24 hours?
- What is the attack vector?

#### Discovery
```kusto
# DDoS Protection Notifications
AzureDiagnostics
| where Category == "DDoSProtectionNotifications"

# DDoS Mitigation Reports
AzureDiagnostics
| where Category == "DDoSMitigationReports"

# DDoS Mitigation Flow Logs
AzureDiagnostics
| where Category == "DDoSMitigationFlowLogs"

# Finding the public ip
AzureDiagnostics
| where Category == "DDoSProtectionNotifications"
| project publicIpAddress_s
````


> [!Tip]
> [Azure DDoS Solution for Microsoft Sentinel](https://techcommunity.microsoft.com/t5/azure-network-security-blog/azure-ddos-solution-for-microsoft-sentinel/ba-p/3732013)  <br />
> [View Azure DDoS Protection alerts in Microsoft Defender for Cloud](https://learn.microsoft.com/en-us/azure/ddos-protection/ddos-view-alerts-defender-for-cloud)




## Scenario 2:

During a DDoS attack the performance of the protected resource is severely degraded, or the resource isn't available.

When to engage [Azure DDoS Rapid Response](https://learn.microsoft.com/en-us/azure/ddos-protection/ddos-rapid-response#when-to-engage-drr)





## Post Condition:

### Questions:
- What is your organizations current mitigation and process for DDoS attacks?
- What can be improved in this current configuration?

> [!Tip]
> [Azure DDoS Protection fundamental best practices](https://learn.microsoft.com/en-us/azure/ddos-protection/fundamental-best-practices)


