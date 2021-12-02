# Git Branch Model  

1. Create a new branch off from the develop branch and Switched to a new branch "myfeature"

```
$ git checkout -b myfeature develop
```

```
$ git checkout develop
Switched to branch 'develop'
$ git merge --no-ff myfeature
Updating ea1b82a..05e9557
(Summary of changes)
$ git branch -d myfeature
Deleted branch myfeature (was 05e9557).
$ git push origin develop
```

The --no-ff flag causes the merge to always create a new commit object, even if the merge could be performed with a fast-forward. This avoids losing information about the historical existence of a feature branch and groups together all commits that together added the feature. 

## Ref:

https://nvie.com/posts/a-successful-git-branching-model/

https://docs.github.com/en/get-started/quickstart/github-flow
