# Anonymous IP walkthrough

## Cited Resources:
[Microsoft: Anonymous IP address Risk Protection](https://learn.microsoft.com/en-us/azure/active-directory/identity-protection/howto-identity-protection-simulate-risk#anonymous-ip-address) <br />


## Assumptions:
A login was attempted from a Tor Browser to simulate anonymous IP addresses.  <br />
[License requirements:](https://learn.microsoft.com/en-us/azure/active-directory/identity-protection/overview-identity-protection#license-requirements) Microsoft Entra ID P2 (Azure Active Directory P2)


## View video on how a threat actor will use the TOR Browser to obfuscate their identity after acquiring stolen credentials


## View in Entra Identity:
1. Using one of the SOC analyst accounts provided, log in to [Microsoft Entra admin Center](https://entra.microsoft.com/)
2. Click on ID Protection <br />
![2](https://github.com/Tungsten66/Scenarios/assets/40893034/ae01e91b-1b3b-41cc-86ac-ce3d1859523d)
3. Click Protection/Identity Protection, then Risk Detections <br />
![3](https://github.com/Tungsten66/Scenarios/assets/40893034/2311ecc9-0450-4d04-a443-002843aeb7f4)
4. In Risk Detections note the one from an Anonymous IP  <br />
![4](https://github.com/Tungsten66/Scenarios/assets/40893034/c6431f65-48d3-4f3f-8a73-982146dd395d)
5. Click on and review the user  <br />
![5](https://github.com/Tungsten66/Scenarios/assets/40893034/f039c6cf-f4d9-4561-a4fd-a9f04f294963)
6. Optionally review Risky Sign-ins <br />
![6](https://github.com/Tungsten66/Scenarios/assets/40893034/9b138657-dcb7-44d8-a584-be00ae9674da)
7. Optionally review Risky Users <br />
![7](https://github.com/Tungsten66/Scenarios/assets/40893034/7673cd4c-45da-43ab-92cf-d2757d66651d)
8. Here is where you can Confirm or Dismiss the user risk (DO NOT do this during the demo) <br />
![8](https://github.com/Tungsten66/Scenarios/assets/40893034/9a0d58d9-2045-4a51-9f9f-6774fb9ee702)


## Instructor demonstration of Analytic Rule Creation:


## View in Microsoft Sentinel:

1. Go to [Azure Portal](https://portal.azure.com/) and Search for Microsoft Sentinel
![1-1](https://github.com/Tungsten66/Scenarios/assets/40893034/b6499c4f-f7a0-4ddb-96bb-77cd4a162458)
2.Click on your desired Sentinel Instance
![1-2](https://github.com/Tungsten66/Scenarios/assets/40893034/20813e65-cd76-4909-8018-895ea33c4b92)
3. Go to Incidents and note the Anonymous IP Address incident
![1-3](https://github.com/Tungsten66/Scenarios/assets/40893034/29954861-fccb-43e1-8d4b-6529ae20cb6a)
