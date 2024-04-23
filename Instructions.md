# Instructor steps to prep for scenarios

## Create SOC accounts
Create user accounts that students will use during the sessions

```console
# Roles and permissions in Microsoft Sentinel | Microsoft Learn

# Manaully capture the Resource Group Sentinel is in and update $ResourceGroup variable with the full id
$ResourceGroup="/subscriptions/<GUID>/resourceGroups/Security"

# Get the tenant domain name
$domain=$(az ad signed-in-user show --query 'userPrincipalName' -o tsv | cut -d '@' -f 2)

#Create a group in the directory
$GroupName = "SOC_Responders"
az ad group create --display-name $GroupName --mail-nickname $GroupName --description "SOC Demo: Microsoft Sentinel Responder permissions to Sentinel LAW"
$GroupId = az ad group show --group $GroupName --query id -o tsv

# Set the password profile
$password = openssl rand -base64 16

# Loop through 1 to 8 and create users
  for ($i = 1; $i -le 8; $i++) {
  
  #Set the display name and user principal name
  $UserDisplayName="SOC User$i"
  $UserPrincipalName="SOCUser$i@$domain"
  
  #Create the users
  az ad user create --display-name $UserDisplayName --password $password --user-principal-name $UserPrincipalName --force-change-password-next-sign-in true
 
  $UserId = az ad user show --id $UserPrincipalName --query id -o tsv

  #Add new users to a group
  az ad group member add --group $GroupName --member-id $UserId

}

# Add group to Microsoft Sentinel Responder role
az role assignment create --role "Microsoft Sentinel Responder" --assignee-object-id $GroupId --assignee-principal-type Group --scope $ResourceGroup

Write-Host "User Created password is $password" -ForegroundColor Green
```
> [!NOTE]
> Manually assign new users to the Security Reader built-in role

# Defense Evasion – Indicator Removal on Host
Clear windows security event log
Build a Windows VM
Azure Portal > Select the virtual machine > Run command > RunPowerShellScript > 

```console
Clear-Eventlog "security"
```

# Credential Access – Failed Login Attempts
login into portal with wrong password 5 or more times
Create this account and try to login with the wrong password to the portal 5 or more times.

```console
# Set the password profile
$password = openssl rand -base64 16

# Get the tenant domain name
$domain=$(az ad signed-in-user show --query 'userPrincipalName' -o tsv | cut -d '@' -f 2)

az ad user create --display-name "Darth Maul" --password $password --user-principal-name MaulD@$domain
```

# Initial Access – Valid Accounts
Rapidly create and delete one account

```console
# Set the password profile
$password = openssl rand -base64 16

# Get the tenant domain name
$domain=$(az ad signed-in-user show --query 'userPrincipalName' -o tsv | cut -d '@' -f 2)

#Create User
az ad user create --display-name "Anakin Skywalker" --password $password --user-principal-name SkywalkerA@$domain

#Wait for 10 seconds
Start-Sleep -Seconds 10

#Delete User
az ad user delete --id SkywalkerA@$domain
```

# Initial Access Exploit Public-Facing Application
1. Follow steps to Enable Web Application Firewall using Azure PowerShell - [https://learn.microsoft.com/en-us/azure/web-application-firewall/ag/tutorial-restrict-web-traffic-powershell)) <br />
2. Build a Windows VM
3. Install nmap

nmap configuration <br />
**Target:** enter the public IP of your application <br />
**Profile:** select Intense scan, no ping <br />

Run this 3 or more times <br />

# Impact-NetworkDenialOfService

Building a VM with a public ip to perform Azure DDoS Protection simulation testing

## Reference Articles
https://learn.microsoft.com/en-us/azure/ddos-protection/test-through-simulations <br />
https://learn.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-cli  <br />
https://learn.microsoft.com/en-us/azure/ddos-protection/manage-ddos-protection-cli

```console
# Variables
resourcegroup="rg-lab-demo"
ddosplanname="DDosLabDemo"
location="eastus"
vnetname="vnet-lab-demo"
vmname="vm-lab-demo"
username="azureuser"
sentinelworkspace="/subscriptions/{subscription-id}/resourceGroups/{log-analytics-resource-group}/providers/Microsoft.OperationalInsights/workspaces/{log-analytics-workspace-name}"

# Create a Resource Group
az group create --name $resourcegroup --location $location

# Create a Virtual Machine with Public IP
az vm create --resource-group $resourcegroup --name $vmname --image Win2022AzureEditionCore --public-ip-address "pip-lab-demo" --admin-username $username --vnet-name $vnetname

# Install web server
az vm run-command invoke -g $resourcegroup -n $vmname --command-id RunPowerShellScript --scripts "Install-WindowsFeature -name Web-Server -IncludeManagementTools"

# Open port 80 for web traffic
az vm open-port --port 80 --resource-group $resourcegroup --name $vmname

# Create a DDoS Protection plan
az network ddos-protection create --resource-group $resourcegroup --name $ddosplanname

# Enable DDoS protection for an existing virtual network
az network vnet update --resource-group $resourcegroup --name $vnetname --ddos-protection-plan $ddosplanname --ddos-protection true

# Creating variable for the public ip address resourece id
pipresourceid=$(az network public-ip list --query "[?name=='pip-lab-demo'].{ResourceId:id}" --output table)

# Create diagnostic settings
az monitor diagnostic-settings create --name "Sentinel-DDoS" --resource $pipresourceid --logs '[{"category": "DDoSProtectionNotifications", "enabled": true}, {"category": "DDOSMitigationFlowLogs", "enabled": true}, {"category": "DDOSMitigationReports", "enabled": true}]' --workspace $sentinelworkspace

# List Public IP address
publicip=$(az vm show -d -g $resourcegroup -n $vmname --query publicIps -o tsv)
echo -e "\e[32mUse this public ip for DDoS testing: $publicip\e[0m"
```
