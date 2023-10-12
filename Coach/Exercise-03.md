# Exercise 03 - Improve and deploy your application

## Task 1 - Configure Microsoft Copilot on your Dev Box

TODO

## Task 2 - Change the application

- To create a new GitHub Issue, select "Issues" from the GitHub options menu and then select the "New issue" button. Enter an appropriate title and comment and then choose "Submit new issue" to create it.
- To create a new branch in GitHub, select "<> Code" from the GitHub options menu. Then, choose the "main" drop-down menu below the GitHub repository title and select "View all branches" to navigate to the branch list. Select the green "New branch" button and then enter your new branch name and select "Create new branch" to complete the process.
- On the dev box, run `git fetch` from the Git bash shell to fetch the latest changes. Then, run `git checkout {new branch name}` to make that the working branch.
- The only necessary app modification is in `Application\src\RazorPagesTestSample\Data\Message.cs` and involves changing line 12.
- To commit the change to the repository, add the changes to the working space with a command like `git add .`. Then, run a command such as `git commit -m "Resolves #6"`, assuming that 6 is the relevant issue number. Run `git push` to push the changes from the remote branch to GitHub.
- To create a pull request in GitHub, select "Pull requests" from the GitHub options menu. Then, select the "New pull request" option. The base branch should be "main" and the compare branch should be the new branch they've created. Then, select the green "Create pull request" button. On the pull request form, learners may add a reviewer by selecting the gear icon in the "Reviewers" section and then selecting a user from the drop-down list or typing in a username. After filling in pull request details, select the "Create pull request" button to create the pull request and alert the reviewer. Note that the GitHub website may prompt learners to create a pull request on the main page or on the Pull requests page, and they may choose this option as a shortcut to save a couple of steps.
- To review a pull request, a learner should open the pull request, either directly through the link in the e-mail GitHub sends or by navigating to the appropriate repository and choosing the pull request from the "Pull requests" menu. The learner should navigate to the "Files changed" tab and review any changes, ensuring that line 12 `Message.cs` now supports 250 characters instead of 200. There may also be one or more unit test updates to review. If the review is successful, the reviewer may choose the green "Review changes" button on the right-hand side, choose the "Approve" radio button, and then select "Submit review" to complete the task. **Note:** a separate team member must review the pull request, not the learner who owns the repository.
- To complete a pull request, 

### Introduction

We have a Dev Box configured, so now it is time to ensure that everything works as expected. We can do this by making a simple change to the demo application and ensuring that we are able to commit this code to GitHub from the Dev Box.

### Description

Developers at Munson's Pickles and Preserves like the Team Messaging System in general, but they consistently bring up issues with one aspect of the application: it limits messages to 200 characters or fewer. Developers have made a case that the app should support messages of up to 250 characters in length instead.

In this task, you will modify the application to support 250 characters instead of 200. You will also follow a feature branching strategy and use a pull request to bring your change into the `main` branch. If you wish to complete this task using a Test-Driven Design approach, please read the **Advanced Challenges (optional)** section below before making any changes.

1. Create a new GitHub Issue indicating the desired outcome.
2. Create a new branch in GitHub for your changes.
3. Fetch changes on your Dev Box and check out the new branch you have created.
4. Make any necessary modifications to the .NET application to change the limit on message length from 200 to 250. Be sure to update any error messages or other indicators as well so you do not have incorrect error messages or display messages. You may also use this as an opportunity to demonstrate GitHub Copilot to a customer.
5. Commit the changes to the feature branch in your GitHub repository. Ensure that your commit message includes verbiage to the extent of "Resolves #x", where 'x' is the issue number for the issue you created. This will allow you to auto-close the issue once your code makes it into the `main` branch via pull request.
6. Create a pull request from your feature branch into `main`. Add another learner on your team as a reviewer for this pull request. Your code reviewer should read through the changes, comment on any issues, and approve the review if the change looks good.
7. After your team member reviews and approves the pull request, complete the pull request to merge the changes from your feature branch into the `main` branch. Delete the feature branch upon completion.

### Success Criteria

- You are able to run your web application successfully after making changes.
- You are able to insert a message of length 250.
- You are not able to insert a message of length 251.
- All of your changes are now in the `main` branch for your GitHub repository.
- The issue you created for this ticket is now closed and the feature branch has been deleted.

## Learning Resources

- [GitHub flow](https://docs.github.com/en/get-started/quickstart/github-flow)
- [Linking a pull request to an issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue)
- [Unit testing C# in .NET Core using dotnet test and xUnit](https://learn.microsoft.com/en-us/dotnet/core/testing/unit-testing-with-dotnet-test)
- [Creating parameterised tests in xUnit with InlineData, ClassData, and MemberData](https://andrewlock.net/creating-parameterised-tests-in-xunit-with-inlinedata-classdata-and-memberdata/)

### Advanced Challenges (optional)

Your code change did not affect any unit tests. In practice, this is a sign that we are missing at least one valid unit test case. As part of your code changes, introduce unit tests which test the insertion of a message of length 199, a message of length 200, a message of length 201, a message of length 249, a message of length 250, and a message of length 251.

Use GitHub Copilot to assist in creating the appropriate unit tests.

## Task 3 - Introduce infrastructure as code

### Introduction

Now that we have some code, we need an environment to deploy it to! The term Infrastructure as Code (IaC) refers to using code-based templates repeatedly and consistently to create the dev, test, and prod infrastructure environments. We can automate the deployment of Azure services we need with an Azure Resource Manager (ARM) template, which we can invoke as part of a GitHub Actions workflow.

Review the following articles:

- [Azure Resource Manager overview](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview)
- [Create Azure Resource Manager template](https://docs.microsoft.com/en-us/azure/azure-resource-manager/how-to-create-template)

### Description

In this task, you will deploy out new Azure infrastructure for the Munson's Pickles and Preserves Team Messaging System. You will deploy this to three environments: `dev`, `test`, and `prod`. Each environment will have its own Web App, though for the sake of this proof of concept, all environments will share a single resource group, App Service Plan, Application Insights instance, and Azure Container Registry.

We will use GitHub Actions to automate the deployment of our Azure infrastructure. For our application, we will deploy 3 environments: `dev`, `test` and `prod`. Each environment will have its own Web App. For the sake of this proof of concept, however, all of our environments will share a single Resource Group, App Service Plan, Application Insights instance, and Azure Container Registry.

**NOTE:** In a real deployment, you likely would not share these resources between environments.

1. Review the Bicep template. Notice how it includes the configuration settings for an App Service Plan, a Web App, Application Insights, and Azure Container Registry into your resource group.
2. Review the use of the `uniqueString` function. This helps create unique names for your resources. This function is not random, but is instead a hash function based on your resource group ID, which provides a consistent but likely unique 13-character string. This function is useful to avoid naming conflicts in Azure.
3. Review the `environment` and `location` parameters. These have default values but you are able to override them as well in order to create other resources in different environments.
4. Create a resource group. Then, create a service principal to log into Azure. The service principal should have Contributor access to the resource group you just created.
5. Create a GitHub repository-level secret to store the service principal's credentials. Create a second repository-level secret to store the name of the Azure resource group.
6. Create a new GitHub Actions workflow called `deploy.yml`. This workflow should run on a manual trigger--that is, *not* triggered by a push or pull request.
7. Configure the workflow to accomplish the following tasks:
    1. Use a service principal to log into Azure using your secret and configuration variable values.
    2. Use the "Deploy Azure Resource Manager (ARM) Template" action to call the Bicep template in your repo.
    **NOTE:** The name is a little confusing here as this action supports both ARM and Bicep as the template file.  This is because Bicep is a transparent abstraction of ARM.  For more details check out [this article](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/overview?tabs=bicep).
8. Manually run your workflow. When your workflow completes successfully, go to the Azure portal to see the `dev` environment.
9. Run the workflow a second time, this time overriding the environment parameter with `test` instead of `dev`. When your workflow completes successfully, go to the Azure portal to see the new `test` App Service.
10. Run the workflow a third time, this time overriding the environment parameter with `prod` instead of `dev`. When your workflow completes successfully, go to the Azure portal to see the new `prod` App Service.

### Success Criteria

- Your `deploy.yaml` workflow completes without any errors and overrides the `environment` parameter when calling the Bicep template.
- Your resource group contains 6 resources: 3 App Services (dev, test, prod), 1 Application Insights, 1 App Service plan and 1 Container registry.

### Learning Resources

- [Azure Login GitHub Action](https://github.com/Azure/login)
- [What is Infrastructure as Code?](https://docs.microsoft.com/en-us/azure/devops/learn/what-is-infrastructure-as-code)
- [Secrets in GitHub Actions](https://docs.github.com/en/actions/security-guides/encrypted-secrets)
- [Configuration Variables in GitHub Actions](https://docs.github.com/en/actions/learn-github-actions/variables#creating-configuration-variables-for-a-repository)
- [Deploy Bicep and Azure Resource Manager templates by using GitHub Actions](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/deploy-github-actions)
- [Overriding ARM template parameters](https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/deploy-cli#parameters)

If you are interested in learning more about Infrastructure as Code, there are multiple [What the Hacks](https://aka.ms/wth) that cover it in greater depth:

- [Infrastructure As Code: Bicep](https://microsoft.github.io/WhatTheHack/045-InfraAsCode-Bicep/)
- [Infrastructure As Code: ARM Templates & PowerShell DSC](https://microsoft.github.io/WhatTheHack/011-InfraAsCode-ARM-DSC/)
- [Infrastructure As Code: Terraform](https://microsoft.github.io/WhatTheHack/012-InfraAsCode-Terraform/Student/)
- [Infrastructure As Code: Ansible](https://microsoft.github.io/WhatTheHack/013-InfraAsCode-Ansible/Student/)

### Advanced Challenges (optional)

Instead of changing the environment variable for each environment that we want to create in `deploy.yml`, you can configure the workflow to prompt the user to enter the environment name before the workflow runs. This eliminates the need to hardcode the environment name.

- Configure your workflow to collect the environment name as a [workflow input](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#onworkflow_callinputs) and use that value to override the environment parameter when calling the Bicep template.

## Task 4 - Build, Push, and Deploy Changes

### Introduction

With our Azure resources created, we have laid out the foundation for our application. Now, we must connect our source code with its destination. The first step in this journey is called Continuous integration (CI).

Continuous Integration is the process of merging local code changes into source control and may include steps to automatically build and test the code. Effective Continuous Integration allows developers rapidly to iterate and collaborate. It also helps ensure that newly added code does not break the current application.

Review the following articles:

- [About continuous integration](https://docs.github.com/en/actions/building-and-testing-code-with-continuous-integration/about-continuous-integration)
- [Setting up continuous integration using workflow templates](https://docs.github.com/en/actions/building-and-testing-code-with-continuous-integration/setting-up-continuous-integration-using-github-actions)

The next practical step is to extend our workflow with steps to build a Docker image and push it to Azure Container Registry (ACR). After that, we will configure the Web App to pull this image from ACR.

Containers are a great way to package and deploy applications consistently across environments. If you are new to containers, there are 3 key steps to creating and publishing an image, which we will cover below. Because this training focuses on DevOps and not containers, we've strived to make this task as straightforward as possible.

1. `docker login` - You need to log into the container registry into which you will push your image. These generally require authentication from an authorized user, as that will prevent unauthorized individuals from publishing images in your registry.

2. `docker build` - The second step is to request that Docker (running locally on your machine or on your build server) create the container image. A *critical* component of this is the `Dockerfile`, which gives instructions to Docker on how to build the image, files to copy, ports to expose, and startup commands.

3. `docker push` - Once you have created your Docker image, you need to store it in a container registry. Container registries are secure, centralized locations to store Docker images. Docker supports a `push` command that copies the Docker image to the registry in the repository you specify. A repository is a logical way of grouping and versioning Docker images, and Docker repositories do not necessarily need to match the structure of your GitHub repositories.

After we automate the build process, we also want to automate the release process. This is a technique called Continuous Delivery (CD). Please take a moment to review this brief article talking about why Continuous Delivery is important.

- [What is Continuous Delivery?](https://docs.microsoft.com/en-us/azure/devops/learn/what-is-continuous-delivery)

### Description

The team at Munson's Pickles and Preserves would like to see how to implement Continuous Integration and Continuous Delivery (CI/CD) in the context of using Docker containers to build code and GitHub Actions workflows to deploy that code.

1. Create a new GitHub Actions `.NET` workflow. You should be able to find this in the "Continuous integration workflows" section.
2. Review the workflow contents. Then, under the "Setup .NET Core" step, ensure that the .NET version is `6.0.x` in order to match the version defined in the Team Messaging System.
3. Ensure that the workflow is configured to trigger on both pushes and pull requests, and then configure both of these triggers with path triggers which only act on changes in the `/Application` folder.
4. Update each of the three pre-defined GitHub Actions workflow steps used to build the .NET application. For each of the three steps, you will need to update each command to pass the relative path to the appropriate `.csproj` file as an argument. The three steps are `restore`, `build`, and `test`. For `restore` and `build`, the argument you will pass in is the path to the application csproj file. For `test`, the argument you will pass in is the path to the unit test csproj file.
5. Test the workflow by making a small change to application code, such as adding a comment to a file. Commit and push the change, and then ensure that the workflow completes successfully. At this point, any changes pushed to the `/Application` folder will automatically trigger the workflow--this is Continuous Integration in action.
6. At the top of your workflow file, create 4 environment variables:
    - `registryName` - the full server address of your ACR instance. Set this to "`registryName`.azurecr.io", replacing `registryName` with the `<prefix>devopsreg` value in your ARM template file on line #26.
    - `repositoryName` - The repository to target in the registry. Set this to "`techboost/dotnetcoreapp`".
    - `dockerFolderPath` - The path to the folder that contains the Dockerfile. You will need to point to the folder `Application/src/RazorPagesTestSample`.
    - `tag` - This needs to be a unique value each time, as this is used to version the images in the repository. GitHub makes [environment variables](https://docs.github.com/en/free-pro-team@latest/actions/reference/context-and-expression-syntax-for-github-actions#github-context) available to help with this. Set `tag` to the [github.run_number](https://www.bing.com/search?q=%24%7B%7Bgithub.run_number%7D%7D&form=QBLH&sp=-1&pq=%24%7B%7Bgithub.run_number%7D%7D&sc=0-22&qs=n&sk=&cvid=D84DA66323DC4E14BD794F90FCFD90D3) environment variable.
7. Go to the Azure Portal and get the username, password, and login server for your ACR instance and save these three as GitHub secrets. The names for these secrets should be `ACR_USERNAME`, `ACR_PASSWORD`, and `ACR_LOGIN_SERVER`, respectively.
8. Add a second **job** to your existing .NET Core workflow. Make sure the first step in your second job includes `- uses: actions/checkout@v2`.
9. To authenticate to the registry, add a step named `Docker login` with the following as the `run` command: `docker login $registryName -u ACR_USERNAME -p ACR_PASSWORD`. Be sure to use the GitHub secrets for ACR username and ACR password instead of the string literals "ACR_USERNAME" and "ACR_PASSWORD".
10. To build your image, add a step named `Docker build` with the following as the `run` command: `docker build -t $registryName/$repositoryName:$tag --build-arg build_version=$tag $dockerFolderPath`
11. To push your image to ACR, add a step named `Docker push` with the following as the `run` command: `docker push $registryName/$repositoryName:$tag`
12. Test the workflow by adding a comment to one of the application files. Commit, push, monitor the workflow. Verify that the GitHub Actions workflow builds a new container image, tags that image, and pushes the image to ACR.
13. Configure your `dev` environment to pull the latest container image from ACR.
   13.1 - Log into Azure using your service principal, if needed ([hint](https://docs.microsoft.com/en-us/azure/app-service/deploy-container-github-action?tabs=service-principal#tabpanel_CeZOj-G++Q-3_service-principal)).
   13.2 - Use the `Azure/webapps-deploy@v2` [action](https://github.com/Azure/webapps-deploy) to update the Web App to pull the latest image from ACR. Key parameters to configure include:
      - `app-name` - the name of the wep app instance to target
      - `images` - the path to the image you pushed to ACR
14. Make a small change to your application, such as adding a string of display text to `Pages/Index.cshtml` in the app. From there, commit and push the change. Then, monitor the workflow and see if the change shows up on the dev instance of the website.
15. Configure your workflow to deploy to your `test` and `prod` environments. Ensure that each environment includes a manual approval step before deployment.

### Success Criteria

- Any changes pushed to the `/Application` folder automatically trigger the workflow.
- .NET Core restore, build, and test steps complete successfully.
- Each successful workflow run builds a new container image with a unique tag. Each container image should be available in the Azure Container Registry after each successful workflow run.
- A small change to a file such as `Pages/Index.cshtml` automatically shows up on the website running in the `dev` environment, `<prefix>devops-dev`.azurewebsites.net.
- Manual approval is required to deploy to the `test` and `prod` environments.

### Learning Resources

- [Introduction to GitHub Actions](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/introduction-to-github-actions)
- [.NET Core Action to build and test](https://github.com/actions/starter-workflows/blob/dacfd0a22a5a696b74a41f0b49c98ff41ef88427/ci/dotnet-core.yml)
- [Understanding workflow path filters](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#onpushpull_requestpaths)
- [dotnet commands](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet#dotnet-commands)
- [GitHub Actions for Azure](https://github.com/Azure/actions)
- [Environment variables](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#env)
- [Introduction to GitHub Actions](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/introduction-to-github-actions)
- [Understanding workflow path filters](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions#onpushpull_requestpaths)
- [Authenticate with an Azure container registry](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-authentication#admin-account)
- [GitHub Actions for Azure](https://github.com/Azure/actions)
- [Deploy a custom container to App Service using GitHub Actions](https://docs.microsoft.com/en-us/azure/app-service/deploy-container-github-action?tabs=service-principal#tabpanel_CeZOj-G++Q-3_service-principal)
- [Using environments for deployment](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment)

### Advanced Challenges (optional)

If the GitHub Actions workflow fails, GitHub sends an e-mail to the repository owner. Sometimes, however, you may wish to log the failure or even create a GitHub issue upon failure. Add a step to your workflow which creates a GitHub issue if there is a failure at any point in the workflow.
