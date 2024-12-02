<img width="955" alt="image" src="https://github.com/user-attachments/assets/cb9403cd-7ed2-4300-9394-ac7bdabbf3bd">

```bash
az ad sp create-for-rbac --name "<SPN_NAME>" --role Contributor --scopes "/subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP>"
az ad sp create-for-rbac --name "<SPN_NAME>" --skip-assignment --output json > spn.json
az role assignment create --assignee "<APP_ID>" --role Reader --scope "/subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP>"
az ad sp delete --id "<APP_ID>"
az login --service-principal -u "<APP_ID>" -p "<PASSWORD>" --tenant "<TENANT_ID>"
az group list --output table
```

<img width="957" alt="image" src="https://github.com/user-attachments/assets/49bdd2ca-6125-436d-a091-3b6d6ae7ab1f">

```bash
az vm identity assign -g <RESOURCE_GROUP> -n <VM_NAME>
az identity create -g <RESOURCE_GROUP> -n <IDENTITY_NAME>
az role assignment create --assignee "<IDENTITY_ID>" --role Reader --scope "/subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP>"
```
<img width="1024" alt="image" src="https://github.com/user-attachments/assets/f4de8114-4d0e-41a0-a1ec-7b81616103cf">

```hcl
resource "azuread_application" "example" {
  display_name = "example-app"
}

resource "azuread_service_principal" "example" {
  application_id = azuread_application.example.application_id
}

resource "azuread_service_principal_password" "example" {
  service_principal_id = azuread_service_principal.example.object_id
  value                = "example-password"
  end_date_relative    = "240h"
}

resource "azurerm_role_assignment" "example" {
  principal_id   = azuread_service_principal.example.object_id
  role_definition_name = "Contributor"
  scope          = azurerm_resource_group.example.id
}
```
<img width="996" alt="image" src="https://github.com/user-attachments/assets/1f7af747-0291-45af-9ce9-8f4ea3e49fe2">



```
