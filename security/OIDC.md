<img width="972" alt="image" src="https://github.com/user-attachments/assets/7b018b16-0972-41fc-83ad-da2283b1dc6f"><img width="979" alt="image" src="https://github.com/user-attachments/assets/f80e89fa-f938-4e28-a3f6-b348014c2660">

<img width="1037" alt="image" src="https://github.com/user-attachments/assets/77b5e16a-53db-4dcd-844a-590da2a1bb3f">

```bash
az identity create --name "Identity-AzureDevOps" --resource-group "<RESOURCE_GROUP>" --location "<LOCATION>"
az role assignment create --assignee "<MANAGED_IDENTITY_CLIENT_ID>" --role Contributor --scope "/subscriptions/<SUBSCRIPTION_ID>"
```

<img width="1016" alt="image" src="https://github.com/user-attachments/assets/9d28a3ef-8bd6-40a4-8dcd-2f5920e9a139">

```hcl
- task: AzureCLI@2
  inputs:
    azureSubscription: 'Identity-AzureDevOps'
    scriptType: 'bash'
    scriptLocation: 'inlineScript'
    inlineScript: |
      az group create --name demo --location eastus
```
<img width="972" alt="image" src="https://github.com/user-attachments/assets/8236fb31-8dcd-41cc-914d-9e27f90aa6a9">

```hcl
resource "azurerm_user_assigned_identity" "identity_devops" {
  name                = "identity-azuredevops"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
}

resource "azurerm_role_assignment" "identity_role" {
  scope                = "/subscriptions/<SUBSCRIPTION_ID>"
  role_definition_name = "Contributor"
  principal_id         = azurerm_user_assigned_identity.identity_devops.principal_id
}

resource "azurerm_user_assigned_identity_federated_credentials" "devops_federation" {
  name                = "identity-azuredevops"
  principal_id        = azurerm_user_assigned_identity.identity_devops.principal_id
  issuer              = "<ISSUER_URL>"
  subject             = "<SUBJECT_IDENTIFIER>"
}
```
<img width="957" alt="image" src="https://github.com/user-attachments/assets/c7dc5c24-2fdd-42ac-bcf9-264613fcf6f1">
<img width="978" alt="image" src="https://github.com/user-attachments/assets/0a8c7925-53f7-4479-8b18-0fcb7483351b">



