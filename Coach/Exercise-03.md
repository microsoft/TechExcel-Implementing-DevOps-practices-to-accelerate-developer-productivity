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
- To review a pull request, a team member should open the pull request, either directly through the link in the e-mail GitHub sends or by navigating to the appropriate repository and choosing the pull request from the "Pull requests" menu. The learner should navigate to the "Files changed" tab and review any changes, ensuring that line 12 `Message.cs` now supports 250 characters instead of 200. There may also be one or more unit test updates to review. If the review is successful, the reviewer may choose the green "Review changes" button on the right-hand side, choose the "Approve" radio button, and then select "Submit review" to complete the task. **Note:** a separate team member must review the pull request, not the learner who owns the repository.
- Once the team member has completed the pull request, the learner may open the pull request and select the green button to merge changes and complete the pull request. The learner may also select the checkbox to delete the feature branch. Alternatively, the learner may return to the branches list from the "<> Code" menu option and delete the branch manually.

## Task 3 - Introduce infrastructure as code

- The following az cli commands will create an application registration and then service principal. This uses a sample app registration name of `TechExcelUser`.
  - `az ad app create --display-name TechExcelUser`. Copy the "appId" output to use in the next command.
  - `az ad sp create --id {appId}`
  - `az ad sp create-for-rbac --name "TechExcelUser" --role contributor --scopes /subscriptions/{subscription_id}/resourceGroups/{resource_group_name} --json-auth`. The output of this command should be the `AZURE_CREDENTIALS` secret.
- The solution for this task is a YAML file in [the solutions folder](./Solution/Exercise-03/Task-3/deploy.yml).
- The solution for the advanced variant of this task is a separate YAML file in [the solutions folder](./Solution/Exercise-03/Task-3/deploy-advanced.yml).
- To enable Application Insights via the Azure portal, navigate to an App Service. Then, choose Application Insights from the Settings menu on the left-hand side. Next, select the "Turn on Application Insights" button to enable Application Insights. Finally, select "Apply" and then "Yes" to complete the process. Repeat this for the two remaining environments.

## Task 4 - Build, Push, and Deploy Changes

- The solution for this task is a YAML file in the solutions folder. To make the solution easier to understand, there are several versions of the solution file. Each version builds upon the prior, so the final version covers all parts of the solution.
  - The [first version of the solution file](./Solution/Exercise-03/Task-4/dotnet-deploy-1.yml) covers steps 1-5.
  - The [second version of the solution file](./Solution/Exercise-03/Task-4/dotnet-deploy-2.yml) adds in steps 6-12.
  - The [third version of the solution file](./Solution/Exercise-03/Task-4/dotnet-deploy-3.yml) adds in steps 13-15.
  - The [fourth version of the solution file](./Solution/Exercise-03/Task-4/dotnet-deploy-4.yml) adds in the advanced challenge.
- **Note:** the deployment YAML files will not run on their own because they include sections which require developer input, such as `{your_prefix}` and `{your_registry_name}`. Filling these in with appropriate values is necessary to get the .yml files in the appropriate condition to run.
- **Note:** the `images` command in each `webapps-deploy` call has a hard-coded value for the image location of `techboost/dotnetcoreapp` and expect you to hard-code the ACR location. This is because the `images` command will not accept repository secrets or variables defined in the workflow like the steps in the `dockerBuildPush` job do.
