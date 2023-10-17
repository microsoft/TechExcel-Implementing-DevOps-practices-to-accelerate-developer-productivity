# Exercise 02 - Start working in GitHub (45 minutes)

## Task 1 - Set up a GitHub repository (10 minutes)

### Introduction

Version control is typically one of the first components that teams implement as they start on projects. Version control is also one of the oldest and most well understood components of DevOps. Version control systems allow developers to collaborate and simultaneously contribute to the same codebase. They can also help teams track versions--so code can be rolled back if bad changes are made--as well as bugs, work items, and test results. If necessary, please take a moment to review the [Git handbook](https://guides.github.com/introduction/git-handbook/) to understand the basics of version control, focusing on the distributed version control technology Git.

### Description

In this task, you will ensure that you have a GitHub repository set up with the Munson's Pickles and Preserves Team Messaging System application code in it. The MP&P development team has provided us a simple version of an internal messaging system they use. This .NET 6 application is the one they would like to use for a DevOps proof of concept. They have also provided a Bicep template for creating Azure resources.

The key tasks are as follows:

1. Create a new GitHub repository in the context of your organization or account.
2. Obtain the sample application and Bicep template from the starter GitHub repository.  (TODO:  need starter repo)
3. Commit the files to your GitHub repository using your preferred Git client.

### Success Criteria

- You have a repository cloned to your local machine and synchronized with GitHub.com.
- The `Application` and `InfrastructureAsCode` folders are at the root of your repository.

### Learning Resources

- Cloning a repository via the [command line](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/cloning-a-repository) or [GitHub Desktop](https://docs.github.com/en/desktop/contributing-and-collaborating-using-github-desktop/cloning-a-repository-from-github-to-github-desktop)
- For those using GitHub Desktop, here is documentation on [committing](https://docs.github.com/en/desktop/contributing-and-collaborating-using-github-desktop/committing-and-reviewing-changes-to-your-project) and [pushing](https://docs.github.com/en/desktop/contributing-and-collaborating-using-github-desktop/pushing-changes-to-github) changes to a repository.
- If working with the command line, check out these articles on [committing](https://docs.github.com/en/github/committing-changes-to-your-project/creating-and-editing-commits) and [pushing](https://docs.github.com/en/github/using-git/pushing-commits-to-a-remote-repository) changes.
- Additionally, you may need to pull other people's changes into your local repository to stay in sync--see documentation for [command line](https://docs.github.com/en/github/using-git/getting-changes-from-a-remote-repository) and [GitHub Desktop](https://docs.github.com/en/desktop/contributing-and-collaborating-using-github-desktop/keeping-your-local-repository-in-sync-with-github).

### Tips

- For a concise explanation of adding files to a repository via the command line, see [here](https://docs.github.com/en/github/managing-files-in-a-repository/adding-a-file-to-a-repository-using-the-command-line).
- To see how it's done in the GitHub portal, check [here](https://docs.github.com/en/github/managing-files-in-a-repository/managing-files-on-github).

## Task 2 - Track your work (15 minutes)

### Introduction

Agile project management is a key component of building a modern DevOps culture at your organization. If you are new to Agile or just want a refresher, we recommend you review the following articles:

1. [What is Agile?](https://docs.microsoft.com/en-us/azure/devops/learn/agile/what-is-agile)
2. [What is Scrum?](https://docs.microsoft.com/en-us/azure/devops/learn/agile/what-is-scrum)
3. [What is Kanban?](https://docs.microsoft.com/en-us/azure/devops/learn/agile/what-is-kanban)
4. [What is Agile Development?](https://docs.microsoft.com/en-us/azure/devops/learn/agile/what-is-agile-development)

To help you with Agile project management, we have [GitHub project boards](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects). With GitHub project boards, you can quickly and easily start tracking your backlog, as well as tasks, issues, bugs, and features associated with your project.

Review the following introduction to GitHub project boards: [GitHub project boards](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects).

### Description

In this task, you will show the Munson's Pickles and Preserves team how to perform a variety of tasks around project and work tracking.

The key tasks are as follows:

1. Add other learners from your team room as contributors to your repository.
2. Create a new project using the "Team backlog" template.
3. Ensure that this new project is linked with your repository.
4. Create five draft issues in total, named `Issue 1`, `Issue 2`, `Issue 3`, `Issue 4`, and `Issue 5`. When creating each issue, make sure to assign an owner for each issue to someone from your team room. Then, convert `Issue 3` from a draft issue to an issue and then select the repository you have created. Look at the difference between a draft and your converted `Issue 3` issue.
5. Move all of your issues to the "Ready" column and convert each one from a draft to an issue.
6. Move `Issue 1` to the "In Progress" column.
7. Ask the team member to whom you assigned `Issue 1` to close the issue. Review your project board to see if the issue automatically moved to the "Done" column.
8. Create a new view to show all of your issues as a table.
9. Add a custom field to your issue. The custom field should be called "Complexity" and will be a single select type with the following options: Very Easy, Easy, Hard, and Very Hard.

### Success Criteria

- You have added one or more collaborators to your repo.
- You have a project board using the automation Kanban template. This project board contains five issues. Of those, four are open and a team member closed the fifth.
- New issues appear under the "New" column.
- Closed issues automatically appear under the "Done" column.
- You have a table view in addition to your Backlog Board.
- You have a custom field called Complexity on your issues.

## Learning Resources

- [Creating a project board in GitHub](https://docs.github.com/en/issues/planning-and-tracking-with-projects/creating-projects/creating-a-project)
- [Enable GitHub Issues on a repo](https://docs.github.com/en/free-pro-team@latest/github/managing-your-work-on-github/disabling-issues)
- [Inviting collaborators to a personal repository](https://docs.github.com/en/free-pro-team@latest/github/setting-up-and-managing-your-github-user-account/inviting-collaborators-to-a-personal-repository)
- [Learn more about automation with project boards](https://docs.github.com/en/issues/planning-and-tracking-with-projects/automating-your-project/using-the-built-in-automations)
- [Adding issues to a project](https://docs.github.com/en/issues/planning-and-tracking-with-projects/managing-items-in-your-project/adding-items-to-your-project)
- [Customizing views](https://docs.github.com/en/issues/planning-and-tracking-with-projects/customizing-views-in-your-project/customizing-a-view)

## Task 3 - Set up a GitHub Actions workflow (20 minutes)

### Introduction

Automation is a critical part of an effective DevOps process, incorporating elements like CI/CD but also processes like IssueOps. GitHub Actions provides a platform for GitHub to respond to various event triggers to execute workflows.

Workflows are broken down into jobs that contain the automation steps to complete these jobs. These steps could be a simple CLI command or a pre-defined piece of automation in the form of a GitHub Action from GitHub's extensive Marketplace.

Jobs can run on Linux, Mac OS, or Windows. They can run in the cloud on a hosted runner or run on your own machines through a self-hosted runner. We will use hosted runners for this training but your requirements after today may include the need for self-hosting or a mix of both.

Jobs can run sequentially or in parallel. Sequential jobs provide the ability to define your workflow sequences. An example of this is a sequence that includes building an application, running tests against that application, and then deploying the application artifacts to a remote server for hosting. By contrast, parallel jobs allow the ability to scale out sections of your workflow. For example, you could build your application on x64 at the same time as ARM or test the application on Chrome at the same time as Firefox.

There are common workflow patterns for CI/CD processes, but the flexibility of GitHub Actions allows us to respond to an extensive range of GitHub events. This enables many other possibilities, such as the ability to execute a workflow whenever anybody creates an issue in your project.

### Description

The IT team at Munson's Pickles and Preserves would like to see how GitHub Actions workflows operate and what is necessary to create one. The workflow you create will start with a single job triggered by manual event, but then you will expand upon it to include a second job and an additional event trigger.

1. Create a GitHub workflow in a `.github/workflows` directory and name it `first-workflow.yml`. Include a trigger to execute this workflow manually (that is, *not* triggered by an automated action such as a push into a branch or pull request).
2. Add a job called `job1` to the workflow. The job should include two steps. Each step will be a simple CLI command to echo out the phrases "Step 1 complete!" and "Step 2 complete!" respectively.
3. Manually trigger the workflow you created. Check the log file for the workflow run and ensure that your job succeeded, emitting the echo statements as expected.
4. Add a second job to this workflow and call it `job2`. In this job, add a single step. This step should call a GitHub Action from the GitHub Marketplace. Find the "Cowsays" action in the marketplace and configure this action to emit the text "Ready for prod--ship it!" in magenta font color. Note that, when you run this job, the log output may print in some other color instead of magenta.
5. Execute your workflow again, ensuring that both jobs run as expected and that the two jobs run in parallel.
6. Modify `first-workflow.yml` to ensure that `job2` does not run until `job1` completes.
7. Execute the workflow again to test the sequence.
8. Add an additional trigger to `first-workflow.yml` which fires when somebody creates an issue.
9. Create a new issue and ensure that the workflow ran as expected.

### Success Criteria

- You can execute your first workflow from a manual trigger or upon somebody creating an issue.
- You can see output from all the steps in the two jobs.
- The two jobs now run sequentially, with `job2` running after `job1` completes.
- `job2` successfully runs the "Cowsays" GitHub Action from the marketplace.

### Learning Resources

- [Understanding GitHub Actions](https://docs.github.com/en/enterprise-cloud@latest/actions/learn-github-actions/understanding-github-actions)
- [Essential features of GitHub Actions](https://docs.github.com/en/enterprise-cloud@latest/actions/learn-github-actions/essential-features-of-github-actions)
- [GitHub Actions Marketplace](https://github.com/marketplace?type=actions)
- [Manually trigger a workflow](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#workflow_dispatch)
- [Events that trigger workflows](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)
- [GitHub Action Variables](https://docs.github.com/en/enterprise-cloud@latest/actions/learn-github-actions/variables)
- [GitHub Actions Expressions](https://docs.github.com/en/enterprise-cloud@latest/actions/learn-github-actions/expressions)

## Advanced Challenges (optional)

In the event that you have completed Exercise 02 before the alloted time, please try the following challenges.

1. In your workflow file, define an environment variable for your workflow or job. Pass the contents of the environment variable into one of your echo steps in `job1`.
2. In your workflow file, pass the value from an environment variable into your "Cowsays" action in `job2`.
