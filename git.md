# Git

The simple commandline for git.

## Basic use

### Git version
For checking the version of git that installed on the endpoint
```sh
git --version
```

### Git help
```sh
git help -a # Short syntax
git help --all # full syntax
```

### Git config
Config the git on local/global repository
```sh
git config --g user.name "<your_username>" # Setting global config user name
git config --g user.email "<your_email@your_company.com>" # Setting global config email
git config --list # List on git config on your endpoint
```

### Git init
```sh
git init # for init a git repository
```

### Git add
```sh
git add * # for adding all files to stash
git add *.md # for adding all .md files to stash
git add test* # for adding all test* files to stash, such as test1.txt, test-case.js,...
```

### Git commit
```sh
git commit -m "<message>"
```

### Git push
```sh
git push # for pushing current change into a remote branch
git push origin <branch_name> # I recommended to use this command to push into a target remote branch
git push origin <local_branch>:<remote_name> # For pushing into a remote branch from a difference local branch name
git push origin <branch_name> --force # or -f if shorter, force to push the change into remote branch (will override history) 
```

### Git checkout
```sh
git checkout <branch_name> # for switch branch name (and switch code branch, or set to remote code if matching branch name)
git checkout -b <branch_name> # checkout a new branch from current branch
```

### Git pull
```sh
git pull # pulling code from current branch
git pull origin <branch_name> # pulling code from remote branch
```

### Git fetch

For fetching state from current branch (and will need to `git pull` command later to pull the new code from origin)
```sh
git fetch
```

### Git log

For logging on git commit from local repository
```sh
git log
```

## Advanced use

### Git rebase

Rebase a branch with master (push branch commit on-top of the master)
```sh
git fetch # on master branch
git pull origin master # for pulling latest code on master
git checkout <branch_name> # checkout to branch code
git rebase master # for rebase branch code into master, resolve conflict if present
git push origin <branch_name> --force # for pushing code into branch (rewrite history), or -f for shorter
```

__Limitation__: Cannot rebase if we have a `MERGED` commit between commits

### Git cherry-pick

Pick commit(s) from another location (with hash `commit_id` given) into current branch. The commit id may come from any branch
For example, I have 3 commits from branch `feat-login`

`a12e3f` `Fix login`

`b34d2d` `Update react18`

`a123bc` `Fix typo`

And I want to pick 2 commits `Fix typo` and `Update react18` to release, and keep the fix login commit to fix and release later, I will do following commands
```sh
git checkout master # for checkout onto master
git fetch
git pull origin master # In my case master is release branch
git checkout -b <release_branch>
git cherry-pick a123bc b34d2d
git log # for logging git history, after confirm the change I will push it into release branch
git push origin <release_branch>
```
__Powerful use:__

Pick some commits, and override into current branch (in case we have a `MERGE` commit and cannot use `git rebase`)

Example we have following commits from `feat-login` branch

`a12e3f` `Fix login`

`fa33d5` `MERGED master into feat-login`

`b34d2d` `Update react18`

`a123bc` `Fix typo`

In above git log, we unable to using `git rebase master` due to `fa33d5` is a `MERGED` commit. So the best way to resolve it is from the `feat-login` branch
```sh
git checkout master # for checkout onto master
git fetch
git pull origin master # In my case master is release branch
git checkout -b <my_branch>
git cherry-pick a123bc b34d2d a12e3f
git log # for logging git history, after confirm the change I will push it into release branch
git push origin <my_branch>:feat-login --force # push code from <my_branch> into remote branch feat-login
```

The result is we have 3 commits onto master (same as rebase)

`a12e3f` `Fix login`

`b34d2d` `Update react18`

`a123bc` `Fix typo`

__Addition:__
```sh
git cherry-pick commit_id1...commit_id5 # will pick all commits EXCEPT `commit_id1` (commit_id2 commit_id3 commit_id4 commit_id5)
```

```sh
git cherry-pick commit_id1^..commit_id5 # will pick all commits (commit_id1 commit_id2 commit_id3 commit_id4 commit_id5)
```

### Git squash, git diff, git submodule, init a secret key, git clone
