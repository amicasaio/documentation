# New repo creation steps

1. Here in Amicasa IO, we use a git workflow named: `git-flow`.

   - Link for learning git-flow: [https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)
   - Install for _MACOS_: `brew install git-flow`
   - Install for _Linux_: `apt-get install git-flow`

2. Next step is running the command: `git-flow init`

   - Her the program is going to ask you for the names of the different branches. Leave all the names by default by just clicking enter and in the last question which is **version prefix**, put a letter "v" so that version may look the following way `v0.1.0` instead of `0.1.0`

3. You will be prompted to the **develop** branch, go to the master branch with `git checkout master` and run the following command which will indicate the first phase of the proyect: `git tag -a v0.0.0 -m 'Initial commit of ${name_of_service} service'` \*change the variable in curly braces.

4. Enter to github to Amicasa IO organization repository and create the new repo. If you don't have permissions to create the repo, ask your superior to open it for you

5. Add the remote repository to your local repository.

6. Push your first tag to link your local repository to the remote repository.

7. Start programming with the git-flow workflow!
