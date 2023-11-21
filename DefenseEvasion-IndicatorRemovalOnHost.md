# Defense Evasion – Indicator Removal on Host

## Cited Resources:
[Microsoft: 1102(S): The audit log was cleared](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/event-1102) <br />

## Assumptions:
Azure Monitoring Agent extension is installed on the VM and a Data Collection Rule was created to send Windows Security logs to the Log Analytics Workspace.
Something malicious has been done on a Windows client and the Security Event log was cleared to hide the actions.<br />
The Windows Security Events solution is installed from the Sentinel Content hub

## Instructions:

Walkthrough to find if the Windows event logs was cleared.

1. Go to [Azure Portal](https://portal.azure.com/) and Search for Microsoft Sentinel
![1-1](https://github.com/Tungsten66/Scenarios/assets/40893034/b6499c4f-f7a0-4ddb-96bb-77cd4a162458)
2. Click on your desired Sentinel Instance
![1-2](https://github.com/Tungsten66/Scenarios/assets/40893034/20813e65-cd76-4909-8018-895ea33c4b92)
3. Click on the Logs blade and click on the X to close the Queries popup.
![image](https://github.com/Tungsten66/Scenarios/assets/40893034/1ea1559d-7478-4d0d-a4a7-9a5e4c7592a9)
4. Write the KQL to look for the Windows Event ID that indicates the Event Log has been cleared.
```console
SecurityEvent
| where EventID == 1102 and EventSourceName == "Microsoft-Windows-Eventlog"
```
5. Run The query and review the results.  In the results look at the Channel, EventID, and Activity.
![image](https://github.com/Tungsten66/Scenarios/assets/40893034/8e380c8e-3f90-4ecc-9859-c7523010208a)

## Post Condition:

> [!NOTE]
> Instructor led

Generate an incident in Sentinel when the above indicator of compromise occurs.

In Microsoft Sentinel

1. Click Analytics blade
2. Click Rule Templates
3. Type NRT Security Event log cleared
4. Click on the Rule to highlight it
5. Click Create Rule
![image](https://github.com/Tungsten66/Scenarios/assets/40893034/4bd7549f-4779-4bc6-bf05-46e2c0ec7caa)
6. Take default settings when creating the rule and Save
7. Review the new Incident created by navigating to the Incidents blade
![image](https://github.com/Tungsten66/Scenarios/assets/40893034/bb73f836-ee2c-4b70-844d-260d842be495)



