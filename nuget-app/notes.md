# CI/CD Pipelines for Publishing NuGet Packages with Azure DevOps

```yaml
# Trigger Section
trigger: # Specifies the branch that triggers the pipeline
  branches:
    include:
      - main # The pipeline runs on changes to the main branch

# Pool Configuration
pool:
  vmImage: 'ubuntu-latest' # Specifies the virtual machine image for the build agent

# Variables Section
variables:
  buildConfiguration: 'Release' # Build configuration (Release or Debug)
  artifactName: 'my-library-nuget' # Name of the Azure Artifact feed

# Stages Section
stages:
  - stage: Build
    displayName: Build Stage
    jobs:
      - job: BuildJob
        displayName: Build and Package NuGet Library
        steps:
          # Step 1: Checkout Source Code
          - task: Checkout@1
            displayName: 'Checkout Source Code'
          
          # Step 2: Restore Dependencies
          - task: UseDotNet@2
            inputs:
              command: 'restore'
              projects: '**/*.csproj' # Restores dependencies for all .csproj files
            displayName: 'Restore Dependencies'

          # Step 3: Build the Project
          - task: UseDotNet@2
            inputs:
              command: 'build'
              projects: '**/*.csproj' # Builds the .csproj file
              arguments: '--configuration $(buildConfiguration)'
            displayName: 'Build the Project'

          # Step 4: Pack NuGet Package
          - task: UseDotNet@2
            inputs:
              command: 'pack'
              projects: '**/*.csproj'
              arguments: '--configuration $(buildConfiguration) --output $(Build.ArtifactStagingDirectory)'
            displayName: 'Pack NuGet Package'

          # Step 5: Publish Package to Azure Artifacts
          - task: NuGetCommand@2
            inputs:
              command: 'push'
              packagesToPush: '$(Build.ArtifactStagingDirectory)/*.nupkg'
              publishVstsFeed: '$(artifactName)' # Pushes to the Azure Artifacts feed
            displayName: 'Publish NuGet Package'
```
<img width="978" alt="image" src="https://github.com/user-attachments/assets/40ed1e06-08e1-413c-9ec0-2b56577defaa">

<img width="1145" alt="image" src="https://github.com/user-attachments/assets/5216b96f-a7b3-479f-a496-3a36abc044f6">

<img width="1050" alt="image" src="https://github.com/user-attachments/assets/12aa7dd6-355c-4f13-8f87-18b18bb1fbe0">

```xml
<PropertyGroup>
  <TargetFramework>netstandard2.0</TargetFramework>
  <PackageId>MyLibrary</PackageId>
  <Version>1.0.0</Version>
  <Authors>AuthorName</Authors>
  <Description>A sample NuGet package</Description>
  <PackageOutputPath>$(OutputPath)</PackageOutputPath>
  <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
</PropertyGroup>
```
```xml
<IsPackable>true</IsPackable>
```

<img width="992" alt="image" src="https://github.com/user-attachments/assets/aaf99307-8eea-428f-bb21-c116f322c67a">

```bash
nuget push *.nupkg -Source https://pkgs.dev.azure.com/yourorg/_packaging/yourfeed/nuget/v3/index.json
```













