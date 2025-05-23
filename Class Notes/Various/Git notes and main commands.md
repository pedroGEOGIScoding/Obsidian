git branchsee this for visual info: [Git commands](https://ndpsoftware.com/git-cheatsheet.html#loc=remote_repo;)
## After installing and before starting:
Check github version with:
git --version
## Next run those commands below to set up the github account
1. **git config --global user.name "Your Name"**
2. **git config --global user.email "Your email"**
### The most important commands are by this order:
1. **git init** (this command is execute in the local folder on your computer where you want to save and keep the local version of the remote repository).

> [!warning]
> > MUY IMPORTANTE NO EJECUTAR ESTE COMANDO EN C:!!! SOLAMENTE dentro de la carpeta especifica del proyecto

2. **git add .** (with the point at the end, all files of the folder repo where **git init** has been executed, will be added to the staging area).
    a) **git add** followed only by the name of the specific file to add only that file.

3. **git commit** (command used to save changes made in the staging area of a repository. It creates a snapshot of the current state of the project at a specific moment, recording the changed files along with a brief message describing the changes. This serves as a historical record, allowing developers to track revisions and revert to previous states if necessary). There are some variants:
    a) **git commit -m "description about the commit"** (adding a description of changes made or update)
    b) **git commit -am "description about the commit"** (with the **a** this command makes the add and commit at the same time)

4. **git status** (the status of the files tracked, untracked, modified, etc)
    a) **git status -s** (this command is the same but retrieve a more concise information)

5. **git log --oneline** (to see the commits made)

6. **git remote -v** (List the remote connections you have to other repositories including the URL)
    a) **git remote add <connection name, which normally is names as origin> <url>** (Create a new connection to a remote repository. After adding a remote, you’ll be able to use ＜name＞ as a convenient shortcut for ＜url＞ in other Git commands)
    b) **git remote rm <name>** (Remove the connection to the remote repository called ＜name＞)
    c) **git remote rename <old-name> <new-name>** (Rename a remote connection)

7. **git branch** (list the local branches and pints out with * in which branch you are working on at this moment)
    a) **git branch -a** (to display all branches).
    b) **git branch -r** (to display all branches, even those from the remote repo).
    c) **git branch <name of the new branch>** (to add a new branch).
    d) **git branch -M <new-branch-name>** (Used to rename the current branch to a new specified name and forcefully update it if a branch with the new name already exists).
    e) **git checkout <name of the branch you want to move on>** (to move to another branch).
    f) **git checkout --track origin (if named as origin)/<branch name>**. This command creates a new local branch with the same name as the remote branch and sets it up to track the remote branch.
        f1) **git branch --set-upstream-to=origin/<remote-branch> <local-branch>**. Set an existing local branch to track a remote branch.
    g) **git branch -avv**. To view all branches along with their tracking information. This will show both local and remote branches, including the upstream branches they are tracking.
    h) **git branch -vv**. This command shows detailed information about your local branches, including which remote branches they're tracking.
    i) **git remote show origin** . Used to display detailed information about the remote repository named "origin." This includes the fetch and push URLs, the tracking branches, and the status of the remote branches compared to your local branches. When you clone a repository, Git automatically names the remote repository "origin," making it the default reference for remote operations.
    j) **git branch -D <name of the branch to delete>**.

8. There are two commands to upload to the github repositoty the commits made. These two have to be executed by this order:
    a) first and only has to be done once is: **git remote add <connection name, which normally is names as origin> <github url>** .
    b) second every time to upload commits remotely: **git push <connection name, which normally is names as origin> <branch name>**.

9. **git push --set-upstream <connection name, which normally is names as origin> <branch-name>** (used to push this branch to a remote repository and set the remote branch as the upstream for the local branch. This means that ***future git push and git pull commands will default to this remote branch*** unless specified otherwise) .

10. **git pull** (download all files from the remote repo).
11. **for remote in `git branch -r`; do git branch --track `echo $remote | cut -d '/' -f2` remotes/$remote; done**. For start tracking all remote branches.
12. **for remote in `git branch -r`; do git checkout `echo $remote | cut -d '/' -f2`; git pull; done**. Checkout every branch and pull.
13. **git clone <github url>** (copy in your local drive all files of remote repo).

14. **git reset --hard <commit code>** (to travel back or forward to the specific commit indicated in the commit code).

15. **git merge** (Used to combine two branches. **git merge** takes two commit pointers, usually the branch tips, and will find a common base commit between them. Once Git finds a common base commit it will create a new "merge commit" that combines the changes of each queued merge commit sequence. Before performing a merge there are a couple of preparation steps to take to ensure the merge goes smoothly).
    a) Execute **git status** to ensure that HEAD is pointing to the correct merge-receiving branch.
    b) Execute **git checkout <name of the receiving branch>** to switch to the receiving branch.
    c) Execute **git fetch** to pull the latest remote commits to make sure the receiving branch and the merging branch are up-to-date with the latest remote changes.
    d) Execute **git pull** to make sure that the fetch is completed and the main branch has the latest updates.
    e) **git merge --abort** (to cancel merging).

16. **git tag <name given by the user to define this version> -m "description"** (making final versions).
    a) **git push --tags** to upload the tags.

17. **GitHub CLI** is an open source tool for using GitHub from your computer's command line.
    a) Further info: [GitHub CLI](https://docs.github.com/en/github-cli/github-cli/about-github-cli).

18. **git ls-files** (Show information about files in the index and the working tree). See further info to exclude certain files from git tracking.

19. The git clone command clones all of the repository’s branches by downloading their data, but it only automatically creates a local branch for the main branch. The other branches are available in the repository; you can switch to them using git checkout <branch-name>, which will create a local branch that tracks the remote branch the first time you check it out. To ensure that you clone all branches and have them available locally and you need to check them out as local branches. You can manually check out each branch one at a time using: git checkout -b <branch-name> origin/<branch-name> or to automate the process and checkout all remote branches, you can use a loop in the command line:

`for branch in `git branch -r | grep -v '\->'`; do git branch --track "${branch#origin/}" "$branch" done`

> [!This script does the following]
> Lists all remote branches.
> Filters out any pointers to other branches (like origin/HEAD -> origin/main).
> Loops through each branch and creates a local branch that tracks the remote branch.After setting up tracking for each branch, make sure your local copies are up to date:`git fetch --all`




