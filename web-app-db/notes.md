# Pipeline for Deploying Web Applications and Databases in Azure DevOps
<img width="848" alt="image" src="https://github.com/user-attachments/assets/213eb355-d1a1-4d57-be4c-dfcba8048db1">

```yaml
# Trigger Section
trigger: # Triggers the pipeline on changes to the main branch
  branches:
    include:
      - main # Specifies the main branch as the trigger source

# Variables Section
variables:
  APP_NAME: "web-app" # Name of the web application
  DB_NAME: "sql-database" # Name of the database
  RESOURCE_GROUP: "ci-cd-rg" # Azure Resource Group name
  ENVIRONMENT: "dev" # Deployment environment
  TEMPLATE_PATH: "infra/templates" # Path to ARM or Terraform templates
  PACKAGE_PATH: "artifacts/packages" # Path for application packages
  DACPAC_PATH: "artifacts/dacpacs" # Path for Dacpac files

# Stages Section
stages:
  - stage: BuildStage # CI stage to build application and database
    displayName: Build Application and Database
    jobs:
      - job: BuildJob
        steps:
          - task: UseDotNet@2 # Task to set up .NET SDK
            inputs:
              packageType: "sdk"
              version: "6.x" # Version of .NET SDK

          - script: dotnet build $(APP_NAME) # Build the web application
            displayName: Build Web Application

          - script: dotnet build $(DB_NAME) # Build the database project
            displayName: Build Database Project

          - task: SonarQubePrepare@4 # Static code analysis for application
            inputs:
              scannerMode: "MSBuild"
              configMode: "manual"
              projectKey: "$(APP_NAME)-sonarqube"

          - task: SonarQubeAnalyze@4 # Analyze the source code
            displayName: Analyze Source Code

          - script: dotnet publish $(APP_NAME) -o $(PACKAGE_PATH) # Publish web application package
            displayName: Publish Web Application Package

          - script: dotnet publish $(DB_NAME) -o $(DACPAC_PATH) # Publish database Dacpac files
            displayName: Publish Database Dacpac

  - stage: DeployStage # CD stage to deploy application and database
    displayName: Deploy Application and Database
    jobs:
      - job: InfraJob
        steps:
          - script: terraform init -backend-config="path=$(TEMPLATE_PATH)" # Initialize Terraform
            displayName: Initialize Terraform

          - script: terraform plan -out=tfplan # Plan infrastructure changes
            displayName: Plan Infrastructure

          - script: terraform apply tfplan # Apply infrastructure changes
            displayName: Deploy Infrastructure

          - task: AzCopy@2 # Deploy web application package to Azure Blob Storage
            inputs:
              sourcePath: "$(PACKAGE_PATH)"
              containerName: "web-app-container"
              destination: "$(RESOURCE_GROUP)"

          - task: AzureCLI@2 # Deploy Dacpac file to SQL Server
            inputs:
              azureSubscription: "AzureServiceConnection"
              scriptType: "bash"
              scriptLocation: "inlineScript"
              inlineScript: |
                sqlpackage /Action:Publish \
                           /SourceFile:$(DACPAC_PATH)/$(DB_NAME).dacpac \
                           /TargetServerName:"<db-server-name>.database.windows.net" \
                           /TargetDatabaseName:$(DB_NAME)

      - job: TestJob
        steps:
          - script: selenium test run # Run Selenium tests for integration/UI testing
            displayName: Run UI Tests
```
<img width="601" alt="image" src="https://github.com/user-attachments/assets/c9fa0dbf-9168-4086-8173-0111426881ac">
<img width="979" alt="image" src="https://github.com/user-attachments/assets/a20637e4-ecd9-4f4d-b34e-aaa9182816f0">










