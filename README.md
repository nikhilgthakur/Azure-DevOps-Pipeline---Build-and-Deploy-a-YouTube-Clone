# Azure DevOps Build Pipeline - Build and Deploy a YouTube Clone

## Project Overview
This project demonstrates how to build and deploy a YouTube Clone application using Azure DevOps pipelines. It covers the process from setting up the infrastructure to deploying the application on Azure App Service.

---

## Prerequisites

- **Azure DevOps Account:** Ensure you have an Azure DevOps organization set up.
- **Azure Subscription:** Required for deploying the application to Azure App Service.
- **IDE:** Use Visual Studio Code (VSCode) or any other preferred IDE.
- **Git:** Ensure Git is installed and configured.

---

## Steps to Set Up the Infrastructure

### 1. Clone the Application Code
Open a terminal in your IDE and run the following commands to download the application code:

mkdir day4_youtube_clone
cd day4_youtube_clone
git init
git clone https://github.com/piyushsachdeva/Youtube_Clone

### 2. Push Code to Azure DevOps
Create a new project in Azure DevOps for this build pipeline and push the code using the following commands:

git remote add origin <YOUR_AZURE_REPO_URL>
git push -u origin all

> **Note:** Replace `<YOUR_AZURE_REPO_URL>` with your Azure repository URL.

### 3. Create Azure App Service
- Go to [Azure Portal](https://portal.azure.com).
- Create an Azure App Service 



### 4. Implement the Build Pipeline
- Use the Azure DevOps classic editor to create the build pipeline.
- Set up service connections and configure service principals as needed.

### 5. Configure App Settings
Set the following app settings to disable all file caching:

- **WEBSITE_DYNAMIC_CACHE:** `0`
- **WEBSITE_LOCAL_CACHE_OPTION:** `Never`

---

## Azure DevOps Pipeline Structure

### Pipeline Components
1. **Trigger:** Determines when the pipeline runs (CI, scheduled, manual, etc.).
2. **Stages:** Organize jobs; a pipeline can have multiple stages.
3. **Jobs:** Each stage contains one or more jobs.
4. **Agents:** Jobs run on agents (e.g., Ubuntu, Windows, macOS).
5. **Steps:** The smallest building block, can be tasks or scripts.
6. **Tasks:** Pre-packaged scripts for specific actions.
7. **Artifacts:** Collections of files or packages published by a run.

---

## Pipeline Code

Below is the pipeline YAML used in the demo:

```yaml
trigger:
- main

stages:
- stage: Build
  jobs:
  - job: Build
    pool:
      vmImage: 'ubuntu-latest'
    steps:
    - task: Npm@1
      inputs:
        command: 'install'
    - task: Npm@1
      inputs:
        command: 'custom'
        customCommand: 'run build'
    
    - task: PublishBuildArtifacts@1
      inputs:
        PathtoPublish: 'build'
        ArtifactName: 'drop'
        publishLocation: 'Container'

- stage: Deploy
  jobs:
  - job: Deploy
    pool:
      vmImage: 'ubuntu-latest'
    steps:
    - task: DownloadBuildArtifacts@1
      inputs:
        buildType: 'current'
        downloadType: 'single'
        artifactName: 'drop'
        downloadPath: '$(System.ArtifactsDirectory)'
    - task: AzureRmWebAppDeployment@4
      inputs:
        ConnectionType: 'AzureRM'
        azureSubscription: 'Tech Tutorials With Piyush (9e9c27ce-e0c8-4171-a368-ad16977ec849)'
        appType: 'webAppLinux'
        WebAppName: 'TechTutorialsWithPiyush'
        packageForLinux: '$(System.ArtifactsDirectory)/drop'
        RuntimeStack: 'STATICSITE|1.0'
```

---

## Key Learnings

- **Service Connection & Service Principal:** Understanding how to securely connect Azure DevOps pipelines to Azure resources.
- **Pipeline Stages and Jobs:** Managing build and deployment processes efficiently.
- **Caching Management:** Properly disabling file caching to ensure reliable application behavior.

---

## Additional Resources

- [Azure DevOps Documentation](https://docs.microsoft.com/en-us/azure/devops)
- [Azure App Service Documentation](https://docs.microsoft.com/en-us/azure/app-service)

---

## License
This project is licensed under the MIT License. See the LICENSE file for details.

