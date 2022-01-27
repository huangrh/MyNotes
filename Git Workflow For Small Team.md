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


# other   
```  
# http://stackoverflow.com/questions/5772192/how-can-i-reconcile-detached-head-with-master-origin   
$ git checkout - # go back to previous branch or # this saves me.  
```  

```

> git stash
# when you are ready to restore a saved stash, git stash pop will apply the newest stash and clear it from your stash clipboard. 
# stash is not bound to the branc where you created it. when you restore it, the chagnes witll e applied to your current HEAD branch, whichever this may be. 
> git stash pop
```

```  
### more about git checkout  
git checkout @{14.days.ago} # Cloning an older version of github repo  
git checkout 'master@{1979-02-26 18:30:00}' # based on a date  
git checkout afe52          # checkout based on a hash  
git checkout master         # go back to the most recent version   
```

```
# untrack a specific file  
> git rm --cached mylogfile.log    
and for a single directory:  
# untrack all file under a directory  
> git rm --cached -r mydirectory  
```

```
# How to config two github ssh files under one computer account. 
# Account 1
Host github.com
HostName github.com
IdentifyFile ~/.ssh/id_rsa

# Account 2
Host github.com
HostName github.com-other
IdentifyFile ~/.ssh/other_id_rsa
```

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
```

## Ref:

https://nvie.com/posts/a-successful-git-branching-model/

https://docs.github.com/en/get-started/quickstart/github-flow
