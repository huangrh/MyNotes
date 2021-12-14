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
### more about git checkout
git checkout @{14.days.ago} # Cloning an older version of github repo
git checkout 'master@{1979-02-26 18:30:00}' # based on a date
git checkout afe52          # checkout based on a hash
git checkout master         # go back to the most recent version 


```
## Ref:

https://nvie.com/posts/a-successful-git-branching-model/

https://docs.github.com/en/get-started/quickstart/github-flow