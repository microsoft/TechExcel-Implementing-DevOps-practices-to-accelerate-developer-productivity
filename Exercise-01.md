# Set up development environment

### Introduction

One common challenge with development with an organization is getting developers machines configured properly. It can be even further complicated when you need all the developers to be working in a uniform environment and all configured the same way. Within the Microsoft cloud, Microsoft has created Microsoft Dev Box as a solution for addressing this issue. It allows an organization to create a virtual desktop environment geared specifically at developers so you can ensure that your developers are working from machines configured the same way with the same version of software deployed to them. In this exercise, you'll configure a Microsoft Dev Box environment.

## Task 01: Deploy the necessary prerequisites required for a Microsoft Dev Box

### Description

Microsoft Dev Box requires the proper infrastructure and resources to be deployed in Azure before you can start deploying virtual machines to users. In this excercise, you'll prep your azure environment for deploying Dev Boxes.

### Success Criteria

- Create a Dev Center
- Create a Project
- Setup the dev center so dev boxes are joined to Azure AD (Microsoft Entra)
  - Create a vnet (if needed) - use an existing vNet or create a new one for the network connection
  - Create a network connection and add it to the dev center

### Learning Resources

### Tips

- [Microsoft Dev Box Prerequisites](https://learn.microsoft.com/azure/dev-box/quickstart-configure-dev-box-service?tabs=AzureADJoin#prerequisites)
- [Dev Box Catalogs](https://learn.microsoft.com/azure/deployment-environments/how-to-configure-catalog)

## Task 02: Setup an image

### Description

Typically, when a developer needs to have an environment configured for their development tasks, they need software deployed, specific versions of tools and source code, any other configurations for their development tasks. This can be a time consuming process when onboarding a new developer or switching to a new development project. If developers are working on multiple projects, they may even have challenges with those different projects requiring different versions of the same software. In this task, you'll setup an image to be used for Dev Box. This enables most of these things to be preconfigured so as soon as a user is onboarded or needs to work on a different project, an environment can be quickly spun up for them from an image.

### Success Criteria

- Create a compute gallery & add it to the Dev center
- Configuration
  - Disable IE Enhanced Security Configuration
- Must include the following software
  - PowerShell  
  - Docker: https://docs.docker.com/desktop/install/windows-install/
    - Uncheck "Use WSL 2 instead of Hyper-V"
    - Will require a restart. Do it last, or just wait to click the restart button until finished installing all the others below.
  - Visual Studio Code (System Installer): https://code.visualstudio.com/Download
  - Java (for JMeter): https://www.java.com/en/download/
  - Apache JMeter: https://dlcdn.apache.org//jmeter/binaries/apache-jmeter-5.6.2.zip
    - Extract the apache-jmeter-5.6.2 folder to C:\
  - Azure Storage Explorer (Install for all users): https://azure.microsoft.com/products/storage/storage-explorer/
  - Git bash: https://git-scm.com/download/win
    - 64bit git for Windows
    - You may want to set the default editor to VSCode, keep the defaults for everything else.
  - .NET SDK 6.0 LTS: https://dotnet.microsoft.com/download/visual-studio-sdks
    - .NET 6.0 x64 Visual Studio 2022 SDK
- Sysprep
  - If you get an error, you may need to Uninstall OneDrive from the programs and features
  - Open PowerShell as an admin and run: Get-AppxPackage -AllUsers Microsoft.OneDriveSync | Remove-AppxPackage -AllUsers
- Create the new image
  - For the VM image definition, create a new one with the name "DevBoxProject", leave everything else the default
  - VM version number can be 1.0.0
  - Default storage sku: Premium SD LRS
  - Review + create --> Create

### Learning Resources

- [Create a Dev Box custom image](https://learn.microsoft.com/azure/dev-box/how-to-customize-devbox-azure-image-builder)
- [Create a Dev Box Definition](https://learn.microsoft.com/azure/dev-box/quickstart-configure-dev-box-service?tabs=AzureADJoin#3-create-a-dev-box-definition)
- [Generalize a VM](https://learn.microsoft.com/azure/virtual-machines/generalize)
- [Create an image from a VM](https://learn.microsoft.com/azure/virtual-machines/capture-image-portal)


### Tips

- For the image creation, use the VM Size: Standard D4s v3 (4 vcpus, 16 GiB memory) with a Windows 10 or 11 Image
- For security, once you create the VM with RDP access, go into the networking settings and set the RDP access Source to be limited to your IP address

## Task 03: Deploy a Microsoft Dev Box

### Description

In this task you'll take everything you configured in the first two tasks and use it to deploy a new dev box. Once it's deployed, you'll verify that you can log in to the Dev Box and that the tools you configured in your custom image are included in the dev box.

### Success Criteria

- Create a dev box definition
- Create a dev box pool within your project
- Have a dev box deployed that you can log into and use the tools you configured in the custom image

### Learning Resources

- [Quickstart: Configure Microsoft Dev Box](https://learn.microsoft.com/azure/dev-box/quickstart-configure-dev-box-service?wt.mc_id=mdbservice_acomdoc01_webpage_cnl&tabs=AzureADJoin)
- [Create a dev box by using the developer portal](https://learn.microsoft.com/azure/dev-box/quickstart-create-dev-box?wt.mc_id=mdbservice_acomdoc02_webpage_cnl)

### Tips

- Dev Portal for logging in: https://devportal.microsoft.com/
- Ensure you assign RBAC roles for the user to access the dev portal