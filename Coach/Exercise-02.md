# Exercise 02 - Start working in GitHub

## Task 1 - Set up a GitHub repository

- Some learners create their first repository via GitHub Desktop.  Make sure learners understand how a distributed source control such as Git differs from a centralized source control system such as Team Foundation Version Control (TFVC) or Subversion (SVN).
- To clone a repository via command line, run `git clone $URL` in the directory into which you wish to copy the repository. In order to get the value for URL, perform the following steps:
  - Navigate to your repository in GitHub.
  - Select the green button labeled "<> Code".
  - In the Clone section of the Local tab, select the copy button to get the URL.
- Add the training files into the root of this new directory.
- In the command line, the command `git add --all` will mark for tracking all of the files you just copied to the folder.
- In order to commit changes, type in a command such as `git commit -am "My first commit"`. Note that the commit message can be whatever you wish it to be.
- The final step is to push this code to the remote GitHub repository. To do this, execute the command `git push -u`. If this is the first time you are pushing to a particular GitHub repository, you might see a warning message.

## Task 2 - Track your work

- Make sure students are aware of the difference between the new "Project (Beta)" and traditional Project boards.
- Learners may only assign tickets to other team members if those members have accepted invitations to be collaborators on the repository.
- The following steps will allow a learner to add one or more collaborators:
  - In your repository, select "Settings" from the options menu.
  - In the Access menu on the left-hand side, choose the "Collaborators" option. Any learners with two-factor authentication enabled for their account may be prompted for additional authentication.
  - Select the "Add people" option from the Manage access section.
  - A dialog box will appear. Enter the e-mail address or username of any team members you wish to add.
  - Team members will need to accept invitations before this process is complete.
- The following steps will allow a learner to create an automated Kanban board:
  - In your repository, select "Projects" from the options menu.
  - Select the drop-down arrow in the "Link a project" button and select "New Project" from the menu. Then select the "New project" button.
  - Select "Team backlog" in the project templates.
  - Select "Create" to generate the project board.
- The following steps will allow a learner to create and work with GitHub Issues:
  - In your project board, select the "Add Items" button.
  - Create the draft issues as specified in the task.
  - Convert draft issues to issues. Make sure you select the repository for each one.
  - New issues, if linked properly, should show up in the "New" column.  Once you set the issue to a status of Closed, it should move to the "Done" column in the project board. If you are viewing the issues from the Kanban project board, you will be able to drag issues across columns.

## Task 3 - Set up a GitHub Actions workflow

- The solution for this task is a YAML file in [the solutions folder](./Solution/Exercise-02/Task-3/first-workflow.yml).
