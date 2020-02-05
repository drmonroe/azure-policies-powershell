# Azure Policies and Management Groups Via PowerShell


A Gathering of PS Scripts and Cmdlets Related to Deploying Policies and Management Groups Using PowerShell


## POLICY ASSIGNMENT EXAMPLE




Create a policy assignment to identify non-compliant resources using Azure PowerShell

Url - https://docs.microsoft.com/en-us/azure/governance/policy/assign-policy-powershell


Cmdlet sequence:

# Register the resource provider if it's not already registered


Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'


# Get a reference to the resource group that will be the scope of the assignment


$rg = Get-AzResourceGroup -Name '<resourceGroupName>'

# Get a reference to the built-in policy definition that will be assigned
$definition = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Audit VMs that do not use managed disks' }

## Create the policy assignment with the built-in definition against your resource group


New-AzPolicyAssignment -Name 'audit-vm-manageddisks' -DisplayName 'Audit VMs without managed disks Assignment' -Scope $rg.ResourceId -PolicyDefinition $definition

## Get the resources in your resource group that are non-compliant to the policy assignment

Get-AzPolicyState -ResourceGroupName $rg.ResourceGroupName 
-PolicyAssignmentName 'audit-vm-manageddisks' -Filter 'IsCompliant eq false'



## Removes the policy assignment

Remove-AzPolicyAssignment -Name 'audit-vm-manageddisks' -Scope '/
subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>'


## POLICY INITIATIVE NOTES


$PolicySet = Get-AzPolicySetDefinition -Name "plociysetguidhere"
    New-AzPolicyAssignment -Name 'Name here' -PolicySetDefinition $PolicySet -Scope "/subscriptions/subscriptionidhere"
    
    
## Azure Policy definition structure

URL - https://docs.microsoft.com/en-us/azure/governance/policy/concepts/definition-structure


