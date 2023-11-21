# Initial Access – Valid Accounts

## Cited Resources:
[Microsoft: Short-lived accounts](https://learn.microsoft.com/en-us/entra/architecture/security-operations-user-accounts#short-lived-accounts) <br />

## Assumptions:
An account was created and deleted in under 24 hours.
The Azure Active Directory solution is installed from the Sentinel Content hub.

## Instructions:

Walkthrough to find accounts that have been created and deleted within 24 hours

1. Go to [Microsoft Entra admin center](https://entra.microsoft.com/)
2. Expand Users, click on All Users, and click on Audit logs <br />
![image](https://github.com/Tungsten66/Scenarios/assets/40893034/ff976f54-fe19-449c-99ea-7a5628d8546b)
3. Click on the Date, Click on Custom time interval, enter the start and end to equal the last 24 hours and click Apply
![image](https://github.com/Tungsten66/Scenarios/assets/40893034/dc7b5b9e-7fe4-48ce-8d45-9e9b056f6773)
4. Click on the Activity filter and type Add User, click the radio button next to Add User and click Apply to view all users that have been created <br />
![image](https://github.com/Tungsten66/Scenarios/assets/40893034/3c06db82-0d3f-42ae-894a-0d39ce897870)
5. If the list is small you can choose to manaully take note of the users created or you can download them by clicking on download
![image](https://github.com/Tungsten66/Scenarios/assets/40893034/7d7d9adf-898f-4b07-acac-efdf55af31f4)
6. Click back on the Activity filter and type Delete user, click on the radio button to select Delete user, and click Apply
![image](https://github.com/Tungsten66/Scenarios/assets/40893034/b1a10da5-3740-459b-b721-3b2308691fe5)
7. Again, if the list is small you can choose to manaully take note of the users deleted or you can download them by clicking on download
![image](https://github.com/Tungsten66/Scenarios/assets/40893034/292072dd-e1fc-4718-a81e-60c5c7e5bde5)
8. Now perform your review and looks for any accounts that have been created and deleted in this time frame.
9. If you want  to further investigate and of the events click on it and get additional details like IP address and the User Principal Name that performed the action. <br />
![image](https://github.com/Tungsten66/Scenarios/assets/40893034/15e070e9-ec95-41ca-958c-45ccaf203ad3)

## Post Condition:

> [!NOTE]
> Instructor led

Generate an incident in Sentinel when the above indicator of compromise occurs.

In Microsoft Sentinel

1. Click Analytics blade
2. Click Rule Templates
3. Type Account Created and Deleted in Short Timeframe
4. Click on the Rule to highlight it
5. Click Create Rule
![image](https://github.com/Tungsten66/Scenarios/assets/40893034/6e93a2f8-d752-44cd-8c1e-5d39b4e41c10)
6. Take default settings when creating the rule and Save
7. Review the new Incident created by navigating to the Incidents blade
![image](https://github.com/Tungsten66/Scenarios/assets/40893034/c1d5c735-2f25-40f2-8995-ddd6c9b4c0ee)



