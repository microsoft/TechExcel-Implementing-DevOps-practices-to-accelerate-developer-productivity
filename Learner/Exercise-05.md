# Make things secure

## Task 01

### Introduction

In the previous steps, we successfully implemented an end-to-end CI/CD pipeline! However, our current workflow will immediately promote every small change directly to production. Typically, you would want to avoid working directly against the main branch in your repository to avoid conflicts and protect the production environment. 

With GitHub, we can solve these challenges using a practice called branching. Some may refer to this as the [GitHub flow](https://guides.github.com/introduction/flow/). When a developer wants to make a change, add a feature, or fix a bug, he or she begins by creating a new 'branch' or copy of the main codebase. Then, the developer makes changes and commits them. He or she creates a pull request to merge these changes back into the main branch. This pull request may or may not involve some testing or discussion. Finally, changes are merged back into the main codebase, and the branch can be deleted. 

In this challenge, you will practice this flow. Additionally, GitHub offers a feature for explicitly protecting against changes directly to the main branch. These are called branch protection rules, and you will start by implementing one.

### Description

- Create a branch protection rule which prevents developers from committing changes to the main branch in the repository.
- Create a feature branch, make a small change to the code (i.e.,`/Application/aspnet-core-dotnet-core/Views/Home/Index.cshtml`), and sync this branch with the GitHub repository.
- Define a code owner for the `/Application` directory. Your branch policy should require a review from the code owner.
- Create and complete a Pull Request, merging your code change into the protected branch.

### Success Criteria

- You have a branch protection rule which prevents changes from being commited to your main branch.
- Changes to the application (i.e.,`/Application/aspnet-core-dotnet-core/Views/Home/Index.cshtml`) are committed to a feature branch.
- Before a pull request is completed:
  - A code owner must approve the changes ([hint](https://docs.github.com/en/free-pro-team@latest/github/creating-cloning-and-archiving-repositories/about-code-owners))
  - A CI workflow is run against the feature branch ensuring the application passes a build and test ([hint](https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/enabling-required-status-checks))
- A completed pull request merges with the protected branch and is automatically deployed to the dev environment.

### Learning Resources

- General information about protected branches can be found [here](https://docs.github.com/en/github/administering-a-repository/about-protected-branches), with more configuration specifics [here](https://docs.github.com/en/github/administering-a-repository/configuring-protected-branches).
- General information about branches can be found [here](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-branches), with more specifics about creation and deletion [here](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-and-deleting-branches-within-your-repository).
- General information about pull requests can be found [here](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests), with more specifics about [creating](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request) and [reviewing](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/reviewing-changes-in-pull-requests).
- [About code owners](https://docs.github.com/en/free-pro-team@latest/github/creating-cloning-and-archiving-repositories/about-code-owners)
- [Enabling required status checks](https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/enabling-required-status-checks)
- [About required reviews for pull requests](https://docs.github.com/en/free-pro-team@latest/github/administering-a-repository/about-required-reviews-for-pull-requests)

### Tips

- If your GitHub account was created on the 'Free' tier, then in order to create a Branch Protection rule your repository must be public. To change a repository from private to public, visit the 'Settings' tab, and scroll to the bottom where you have the option to 'Change visibility.'
- If using the git command line interface, you can find a number of sample git commands that are useful for branching [here](https://gist.github.com/JamesMGreene/cdd0ac49f90c987e45ac). (Make sure to focus on the 'git' commands, rather than 'gitflow'.)
- If using the git command line interface, try adding '--help' after a command to get helpful information about arguments and usage.

## Task 02

### Introduction

Our application is up and running! We are even using a proper Git Flow to protect against unintended changes to our main branch, and we are recording application telemetry into App Insights. Before we are truly production ready, though, there is one topic we have to cover--security.

One good DevOps practice is to enable protections against code-level vulnerabilities, and GitHub provides a number of useful features in this area. First, there are Issues, which allow developers or users to open 'tickets' indicating bugs to be fixed or potential vulnerabilities. If your organization prefers security flaws to be reported in a location other than GitHub, you have the option to provide a custom Security policy which describes the process for reporting. 

In addition to these manual processes, GitHub also provides automated tools for scanning code for common errors. In this challenge, you will utilize the built in Dependabot which provides alerts if your repository contains libraries, packages, or external dependencies with known vulnerabilities. You will also set up a workflow with CodeQL which can scan your source code for common coding errors or basic security flaws.

### Description

In this challenge, you will improve the security of your repository using some of GitHub's built-in tools.

- Find the repository's Security policy. If there is an existing policy, make an edit and merge your change back into the main branch. Otherwise, go ahead and create a policy using the template provided. GitHub Security policies are Markdown documents that indicate the preferred way to report security vulnerabilities for the repository.
- Enable Dependabot alerts for the repository. Dependabot is an automated tool that creates a pull request when any dependencies in the code base has a known vulnerability.
- Finally, set up and run a Code scanning workflow for the repository using GitHub's 'CodeQL Analysis.' This workflow can run either on each pull request or on a schedule, and it checks your code for common vulnerabilities or errors.

### Success Criteria

- In GitHub, you should be able to view the 'closed' pull request which either created or updated the Security policy (SECURITY.md file). 
- Additionally, you should be able to view a new 'open' pull request created by Dependabot requesting an update of a dependency.
- Finally, you should be able to view the results of the CodeQL Analysis in the Security tab.

### Learning Resources

- Learn more about adding a security policy to your repository [here](https://docs.github.com/en/github/managing-security-vulnerabilities/adding-a-security-policy-to-your-repository).
- Learn more about Dependabot and vulnerable dependencies [here](https://docs.github.com/en/github/managing-security-vulnerabilities/managing-vulnerabilities-in-your-projects-dependencies).
- Learn more about automated code scanning and understanding results [here](https://docs.github.com/en/github/finding-security-vulnerabilities-and-errors-in-your-code).

### Tips

- If you are stuck, check out the 'Security' tab of your repository on GitHub.

## Task 03

### Introduction

To bring our DevOps journey full circle we need to understand what is happening in our deployed environments. It is too late for us to find out about a problem, by the time our users are complaining about it. It is also imperative to know about not only the performance of the site, but also the impact positive or negative a feature has had on our users. Please take a moment to review the articles below to gain a better understanding of the importance of monitoring and Application Insights, one of the tools we have to make it easy in Azure.

### Description

In this challenge we will look at some of the telemetry that has already been collected by our running instance from Application Insights, injected into the Azure resources created back in our earlier Infrastructure-as-code challenge.

- Review the `container-webapp-template.json` ARM template. Find where the Application Insights node was created and note how the Web App was configured to send its logs there. 
- Create a dashboard in the Azure Portal to provide a summary of the status of our site. ([hint](https://docs.microsoft.com/en-us/azure/azure-monitor/app/overview-dashboard#application-dashboard))
- Implement an outside in availability test for the homepage of your site ([hint](https://docs.microsoft.com/en-us/azure/azure-monitor/app/monitor-web-app-availability))

### Success Criteria

- You should understand the importance of monitoring and some of the basic features offered by Application Insights.
  - **NOTE**: We are just scratching the surface of what is offered in Azure Monitoring, if you are interested in learning more there is a full What the Hack focused on [Azure Monitoring](https://github.com/microsoft/WhatTheHack/tree/master/007-AzureMonitoring).

### Learning Resources

- [What is Monitoring?](https://docs.microsoft.com/en-us/azure/devops/learn/what-is-monitoring)
- [What is Application Insights?](https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview)

### Tips