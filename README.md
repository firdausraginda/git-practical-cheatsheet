# git-practical-cheatsheet

- to initialize git in a folder: `git init `
- **merge request** vs **pull request** are exactly the same. [link](https://stackoverflow.com/questions/22199432/pull-request-vs-merge-request)
- **fork** is duplicate someone's repository (including its histories) to our account, and we can make changes without interrupt the original repo
    - fork will give credit to the original repo
    - after make changes in our repo, we can create merge request or pull request to the original one (source repo)

- [3 **states**](https://serengetitech.com/tech/three-states-of-git-and-three-sections-of-a-git-project/) in git:
    - **modified**: have changed the file but not committed yet
    - **staged**: have marked the modified file in its current version, and ready to go to next commit snapshot
    - **committed**: data is safely stored in .git directory

- [3 **areas**](https://serengetitech.com/tech/three-states-of-git-and-three-sections-of-a-git-project/) in git:
    - **working dir**: is a current working directory
    - **staging area**: stores information about what will go into the next commit
    - **git dir**: is where git stores data after you do commit

- `git config` to check the active **user.name** & **user.email**:
    - to list down all git configs: `git config --list`
    - to set the user email globally: `git config --global user.email "<email>"`
    - to set the username globally: `git config --global user.name "<user name>"`
    - to unset the global user email: `git config --global --unset-all user.email`
    - to unset the global username: `git config --global --unset-all user.name`

- recording changes to the repository:
    - to check current branch, commits, & changes: `git status`
    - add modified files to staging area: `git add <file_1> <file_2>`
        - add all modified files at once: `git add .`
    - unstage file from staging area: `git restore --staged <file_1>`
    - commit with multiple line comments: `git commit`
        - this will enter vim environment
            - to start edit the file in vim: simply press `a`
            - to save changes on vim: press `escp` and then `:wq`
            - to discard changes & exit vim: press `escp` and then `q!`
    - commit with 1 line comment: `git commit -m <comments>`
        - to see what are the **insertions** & **deletion** in commit, just check the github UI
    - if changes are **modified**, not **untracked**, then can just do commit without add, using flag `-a`: `git commit -a -m <comments>`
    - push to master: `git push origin main/master`

- if in the middle of coding you want to undo everything and get back to the last commit, can do: `git restore .`:
    - this can be done as long as you haven't done `git add .` yet

- branching:
    - to list local branches: `git branch`
    - to list all branches (local & remote): `git branch --all`
    - to list merged branches: `git branch --merged`
    - to list un-merged branches: `git branch --no-merged`
    - to delete local branch: `git branch -d <branch_name>`
    - to force delete certain branch: `git branch -D <branch_name>`
    - to delete remote branch: `git push <remote_name> --delete <branch_name>`
    - to switch branch: `git checkout <branch_name>`
        - another way to do this: `git switch <branch_name>`
    - to switch to main/master branch: `git checkout main/master`
        - another way to do this: `git switch -`
    - to create new branch and switch to it: `git checkout -b <new_branch_name>`
      - another way to do this: `git switch -c <new-branch-name>`
    - to create new branch but stay in current branch: `git branch <new_branch_name>`
    - to rename branch: `git branch --move <current_branch_name> <new_branch_name>`
        - but don't rename the primary branch (main/master), because it will not overwritten
    - push changes to the new branch: `git push -u origin <new_branch_name>`

- if you have difficulty to get sense of the current branch condition, can visualize it using this command: `git log --all --decorate --oneline --graph`
    - to save this command with alias name: `alias <alias_name>="<command>"`
        - using the `<alias_name>` we don't need to type all the complete command next time, can just call the `<alias_name>`

- to check **git commit history**:
    - list all commits: `git log`
        - this command will return list of **commit hash**, commit author, commit time, & commit comment
        - this **commit hash** is like **id** to a commit
    - list the last 3 commits: `git log -3`
    - list all commits that affect only specific file: `git log -- <file_name>`
    - to jump to the previous commit: `git checkout <commit_hash>`
        - jump to previous commit to maintain the changes that exists in specific file: `git checkout <commit-hash> -- <file_name>`
        - to discard all changes from the prev commit, can just get back to main branch: `git checkout main/master`
        - to save changes after jump to previous commit, can just create & switch to new branch: `git checkout -b <new_branch_name>`

- if you created git repo on local, and want to push to remote:
    - `git remote add <remote_name> <repo_link>`
        - `<remote_name>` by default is `origin`
    - add & commit
    - `git push -u <remote_name> <branch_name>`
        - `<remote_name>` by default is `origin`
        - `<branch_name>` by default is `master` or `main`
        - flag `-u` is for **upstream**, means we push from local to remote branch. We only need to do push with flag `-u` once, after that can do `git push` only

- git **remote**:
    - **remote** is others repo that mirrored our local repo
    - when we clone the remote repo to our local, it has an alias name, which can be seen using: `git remote`
        - by default, remote in our local called **origin**
    - when run command `git status`, we'll see this msg: ***Your branch is up to date with 'origin/main'***, it means our local branch is exactly the same with the **main** branch in **origin** remote

- type of [**merges**](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging):
  - **a fast-forward** merge: when branches have direct path
  - **three-way** merge: when branches doesn't have direct path

- to merge local branch to remote master/main branch:
    - checkout to master: `git checkout main/master`
    - merge: `git merge <branch_name>`
- conflict:
    - often happen when **merge non-main/master branch to remote main/master branch**. This caused by **remote main/master branch** already **merged** code from **other branches**, the same line of code that occurs in our changes
    - to resolve this conflict: 
        - can do resolve in vim or vscode, choose which changes want to keep: the **incoming change** or the **current change**
        - add & commit changes

- changes are not detected in local branch, to resolve this problem:
    - require `fetch` to detect it : `git fetch`, 
    - pull changes from remote main/master branch: `git pull` or `git pull origin main/master`, 
    - resolve in vim or vscode, choose which changes want to keep: the **incoming change** or the **current change**, 
    - add & commit changes, 
    - push to remote main/master branch after resolve conflict

- rebase branches:
    - rebase is **another way to merge branches**, this is effective for three-way merge, or when branches doesn't have direct path
        - checkout to non-main/master branch that we want to merge: `git checkout <non-main/master_branch_name>`
        - do rebase: `git rebase main/master`
        - 1st rebase will caused the non-main/master branch is 1 step ahead of main/master, need to run 1 more rebase to make it exactly the same
            - `git checkout <main/master>`
            - `git rebase <non-main/master_branch_name>`
        - after done the 2nd rebase, push it to master: `git push -u origin main/master`

# references
- https://rogerdudler.github.io/git-guide/
- https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging
- https://stackoverflow.com/questions/4037928/can-you-issue-pull-requests-from-the-command-line-on-github
- https://towardsdatascience.com/an-easy-beginners-guide-to-git-2d5a99682a4c
