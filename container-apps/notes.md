# Azure DevOps CI/CD Pipeline for Deploying Applications to Azure Container Apps
```yaml
# Trigger Section
trigger: # Specifies the branch that triggers the pipeline
  branches:
    include:
      - main # The pipeline triggers on commits to the main branch
    exclude:
      - README.md # Excludes changes to README.md from triggering the pipeline

# Variable Definitions
variables:
  IMAGE_NAME: "myapp-image" # The name of the Docker image to be created
  CONTAINER_APP: "my-container-app" # Name of the Azure Container App
  RESOURCE_GROUP: "my-resource-group" # Azure Resource Group name
  ENV_NAME: "my-container-env" # Azure Container App environment name
  TAG: "$(Build.BuildId)" # Tag for the Docker image, using the build ID for uniqueness

# Stages Section
stages:
  - stage: BuildAndPush # First stage: Build Docker image and push to Docker Hub
    displayName: Build and Push Docker Image # A human-readable name for the stage
    jobs:
      - job: BuildImage # The job responsible for building the image
        steps:
          - task: Docker@2 # Task to interact with Docker
            inputs:
              containerRegistry: "DockerHubConnection" # Docker Hub service connection name
              repository: "$(IMAGE_NAME)" # Repository name on Docker Hub
              command: "buildAndPush" # Command to build and push the Docker image
              Dockerfile: "Dockerfile" # Path to the Dockerfile in the repository
              tags: "$(TAG)" # Tagging the image with the build ID

  - stage: Deploy # Second stage: Deploy the Docker image to Azure Container Apps
    displayName: Deploy to Azure Container Apps # A human-readable name for the stage
    jobs:
      - job: DeployApp # The job responsible for deploying the image
        steps:
          - task: AzureCLI@2 # Task to execute Azure CLI commands
            inputs:
              azureSubscription: "AzureServiceConnection" # Azure service connection for authentication
              scriptType: "bash" # Type of script to run
              scriptLocation: "inlineScript" # Indicates the script is written inline
              inlineScript: |
                az containerapp update \ # Command to update the container app
                  --name $(CONTAINER_APP) \ # Name of the Azure Container App
                  --resource-group $(RESOURCE_GROUP) \ # Resource group containing the app
                  --image mydockerhubuser/$(IMAGE_NAME):$(TAG) \ # Docker image to deploy
                  --ingress external \ # Expose app on a public endpoint
                  --target-port 3500 # Port exposed by the application in the container
```

<img width="774" alt="image" src="https://github.com/user-attachments/assets/2079e262-4590-4b0f-bc7f-c1308fe07613">
<img width="643" alt="image" src="https://github.com/user-attachments/assets/5133d7ab-c123-41e3-b53c-4cdd453bf24b">
<img width="1236" alt="image" src="https://github.com/user-attachments/assets/37e63ad7-9c2d-4c70-9c82-5fc8e987edc0">

```bash
az ad sp create-for-rbac --name "my-service-principal" --role contributor --scopes /subscriptions/<subscription-id>/resourceGroups/my-resource-group
```

