
# Git Commands

#### _CONFIGURATION_
```
# check your git configuration
git config --list

# setup your git username and email
git config --global user.name "Aayush Agrawal"
git config --global user.email "agrawal.aayushn91@gmail.com"
```

#### _SETUP & INIT_
```
# initialize the git repository
git init

# retrieve an entire repository from a hosted location via URL
git clone url
```

#### _STAGE & COMMIT_
```
# add a file to the staging area
git add filename

# add all file to the staging area
git add .

# see the changes and add to the staging area
git add -p

# add only certain  files to the staging area
git add filena*

# commit changes
git commit -m "message"

# add track files to staging area and commit changes
get commit -am "message"
```

#### _HISTORY, LOGS & DIFF_
```
# check the commit history
git log --oneline --graph

# see the commit log of the remote repository
git log alias/branchname

# see a specific commit
git show commit_id

# see changes made before committing them
git diff
git diff filename
git diff --staged
```

#### _UNDO_
```
#revert unstaged changes
git checkout filename
git checkout  .
git restore .

# revert staged changes
git restore --staged ..
git restore -S ..
git reset HEAD
git reset HEAD filename
git reset HEAD -p

# undo the last local commit
git reset --soft HEAD~1
git reset HEAD~1

# rollback the last commit
git revert HEAD

# rollback the specific commit
git revert commit-id

# amend the most recent commit
git commit --amend
```

#### _BRANCHING & MERGING_
```
# create a branch
git branch branchname

# switch to a branch
git checkout branchname

# create and switch to a branch
git checkout -b branchname

# list branches
git branch

# list remote branches
git branch -r

# delete a branch
git branch -d branchname

# merge a branch
git merge remote/branchname

# abort the merge
git merge --abort
```

#### _TEMPORARY COMMITS_
```
# save modified and staged changes
git stash
git stash save "message"

# apply changes from the stash stack
git stash pop
git stash apply 0

# see the stash stack
git stash list

# discard the changes from top of the stash stack 
git stash drop
```

#### _SHARE & UPDATE_
```
# add a remote repository
git remote add  alias URL

# see all remote repositories
git remote -v

# pull changes from remote repository & merge with local changes as seperate commit
git pull 

# apply unpublished changes from the local with the published changes from remote
git pull --rebase

# push the changes to remote repository
git push alias branchname
git push alias branch --set-upstream
git push -u alias branch
```

#### _CLEAN_
```
# dry run
git clean -n

# directories
git clean -d

# force
git clean -f