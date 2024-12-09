<img width="972" alt="image" src="https://github.com/user-attachments/assets/db1ae6a8-1e53-4856-b1a1-1c65bca345d5">

<img width="1036" alt="image" src="https://github.com/user-attachments/assets/28dacdab-035d-4a7d-bbc0-44d08054a73d">

<img width="1052" alt="image" src="https://github.com/user-attachments/assets/b6927e75-593a-44a2-86a7-e6db6db8a61a">

<img width="1056" alt="image" src="https://github.com/user-attachments/assets/9697e5cc-44b8-4a61-86ab-7cc91b2252d3">

<img width="981" alt="image" src="https://github.com/user-attachments/assets/ad012f75-b4d4-4960-a89b-c466bc7203ce">

```bash
Authorization: token <PAT>

curl -X POST -H "Authorization: Bearer <JWT>" -H "Accept: application/vnd.github.v3+json" https://api.github.com/repos/<owner>/<repo>/dispatches
```

```yaml
- name: Install Azure CLI Beta
  run: az extension add --name "aks-preview"
----
- name: Login to Azure
  uses: azure/login@v1.4.0
  with:
    client-id: ${{ secrets.CLIENT_ID }}
    tenant-id: ${{ secrets.TENANT_ID }}
    subscription-id: ${{ secrets.SUBSCRIPTION_ID }}

---
permissions:
  id-token: write
```
