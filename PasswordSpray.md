# Password Spray walkthrough

## Cited Resources:
[Microsoft Security Blog: Protecting your organization against password spray attacks](https://www.microsoft.com/en-us/security/blog/2020/04/23/protecting-organization-password-spray-attacks/) <br />
[Error code lookup tool](https://login.microsoftonline.com/error)

## Assumptions:

The Azure Active Directory solution is installed from the Sentinel Content hub <br />
The Azure Active Directory Diagnostics settings have been configured to send SignInLogs to the Sentinel Log Analytics workspace


## Instructions:

> [!NOTE]
> Prior to this session the instructors attempted to log in with a demo account 10 times with the wrong password
1. Using one of the demo user accounts provided login to [Microsoft Entra admin Center](https://entra.microsoft.com/)
2. In the Microsoft Entra admin center go to Sign-in events by clicking on Identity > Monitoring & health > Sign-in logs
![2](https://github.com/Tungsten66/Scenarios/assets/40893034/f5a75274-e60d-4779-8e58-1ad3080dba06)
3. To find the user, click on Add filters, select Username and click Apply.<br />
![3](https://github.com/Tungsten66/Scenarios/assets/40893034/d8b5ea9c-4eb1-4482-bb3f-cf10962640b8)
4. Type the username into the field and click Apply <br />
![4](https://github.com/Tungsten66/Scenarios/assets/40893034/4d41ced7-31d6-420b-aab4-837505a4a6bb)
5. See all the Failures for the demo user
![5](https://github.com/Tungsten66/Scenarios/assets/40893034/f2e7dfab-6f35-432c-9bef-39837d75ed16)
6. Click on one of the failures and capture the Sign-in error code
![6](https://github.com/Tungsten66/Scenarios/assets/40893034/4c83594c-5dab-44e0-bdc1-d80a66612388)
7. Login to [Azure Portal](https://portal.azure.com/)
8. Go to Sentinel, select Logs, and close the first window that appears  
![8](https://github.com/Tungsten66/Scenarios/assets/40893034/8c456be4-2deb-442b-9d1d-84ed3f5de4c6)

9. Write the KQL query to search for users that have tried to login into the Azure Portal five or more times using the previously captured Sign-in error code 
```console
SigninLogs
| where AppDisplayName == "Azure Portal"
| where ResultType == "50126"
| summarize count() by UserPrincipalName
| where count_ > 4
```
10. Run the query and review the results <br />
![10](https://github.com/Tungsten66/Scenarios/assets/40893034/d12eeb5f-2040-4288-b983-a94e572da576)



## Post Condition:

> [!NOTE]
> Instructor led

Generate an incident in Sentinel by creating an analytic rule to generate an incident when the above indicator of compromise occurs.

1. Under Sentinel > Analytics - Create a Scheduled query rule
![1-1](https://github.com/Tungsten66/Scenarios/assets/40893034/ceba4c78-491c-4cad-8080-4b469ce09e23)
2. Paste in the KQL into the Rule Query, update the Entity mapping, and update your Query scheduling
![1-2](https://github.com/Tungsten66/Scenarios/assets/40893034/a8622909-7bed-4237-83bd-2507249420a6)
![1-2-1](https://github.com/Tungsten66/Scenarios/assets/40893034/93bab63f-af0f-455f-bf22-f3591dfa9d0d)
3. Review new incident
![1-3](https://github.com/Tungsten66/Scenarios/assets/40893034/0a9f4e3c-6cdc-46ea-8393-6d2376db7ea7)

