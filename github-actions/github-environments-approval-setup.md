<img width="954" alt="image" src="https://github.com/user-attachments/assets/4e08203e-3225-48d6-8ec1-5ff574da2315">

<img width="907" alt="image" src="https://github.com/user-attachments/assets/40cabcd7-b695-446f-8bd0-a20b6a8e6f62">

```yaml
deploy-to-aks:
  runs-on: ubuntu-latest
  environment: PROD
  steps:
    - name: Checkout Code
      uses: actions/checkout@v3
    - name: Deploy to AKS
      uses: Azure/k8s-deploy@v3
      with:
        manifests: |
          ./manifests/deployment.yaml
          ./manifests/service.yaml
        images: |
          ${{ secrets.ACR_NAME }}.azurecr.io/webapp:${{ github.run_id }}
```
<img width="998" alt="image" src="https://github.com/user-attachments/assets/4014f3d4-a13d-4a74-862e-701320dfbe52">

<img width="993" alt="image" src="https://github.com/user-attachments/assets/0cd86788-c286-45dd-b3b7-218f9ff8523c">
