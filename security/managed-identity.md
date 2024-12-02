<img width="996" alt="image" src="https://github.com/user-attachments/assets/0c350860-77a7-4e7e-a031-d83a0896a285">

<img width="1024" alt="image" src="https://github.com/user-attachments/assets/f0b565b1-a4a8-4915-8480-ab798e9c341b">

<img width="1015" alt="image" src="https://github.com/user-attachments/assets/5fe12d0c-0e67-4dea-8d79-bee4e5613ffc">

```bash
az identity create --name "<IDENTITY_NAME>" --resource-group "<RESOURCE_GROUP>" --location "<LOCATION>"
az role assignment create --assignee "<IDENTITY_CLIENT_ID>" --role Reader --scope "/subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP>"
curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2019-08-01&resource=<RESOURCE_URI>"
az login --identity
az group list --output table

az login --identity --username "<IDENTITY_NAME>"
az group list --output table
```

<img width="1056" alt="image" src="https://github.com/user-attachments/assets/87e751d8-e22c-4d99-a7b6-f278aae36724">

```hcl
resource "azurerm_user_assigned_identity" "example" {
  name                = "example-identity"
  resource_group_name = azurerm_resource_group.example.name
  location            = azurerm_resource_group.example.location
}

resource "azurerm_role_assignment" "example" {
  principal_id         = azurerm_user_assigned_identity.example.principal_id
  role_definition_name = "Reader"
  scope                = azurerm_resource_group.example.id
}

resource "azurerm_linux_virtual_machine" "example" {
  name                = "example-vm"
  resource_group_name = azurerm_resource_group.example.name
  location            = azurerm_resource_group.example.location
  size                = "Standard_DS1_v2"

  identity {
    type = "UserAssigned"
    user_assigned_identities = {
      azurerm_user_assigned_identity.example.id = {}
    }
  }
}
```
<img width="1019" alt="image" src="https://github.com/user-attachments/assets/694810ac-d004-4002-9fcb-dd5cbb988c68">

<img width="932" alt="image" src="https://github.com/user-attachments/assets/f99ac743-01f5-46e8-aeff-250a66ee4f35">







