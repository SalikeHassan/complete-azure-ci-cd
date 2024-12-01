<img width="773" alt="image" src="https://github.com/user-attachments/assets/aaf3d830-843e-40a3-9f7a-46b0bfe0d6e0">

```yaml
trigger:
  branches:
    include:
      - main # Trigger on changes to the main branch

pool:
  vmImage: 'ubuntu-latest' # Build agent

stages:
  - stage: Deploy
    displayName: Deploy Stage
    jobs:
      - job: ReuseTemplate
        displayName: Reuse Template
        steps:
          - template: sample-template.steps.yaml # Reference the external template
            parameters:
              message: "Hello from YAML Templates in Azure DevOps" # Parameter passed to template
```

```yaml
sample-template.steps.yaml
parameters:
  message: "Default Message" # Default parameter value

steps:
  - script: |
      echo $(message) # Print the message
    displayName: "Print Message from Template"
```
<img width="1001" alt="image" src="https://github.com/user-attachments/assets/de3b3606-bfe6-4580-addc-81ecbbd12e9b">

# Using Templates with Terraform
```yaml
trigger:
  branches:
    include:
      - main

pool:
  vmImage: 'ubuntu-latest'

variables:
  environment: 'dev'

stages:
  - stage: CreateInfrastructure
    displayName: Create Terraform Backend
    jobs:
      - job: DeployBackend
        steps:
          - template: terraform-backend.template.yaml
            parameters:
              storageAccountName: "tfstate$(environment)"
              resourceGroupName: "rg-$(environment)"
              location: "eastus"
  - stage: DeployInfrastructure
    displayName: Deploy Terraform Stages
    dependsOn: CreateInfrastructure
    jobs:
      - job: DeployToEnv
        steps:
          - template: terraform-stages.template.yaml
            parameters:
              environment: "$(environment)"
```
```yaml
Terraform-backend.template.yaml
parameters:
  storageAccountName: ""
  resourceGroupName: ""
  location: ""

steps:
  - script: |
      az group create --name $(resourceGroupName) --location $(location)
      az storage account create --name $(storageAccountName) --resource-group $(resourceGroupName) --location $(location) --sku Standard_LRS
    displayName: "Create Terraform Backend"
```
```yaml
terraform-stages.template.yaml
parameters:
  environment: "dev"

steps:
  - script: |
      terraform init
      terraform plan -var="environment=$(environment)"
      terraform apply -auto-approve
    displayName: "Deploy Terraform for $(environment)"
```
<img width="955" alt="image" src="https://github.com/user-attachments/assets/4f9a2a15-7a0f-4c2b-9fd0-521cdebe33e6">

```yaml
variables:
  - group: MyVariableGroup # Reference the variable group

pool:
  vmImage: 'ubuntu-latest'

stages:
  - stage: Deploy
    displayName: Deploy Stage
    jobs:
      - job: DisplayVariable
        steps:
          - script: |
              echo $(MyVariable) # Access variable from the group
            displayName: "Print Variable from Group"
```
<img width="993" alt="image" src="https://github.com/user-attachments/assets/3954f805-dd35-430a-b2ee-1b8e5687dc83">

```yaml
variables:
  - group: KeyVaultVariableGroup # Reference the linked Key Vault variable group

pool:
  vmImage: 'ubuntu-latest'

stages:
  - stage: SecureStage
    displayName: Secure Deployment
    jobs:
      - job: UseSecrets
        steps:
          - script: |
              echo "Using secrets securely!"
              echo $(SecretVariable) # Access Key Vault secret
            displayName: "Access Key Vault Secret"
```

<img width="983" alt="image" src="https://github.com/user-attachments/assets/50c71999-9695-465b-bf8e-2743193ebdab">

```yaml
resources:
  pipelines:
    - pipeline: FirstPipeline # Alias for the first pipeline
      source: First-Pipeline-Name # Name of the first pipeline
      trigger: true # Enable triggering

stages:
  - stage: Deploy
    displayName: Deploy After First Pipeline
    jobs:
      - job: RunStep
        steps:
          - script: echo "Triggered after First Pipeline"
```
```yaml
steps:
  - template: second-pipeline.template.yaml # Include another pipeline as a template
```


































