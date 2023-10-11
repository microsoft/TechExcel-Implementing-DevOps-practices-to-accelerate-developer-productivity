# Set up development environment

### Introduction

One common challenge with development with an organization is getting developers machines configured properly. It can be even further complicated when you need all the developers to be working in a uniform environment and all configured the same way. Within the Microsoft cloud, Microsoft has created Microsoft Dev Box as a solution for addressing this issue. It allows an organization to create a virtual desktop environment geared specifically at developers so you can ensure that your developers are working from machines configured the same way with the same version of software deployed to them. In this exercise, you'll configure a Microsoft Dev Box environment.

## Task 01: Deploy the necessary prerequisites required for a Microsoft Dev Box

### Description

Microsoft Dev Box requires the proper infrastructure and resources to be deployed in Azure before you can start deploying virtual machines to users. In this excercise, you'll prep your azure environment for deploying Dev Boxes.

### Success Criteria

- Create a Dev Center
- Create a Project

### Learning Resources

### Tips

- [Microsoft Dev Box Prerequisites](https://learn.microsoft.com/en-us/azure/dev-box/quickstart-configure-dev-box-service?tabs=AzureADJoin#prerequisites)

## Task 02: Setup an image

### Description

Typically, when a developer needs to have an environment configured for their development tasks, they need software deployed, specific versions of tools and source code, any other configurations for their development tasks. This can be a time consuming process when onboarding a new developer or switching to a new development project. If developers are working on multiple projects, they may even have challenges with those different projects requiring different versions of the same software. In this task, you'll setup an image to be used for Dev Box. This enables most of these things to be preconfigured so as soon as a user is onboarded or needs to work on a different project, an environment can be quickly spun up for them from an image.

### Success Criteria

### Learning Resources

### Tips

- [Create a Dev Box custom image](https://learn.microsoft.com/en-us/azure/dev-box/how-to-customize-devbox-azure-image-builder)
- [Create a Dev Box Definition](https://learn.microsoft.com/en-us/azure/dev-box/quickstart-configure-dev-box-service?tabs=AzureADJoin#3-create-a-dev-box-definition)

## Task 03: Deploy a Microsoft Dev Box

### Description

In this task you'll take everything you configured in the first two tasks and use it to deploy a new dev box. Once it's deployed, you'll verify that you can log in to the Dev Box and that the tools you configured in your custom image are included in the dev box.

### Success Criteria

- Have a dev box deployed that you can log into and use the tools you configured in the custom image

### Learning Resources

### Tips