# 1. setup    
- create a new repo on github and follow the direction to next step    
    - in private mode    
    - add collobrates      
    - add rule to require a review before merge      
- Create a directory for a project         
- initial a git repository at the local computer     
> git init    

- git remote add    
# 
> git remote add  origin git@github.com:huangrh/mynotes.git  
> git remote -v # check the remote address  



3. git remote add origin git@github.com-serena......  


# 2. stage the change and Commit      
> git add .  # stage all change     
> git commit -am "commit message"     

# 3. git push origin master    

# 4. collobarator       
> git chechout -b new_branch  # change the name 'new-branch' to a meaningful name    
> git pull origin master   
> git add .   
> git commit -am "meaningful commit message"   
> git push origin new_branch   
> make a pull request    
> 


# Github ssh key setup



# 5. Git Branch Model  

1. Create a new branch off from the develop branch and Switched to a new branch "myfeature"

```
$ git checkout -b myfeature develop
```

```
$ git checkout develop # Switch back to branch 'develop'

$ git merge --no-ff myfeature # merge the change in myfeature branch back to 'develop' branch.   
Updating ea1b82a..05e9557  
(Summary of changes)  
$ git branch -d myfeature # Delet the branch named 'myfeature' (was 05e9557).  
$ git push origin develop  
```  
 
The --no-ff flag causes the merge to always create a new commit object, even if the merge could be performed with a fast-forward. This avoids losing information about the historical existence of a feature branch and groups together all commits that together added the feature.   

# 6. Git diff 
```
# https://www.datacamp.com/tutorial/git-diff-guide
Git Diff Command Reference
Git diff offers a wide range of options to customize its output and behavior for specific situations. Here’s a comprehensive reference of the most commonly used parameters to enhance your differential analysis:

Basic comparison options
git diff - Compare working directory to staging area
git diff --staged (or --cached) - Compare staging area to last commit
git diff HEAD - Compare working directory to last commit
git diff <commit> - Compare working directory to specific commit
git diff <commit1> <commit2> - Compare two specific commits
git diff <branch1> <branch2> - Compare two branches
Path limiting
git diff -- <path> - Limit comparison to specific file or directory
git diff --stat - Show summary of changes (files changed, insertions, deletions), a very useful option for large diffs
git diff --name-only - Show only names of changed files
git diff --name-status - Show names and status (added, modified, deleted) of changed files
Display control
git diff -w (or --ignore-all-space) - Ignore whitespace changes
git diff --ignore-space-change - Ignore changes in amount of whitespace
git diff --color-words - Show word-level differences with color
git diff --word-diff - Show word-level differences in a different format
git diff -U<n> - Show n lines of context (default is 3)
git diff --no-prefix - Don't show a/ and b/ prefixes in diff output
Content filtering
git diff --binary - Show changes to binary files
git diff -S<string> - Look for changes that add or remove the specified string
git diff -G<regex> - Look for changes that match the specified regex pattern
git diff --pickaxe-all - When using -S or -G, show all changes in the file, not just matching ones
Format options
git diff --patch-with-stat - Show patch and stats summary
git diff --compact-summary - Show stats summary in a compact format
git diff --numstat - Show stats in a machine-friendly format
git diff --summary - Show creation/deletion summary
```

# -----------------------------------------------------------------------------------------------
# other   
```  
# http://stackoverflow.com/questions/5772192/how-can-i-reconcile-detached-head-with-master-origin   
$ git checkout - # go back to previous branch or # this saves me.  
```



```  
### more about git checkout  
git checkout @{14.days.ago} # Cloning an older version of github repo  
git checkout 'master@{1979-02-26 18:30:00}' # based on a date  
git checkout afe52          # checkout based on a hash  
git checkout master         # go back to the most recent version   
```

- [Git fetch remote branch](https://stackoverflow.com/questions/9537392/git-fetch-remote-branch)
- 
```
# You need to create a local branch that tracks a remote branch. 
# The following command will create a local branch named daves_branch, tracking the remote branch origin/daves_branch.
# When you push your changes the remote branch will be updated.
git checkout --track origin/daves_branch

# OR fetch remote branch. 
git fetch <remote> <rbranch>:<lbranch>  # r = remote, l = local, lbranch=local branch  
git checkout <lbranch>

# OR 
git fetch --all  # fetch all branck
git checkout myBranch 
```

```
# untrack a specific file
# To stop tracking a file in Git, you can use the command git rm --cached <filename>.
# This removes the file from the index, but it remains in your working directory.
# You can then add the filename to the .gitignore file to prevent the file from being reintroduced in later commits. 
> git rm --cached mylogfile.log    
and for a single directory:  
# untrack all file under a directory  
> git rm --cached -r mydirectory

```

# 6. Git Stash  
```    
> git stash
- https://www.freecodecamp.org/news/git-stash-commands/
# when you are ready to restore a saved stash, git stash pop will apply the newest stash and clear it from your stash clipboard. 
# stash is not bound to the branc where you created it. when you restore it, the chagnes witll e applied to your current HEAD branch, whichever this may be. 
> git stash pop # apply the recorded changes of your latest stash on the current working branch as well as remvoe that stash from the stash stach.
# or
> git stash apply
> git stash clear
# delete a particlular stash
> git stash drop stash@{2} 
# 
> git stash show
> git stash list
> git stash branch <new-branch-name> stash@{2}  # create a branch basing on stash revision  
```

# 7. Tracked vs untracked  

[How can I make git show a list of the files that are being tracked?](https://stackoverflow.com/questions/15606955/how-can-i-make-git-show-a-list-of-the-files-that-are-being-tracked/15606998)
```
# 
# 
git ls-files
git ls-tree master
git ls-tree -r master --name-only
```

[how-do-i-fix-a-git-detached-head](https://stackoverflow.com/questions/10228760/how-do-i-fix-a-git-detached-head)
```
> git log --oneline # find the commit id
> git checkout d45c57a # git reflog to see the index. 
> git log --all 
> git checkout master # go back to the master branch  
> git checkout your-branch # go back to your branch  
> git checkout HEAD@{1}  # HEAD@{0, 1,2,3,4,5, etc}
If you want to go back to any commit you previously checked out (either 1, 2, or 3 etc steps) then you can take a look at git reflog
> git reflog # 
# Git only requires a network connection on fetch/push/pull operations.
# git a dog
git log --all --decorate --oneline --graph
```

```
Commit changes you want to keep. If you want to take over any of the changes you made in detached HEAD state, commit them. Like:

> git commit -a -m "your commit message"
Discard changes you do not want to keep. The hard reset will discard any uncommitted changes that you made in detached HEAD state:

> git reset --hard
(Without this, step 3 would fail, complaining about modified uncommitted files in the detached HEAD.)

Check out your branch. Exit detached HEAD state by checking out the branch you worked on before, for example:

> git checkout master
Take over your commits. You can now take over the commits you made in detached HEAD state by cherry-picking, as shown in my answer to another question.

> git reflog
> git cherry-pick <hash1> <hash2> <hash3> …
# Since "detached head state" has you on a temp branch, just use 
> git checkout - # which puts you on the last branch you were on.
```

```
# https://stackoverflow.com/questions/8358035/whats-the-difference-between-git-revert-checkout-and-reset
git checkout 
git revert commit-hash
git reset --hard commit-hash
git reset --soft commit-hash
```

```
# How to amend last commited message
git commit --amend -m "New and correct message"
git commit --amend -m “this fixes the previous oopsies”
git commit --amend --no-edit # it will not change the message associaed with the commit because we have not used the -m flag.  
```




## Pull changes from a remote branch  
- create a new branch
> git branch branch-name

- change environment to the new branch
> git checkout branch-name
- 
> git fetch origin # fetch all the remote branch  
> git fetch origin adf_publish  # fetch one branch
> git branch -a # list all the branches available for checkout  
> git checkout -b fix-failing-tests origin/fix-failing-tests   

What this does is:      

- it creates a new branch called fix-failing-tests  
- it checkouts that branch  
- it pulls changes from origin/fix-failing-tests to that branch  

## Push

```
git push <remote repo> <local branch name>:<remote branch name>
```

# 8 git config  

## Git add more multiple accounts
- https://code.tutsplus.com/quick-tip-how-to-work-with-github-and-multiple-accounts--net-22574t  
1. add ssh keys  
2. edit ~/.ssh/config file

```
C:\Users\huang\.ssh> cat config  
# Default GitHub  
Host github.com  
  HostName github.com  
  User git  
  IdentityFile ~/.ssh/ed25519  
 
# serena  
Host github.com-serena  
  HostName github.com  
  User git  
  IdentityFile ~/.ssh/serena_ed25519
```

```
# How to config two github ssh files under one computer account. 
# Account 1
Host github.com
HostName github.com
User git
IdentifyFile ~/.ssh/id_rsa

# Account 2
Host github.com-emily
HostName github.com
User git
IdentifyFile ~/.ssh/other_id_rsa
```

##  using includeIf to manage your git identidites    

- https://medium.com/@mrjink/using-includeif-to-manage-your-git-identities-bcc99447b04b  
- https://git-scm.com/docs/git-config#_conditional_includes
  
```
# (Part of) my ${HOME}\.gitconfig looks like this:
[includeIf "gitdir:C:/SOURCE/PERSONAL/"]
    path = .gitconfig-personal
[includeIf "gitdir:C:/SOURCE/COMPANY1/"]
    path = .gitconfig-company1
[includeIf "gitdir:C:/SOURCE/COMPANY2/"]
    path = .gitconfig-company2

# Here’s my ${HOME}\.gitconfig-personal:
[user]
    name = Gillis J. de Nijs
    email = gillis@home.tld

```


```
> git config --global user.email "your_email@example.com"
> git config --global user.name "First Name Last Name"
```
# 9. gpg commit signature  

- https://docs.github.com/en/authentication/managing-commit-signature-verification/about-commit-signature-verification

- 
## Ref:


https://docs.microsoft.com/en-us/azure/devops/repos/git/pushing?view=azure-devops&tabs=git-command-line#tabpanel_1_git-command-line

https://nvie.com/posts/a-successful-git-branching-model/

https://docs.github.com/en/get-started/quickstart/github-flow
