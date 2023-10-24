# Exercise 01 - Set up development environment (120 minutes)

## Prerequisites

During the labs in the workshop, you will need:

- An AAD tenant where you are a global admin. and an Azure subscription in that same tenant. 
- M365 E5 licenses (or a trial for these licenses configured) in that same tenant .

**[MCAPS non-prod subscriptions](https://dev.azure.com/OneCommercial/NoCode/_wiki/wikis/NoCode.wiki/12/Hybrid-Subscription) are the most convenient way for you to meet all these pre-requites**, and the lab activities assume that you have configured an external subscription via [https://aka.ms/MCAPSNewAzureSub](https://aka.ms/MCAPSNewAzureSub).

## Introduction

Munson's Pickles and Preserves has a large number of developers on their team, working on a variety of projects. One of those projects is the Team Messaging System. A challenge that Munson's Pickles and Preservers has with development is getting developers machines configured properly in a timely manner when they need to work on these projects. 

Developers occasionally will also be working on two projects simultaneously that require different configurations or various version for the same software packages, making it challenging to keep their development computer consistent when working on the two projects. In the past, MPP has also had issues with one developer’s computer being configured slightly differently that another developer’s so the code they were developing behaved differently in different circumstances. 

Within Microsoft Azure, Microsoft has created Microsoft Dev Box as a solution for addressing this issue. It allows an organization to create a virtual desktop environment geared specifically at developers. This allows organizations to ensure that developers are working from machines configured the same way with the same version of software deployed to them. In these next few tasks, you'll configure a Microsoft Dev Box environment.

## Task 01 - Deploy the necessary prerequisites required for a Microsoft Dev Box

Before standing up Dev Boxes for the developers at Munson's Pickles and Preservers to use, they have some prerequisite tasks that need to be done in order to ensure that developers have the right permissions and network access in place and can access the resources required for their. 

In this first task you will walk through some of those base configurations that are needed to stand up a Microsoft Dev Box in Azure. This includes a Dev Center where all the projects will live. The projects where you can add the virtual machines configured specifically for that project, as well as the network these development virtual machines will be attached to. This ensures users can login with the Azure AD account as well as that they are able to access other resources on the network.

### Description

Microsoft Dev Box requires the proper infrastructure and resources to be deployed in Azure before you can start deploying  machines for users. In this exercise, you'll prep your Azure environment for deploying Dev Boxes.

### Success Criteria

- Create a Dev Center
- Create a Project
- Setup the dev center so dev boxes are joined to Azure AD (Microsoft Entra)
  - Create a vNet (if needed) - use an existing vNet or create a new one for the network connection
  - Create a network connection and add it to the dev center

### Learning Resources

- [Microsoft Dev Box Prerequisites](https://learn.microsoft.com/azure/dev-box/quickstart-configure-dev-box-service?tabs=AzureADJoin#prerequisites)

### Tips

- [Step-by-step to create a dev center and project](https://learn.microsoft.com/en-us/azure/dev-box/quickstart-configure-dev-box-service?tabs=AzureADJoin#1-create-a-dev-center)
- [Step-by-step to create a Dev Box network and network connection](https://learn.microsoft.com/en-us/azure/dev-box/quickstart-configure-dev-box-service?tabs=AzureADJoin#2-configure-a-network-connection)
- [If you want to use an ARM template, configure a Dev Box by using an ARM template](https://learn.microsoft.com/en-us/azure/dev-box/quickstart-configure-dev-box-arm-template)

### Related features not covered in this lab

- [Template Catalogs for Azure Deployment Environments](https://learn.microsoft.com/azure/deployment-environments/how-to-configure-catalog)
- [Environment definitions for Dev Center Projects](https://learn.microsoft.com/en-us/azure/deployment-environments/configure-environment-definition)

## Task 02 - Setup an image

### Description

Typically, when a developer needs to have an environment configured for their development tasks, they need software deployed, specific versions of tools and source code, any other configurations for their development tasks. This can be a time-consuming process, particularly when onboarding a new developer or switching to a new development project. 

If developers are working on multiple projects, they may even have challenges with those different projects requiring different versions of the same software. In this task, you'll setup an image to be used for Dev Box. This enables most of these things to be preconfigured so as soon as a user is onboarded or needs to work on a different project, an environment can be quickly spun up for them from an image.

### Success Criteria

- Create a compute gallery & add it to the Dev center
- Create a custom image using Windows 11 Enterprise, version 22H2 - x64 Gen2
  - Note: Make sure you select enterprise and not pro. Pro is not supported for Dev Box.
  - Must include the following software
    - Docker (without WSL): [https://docs.docker.com/desktop/install/windows-install/](https://docs.docker.com/desktop/install/windows-install/)
      - **Note:** Make sure to UNCHECK to use WSL during installation. This will cause issue with docker
    - Visual Studio Code (System Installer): [https://code.visualstudio.com/Download](https://code.visualstudio.com/Download)
      - Make sure this finishes before installing Git so you can set it as the default editor
    - Java (for JMeter): [https://www.java.com/en/download/](https://www.java.com/en/download/)
    - Apache JMeter: [https://dlcdn.apache.org//jmeter/binaries/apache-jmeter-5.6.2.zip](https://dlcdn.apache.org//jmeter/binaries/apache-jmeter-5.6.2.zip)
      - Extract the apache-jmeter-5.6.2 folder to C:\
    - Azure Storage Explorer (Install for all users): [https://azure.microsoft.com/products/storage/storage-explorer/](https://azure.microsoft.com/products/storage/storage-explorer/)
    - Git bash: [https://git-scm.com/download/win](https://git-scm.com/download/win)
      - 64bit git for Windows Setup
      - You may want to set the default editor to VSCode, keep the defaults for everything else.
    - .NET SDK 6.0 LTS: [https://dotnet.microsoft.com/download/visual-studio-sdks](https://dotnet.microsoft.com/download/visual-studio-sdks)
      - .NET 6.0 x64 Visual Studio 2022 SDK
  - You should make sure you reboot at this point in time and Docker is running before doing the sysprep
  - **Important:** At this point in time, take a snapshot of the OS disk on your VM so you can make updates to it later if needed. Once you sysprep a VM you can't boot it back up again to make changes.
  - Sysprep
    - Delete C:\Windows\Panther and empty the recycle bin
    - In the command prompt, CD to C:\Windows\System32\SysPrep and run ```sysprep.exe /oobe /generalize /shutdown```
- Create the new image
  - For the VM image definition, create a new one with the name "DevBoxProject", leave everything else the default
  - VM version number can be 1.0.0
  - Default storage SKU: Premium SD LRS
  - Review + create --> Create
  - It can take several minutes for the image to be created.

### Learning Resources

- [Create an Azure Compute gallery](https://learn.microsoft.com/en-us/azure/virtual-machines/create-gallery?tabs=portal%2Cportaldirect%2Ccli2)
- [Create a Dev Box Definition](https://learn.microsoft.com/azure/dev-box/quickstart-configure-dev-box-service?tabs=AzureADJoin#3-create-a-dev-box-definition)
- [Generalize a VM](https://learn.microsoft.com/azure/virtual-machines/generalize)
- [Create an image from a VM](https://learn.microsoft.com/azure/virtual-machines/capture-image-portal)

### Tips

- For the image creation, use the VM Size: Standard D4s v3 (4 vCPUs, 16 GiB memory) with a Windows 10 or 11 Image
- After running sysprep on a virtual machine in Azure, you can't use it again, so you can delete the VM along with it's associated resources (NIC, Disk, etc.)
- For security, once you create the VM with RDP access, go into the networking settings and set the RDP access Source to be limited to your IP address
- If you do need to make changes to your VM after you ran sysprep, you'll need to restore from the snapshot
    1. Create a new disk from the snapshot
    2. Create a new VM from the new managed disk
    3. Make your updates on the VM
    4. Create a new snapshot
    5. Run sysprep again.

## Task 03 - Deploy a Microsoft Dev Box

### Description

In this task you'll take everything you configured in the first two tasks and use it to deploy a new dev box. Once it's deployed, you'll verify that you can log in to the Dev Box and that the tools you configured in your custom image are included in the dev box.

### Success Criteria

Now that you have the custom image with all your software installed, developers will need to be able to use that image to create and run their own development box.

- Create a dev box definition with a custom image
- Create a dev box pool within your project
- Create and log in to your own dev box and use the tools you configured in the custom image

### Learning Resources

- [Quickstart: Configure Microsoft Dev Box](https://learn.microsoft.com/azure/dev-box/quickstart-configure-dev-box-service?wt.mc_id=mdbservice_acomdoc01_webpage_cnl&tabs=AzureADJoin)
- [Create a dev box by using the developer portal](https://learn.microsoft.com/azure/dev-box/quickstart-create-dev-box?wt.mc_id=mdbservice_acomdoc02_webpage_cnl)
- [Configure a managed identity for a dev center](https://learn.microsoft.com/en-us/azure/deployment-environments/how-to-configure-managed-identity)

### Tips

- To access the Dev Portal for logging in as a user visit: [https://devportal.microsoft.com/](https://devportal.microsoft.com/).
- Ensure you assign the [appropriate RBAC roles](https://learn.microsoft.com/en-us/azure/dev-box/how-to-dev-box-user) for the users who to access the dev portal to login to a virtual machine.
- The first time you need a virtual machine, you need to first create it. It can take a little bit to create the VM the first time.
- **Note**: It's possible that Docker for Desktop causes issues following the creation of your Dev Box due to the custom image. To resolve, it may need to be uninstalled/reinstalled once your Dev Box is created.
