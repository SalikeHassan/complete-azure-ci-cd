# CI/CD Pipeline for Deploying Applications into Kubernetes Clusters with Azure DevOps

```yaml
# Trigger Section
trigger: # Specifies what events trigger the pipeline
  branches:
    include:
      - main # Trigger pipeline on changes to the main branch

# Variables Section
variables:
  TAG: "$(Build.BuildId)" # Auto-generated tag for images using Azure DevOps build ID
  IMAGE_NAME: "app-image" # Name of the Docker image
  RESOURCE_GROUP: "ci-cd-rg" # Azure Resource Group for the cluster
  CLUSTER_NAME: "my-aks-cluster" # Azure Kubernetes Service (AKS) cluster name
  NAMESPACE: "dev" # Namespace for deployments
  HELM_CHART_NAME: "app-chart" # Name of the Helm chart
  HELM_RELEASE_NAME: "app-release" # Helm release name for deployments

# Pool Configuration
pool:
  vmImage: "ubuntu-latest" # Use Ubuntu latest as the build agent

# Pipeline Stages
stages:
  - stage: Build
    displayName: Build Stage
    jobs:
      - job: ScanAndTest
        displayName: Scan and Test Application
        steps:
          - script: |
              sudo apt-get update
              sudo apt-get install -y python3-pip
              pip3 install checkov
            displayName: Install Checkov # Installs Checkov for scanning

          - script: checkov --directory ./app --output json
            displayName: Scan Application Configuration # Scans Dockerfile and YAML files

          - script: docker build -t $(IMAGE_NAME):$(TAG) .
            displayName: Build Docker Image # Builds Docker image with a specific tag

          - script: |
              docker run --rm aquasec/trivy image $(IMAGE_NAME):$(TAG)
            displayName: Scan Docker Image # Scans Docker image for vulnerabilities

          - script: docker push $(IMAGE_NAME):$(TAG)
            displayName: Push Docker Image to ECR # Pushes Docker image to Azure Container Registry

  - stage: Release
    displayName: Release Stage
    jobs:
      - job: DeployDev
        displayName: Deploy to Dev Environment
        steps:
          - task: Kubernetes@1
            inputs:
              connectionType: "Azure Resource Manager"
              azureSubscription: "AzureServiceConnection"
              resourceGroupName: "$(RESOURCE_GROUP)"
              clusterName: "$(CLUSTER_NAME)"
              namespace: "$(NAMESPACE)"
              command: "apply"
              manifests: "**/*.yaml"
            displayName: Deploy YAML Files to Kubernetes # Deploys YAML manifests to the dev namespace

      - job: IntegrationTests
        displayName: Run Integration Tests
        steps:
          - script: selenium test run
            displayName: Run Selenium Tests # Runs Selenium-based UI or integration tests

      - job: DeployQA
        displayName: Deploy to QA Environment
        steps:
          - task: HelmDeploy@0
            inputs:
              azureSubscription: "AzureServiceConnection"
              resourceGroupName: "$(RESOURCE_GROUP)"
              clusterName: "$(CLUSTER_NAME)"
              namespace: "qa"
              chartType: "FilePath"
              chartPath: "./charts/$(HELM_CHART_NAME)"
              releaseName: "$(HELM_RELEASE_NAME)"
              overrideValues: |
                image.tag=$(TAG)
              command: "upgrade"
              install: true
              waitForExecution: true
            displayName: Deploy Helm Chart to QA # Deploys Helm chart to QA namespace

      - job: ManualValidation
        displayName: Validate Deployment to Production
        dependsOn: DeployQA
        steps:
          - task: ManualValidation@0
            inputs:
              instructions: "Validate deployment in QA before proceeding to production."
              timeout: "120"
              onTimeout: "reject"
            displayName: Manual Approval Step # Manual approval before deploying to production

      - job: DeployProd
        displayName: Deploy to Production
        dependsOn: ManualValidation
        steps:
          - task: HelmDeploy@0
            inputs:
              azureSubscription: "AzureServiceConnection"
              resourceGroupName: "$(RESOURCE_GROUP)"
              clusterName: "$(CLUSTER_NAME)"
              namespace: "production"
              chartType: "FilePath"
              chartPath: "./charts/$(HELM_CHART_NAME)"
              releaseName: "$(HELM_RELEASE_NAME)"
              overrideValues: |
                image.tag=$(TAG)
              command: "upgrade"
              install: true
              waitForExecution: true
            displayName: Deploy Helm Chart to Production # Deploys Helm chart to production namespace
```
<img width="1056" alt="image" src="https://github.com/user-attachments/assets/cc68c80d-6d22-42ff-ba9d-aa3898f30aac">








