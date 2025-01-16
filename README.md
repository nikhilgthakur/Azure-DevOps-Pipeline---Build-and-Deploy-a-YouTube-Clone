# 🚀 Azure DevOps Pipeline - Build and Deploy a YouTube Clone 

## Project Overview
This project demonstrates how to build and deploy a YouTube Clone application using Azure DevOps pipelines. It covers the process from setting up the infrastructure to deploy the application on Azure App Service.

## Prerequisites

- **Azure DevOps Account:** Ensure you have an Azure DevOps organization set up.
- **Azure Subscription:** Required for deploying the application to Azure App Service.
- **IDE:** Use Visual Studio Code (VSCode) or any other preferred IDE.
- **Git:** Ensure Git is installed and configured.

## Steps to Set Up the Infrastructure

### 1. Clone the Application Code
Open a terminal in your IDE and run the following commands to download the application code:
  
  ```
  mkdir youtube_clone; cd youtube_clone
  git init
  git clone https://github.com/piyushsachdeva/Youtube_Clone
  ```

### 2. Push Code to Azure DevOps
Create a new project in Azure DevOps for this build pipeline and push the code using the following commands:

  ```
  git remote add origin $YOURAZUREREPO
  git push -u origin all
  ```

> **Note:** Replace `<YOUR_AZURE_REPO_URL>` with your Azure repository URL.

### 3. Create Azure App Service
- Go to [Azure Portal](https://portal.azure.com).
- Create an Azure App Service 

<img width="916" alt="image" src="https://github.com/user-attachments/assets/1fe831a2-a90f-4289-b694-3894c55574be" />

### 4. Implement the Build Pipeline
- Use the Azure DevOps pipeline the build pipeline.
- Set up service connections and configure service principals as needed.

## Pipeline Code

Below is the pipeline YAML used in the project:

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
        azureSubscription: '<<Subscription_name>>'
        appType: 'webAppLinux'
        WebAppName: '<<WebApp_name>>'
        packageForLinux: '$(System.ArtifactsDirectory)/drop'
        RuntimeStack: 'STATICSITE|1.0'
```

<img width="917" alt="image" src="https://github.com/user-attachments/assets/a43ce827-83db-457d-8f00-e128bcda1a6b" />

<img width="913" alt="image" src="https://github.com/user-attachments/assets/2d6d1005-e6f9-4969-b3db-cf6d251c18b7" />
  
<img width="950" alt="image" src="https://github.com/user-attachments/assets/134453be-ac1b-4154-9722-f52367a1030b" />


### 5. Configure App Settings
Set the following app settings to disable all file caching:

- **WEBSITE_DYNAMIC_CACHE:** `0`
- **WEBSITE_LOCAL_CACHE_OPTION:** `Never`

<img width="898" alt="image" src="https://github.com/user-attachments/assets/09911c81-82bf-4f83-8c8c-143535bcd9e4" />


## Azure DevOps Pipeline Structure

### Pipeline Components
1. **Trigger:** Determines when the pipeline runs (CI, scheduled, manual, etc.).
2. **Stages:** Organize jobs; a pipeline can have multiple stages.
3. **Jobs:** Each stage contains one or more jobs.
4. **Agents:** Jobs run on agents (e.g., Ubuntu, Windows, macOS).
5. **Steps:** The smallest building block, can be tasks or scripts.
6. **Tasks:** Pre-packaged scripts for specific actions.
7. **Artifacts:** Collections of files or packages published by a run.

Application Running on Azure WebApp

![image](https://github.com/user-attachments/assets/ae72a703-eaa7-4ac1-80e0-511ab08d8433)

---


---

# Deploying the Application Using Separate Build and Release Pipelines with an Additional Stage for Deployment to the Dev Environment

![image](https://github.com/user-attachments/assets/93b157b3-591c-4781-801f-57227da796c9)


Follow the same process up to Step 3 as outlined above.

### 4. Implement the Build Pipeline

Code below

```
trigger:
- main

stages:
  - stage: Build
    jobs:
    - job: Build
      pool: 
        vmImage: 'Ubuntu-latest'
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
```

### 5. Implement the Release Pipeline

![image](https://github.com/user-attachments/assets/d747e862-863e-4a17-8051-8c004e881ee3)

![image](https://github.com/user-attachments/assets/4f28743a-27f5-4e8d-9087-67bdcb126272)

![image](https://github.com/user-attachments/assets/b2f4f643-309f-4aa0-86b0-41bdebe3ac9e)

![image](https://github.com/user-attachments/assets/7d661cb3-e50d-44e4-9b83-e193eb83661d)

![image](https://github.com/user-attachments/assets/acfeb9b7-6a5b-48c3-932d-5a8edebba24a)

![image](https://github.com/user-attachments/assets/ba00cd97-4688-4aa7-a8c9-c282dc23f73d)

![image](https://github.com/user-attachments/assets/556151c6-90e8-4d79-93b6-57a0401cf435)

![image](https://github.com/user-attachments/assets/41286c04-226a-411c-b6dd-c65dd956b59f)

![image](https://github.com/user-attachments/assets/3248cd9f-579f-4842-8f9e-7a976e91037e)

![image](https://github.com/user-attachments/assets/06d45ca6-3aa9-4e6d-80aa-ed513f8a2df6)



### 6. Swap the  deployment slots


![image](https://github.com/user-attachments/assets/5f44b30d-8248-407a-9c78-85c5477558c5)

### 7. Application running successfully


![image](https://github.com/user-attachments/assets/2e4bd098-887f-4fc4-858a-cdd90b66af00)


END

