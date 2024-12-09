<img width="966" alt="image" src="https://github.com/user-attachments/assets/9940ce05-faf1-4006-bbff-b432a4109b83">

<img width="995" alt="image" src="https://github.com/user-attachments/assets/c1d4b185-5506-47ec-8e58-71c992b277d0">

```bash
Create a resource group
az group create --name <RESOURCE_GROUP_NAME> --location <LOCATION>

Create Azure Container Registry (ACR)
az acr create --resource-group <RESOURCE_GROUP_NAME> --name <ACR_NAME> --sku Basic

Configure a service principal
az ad sp create-for-rbac --name <SP_NAME> --role AcrPush --scopes $(az group show --name <RESOURCE_GROUP_NAME> --query id -o tsv)
```

<img width="1025" alt="image" src="https://github.com/user-attachments/assets/5b23fbb7-5cb9-4434-89bd-b6ffacec0931">

```yaml
name: Build and Deploy

on:
  push:
  workflow_dispatch:

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3
      - name: Login to Azure Container Registry
        uses: azure/docker-login@v1
        with:
          login-server: ${{ secrets.ACR_NAME }}.azurecr.io
          username: ${{ secrets.CLIENT_ID }}
          password: ${{ secrets.CLIENT_SECRET }}
      - name: Build and Push Image
        run: |
          docker build -t ${{ secrets.ACR_NAME }}.azurecr.io/webapp:${{ github.run_id }} .
          docker push ${{ secrets.ACR_NAME }}.azurecr.io/webapp:${{ github.run_id }}
```

```bash
az aks create --resource-group <RESOURCE_GROUP_NAME> --name <AKS_NAME> --node-count 2 --attach-acr <ACR_NAME> --service-principal <SP_APP_ID> --client-secret <SP_PASSWORD>
```

<img width="1019" alt="image" src="https://github.com/user-attachments/assets/ea9186ef-a2eb-4d77-a148-f3a0492b5eac">















