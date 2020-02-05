# Azure Policies, Management Groups and Subscriptions Via PowerShell


A Gathering of PS Scripts and Cmdlets Related to Deploying Policies, Management Groups and Subscriptions Using PowerShell



## Management Group Creation

URL - https://docs.microsoft.com/en-us/azure/governance/management-groups/create


## Example cmdlets

### New Group:

New-AzManagementGroup -GroupName 'Contoso'

### New Group with Display Name:

New-AzManagementGroup -GroupName 'Contoso' -DisplayName 'Contoso Group'

### New Group Under a parent:

$parentGroup = Get-AzManagementGroup -GroupName Contoso
New-AzManagementGroup -GroupName 'ContosoSubGroup' -ParentId $parentGroup.id

## Subscription Creation and Assignment

URL - https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/programmatically-create-subscription?tabs=azure-powershell

1.) Get the enrollment account

Get-AzEnrollmentAccount

2.) Create subscription under specific enrollment account

New-AzSubscription -OfferType MS-AZR-0017P -Name "Dev Team Subscription" -EnrollmentAccountObjectId <enrollmentAccountObjectId> -OwnerObjectId <userObjectId1>,<servicePrincipalObjectId>
    
## Add subscription to Management Group


URL - New-AzManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'

New-AzManagementGroupSubscription -GroupName 'Contoso' -SubscriptionId '12345678-1234-1234-1234-123456789012'
    




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


