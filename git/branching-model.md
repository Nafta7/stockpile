# GIT BRANCHING MODEL

A successful Git branching model by [@nvie](http://nvie.com/posts/a-successful-git-branching-model/).

## Main branches

The central repo holds two main branches with an infinite lifetime:

 - **origin/master**: The main branch where the source code of HEAD always reflects a production-ready state.

 - **origin/develop**: The main branch where the source code of HEAD always reflects a state with the latest delivered development changes for the next release. Also know as "integration branch".

## Supporting branches

The different types of branches we may use are:

 - [Feature branches](#feature-branches)
 - [Release branches](#release-branches)
 - [Hotfix branches](#hotfix-branches)

## Feature branches

May branch off from:  
  - **develop**

Must merge back into:  
  - **develop**

Branch naming convention:  
  - anything **except** *master*, *develop*, *release-**, or *hotfix-**

### Creating a feature  branch

When starting work on a new feature, branch off from the develop branch.

```bash
$ git checkout -b myfeature develop
# Switched to a new branch "myfeature"
```

### Incorporating a finished feature on develop

Finished features may be merged into the develop branch definitely add them to the upcoming release:

```shell
$ git checkout develop
# Switched to branch 'develop'
$ git merge --no-ff myfeature
# Updating ea1b82a..05e9557
# (Summary of changes)
$ git branch -d myfeature
# Deleted branch myfeature (was 05e9557).
$ git push origin develop
```

## Release branches

>Release branches support preparation of a new production release.
They allow for last-minute dotting of i’s and crossing t’s.
Furthermore, they allow for minor bug fixes and preparing meta-data
for a release (version number, build dates, etc.)

May branch off from:
  - **develop**

Must merge back into:
  - **develop** and **master**

Branch naming convention:
  - **release-***

### Creating a release branch

```bash
$ git checkout -b release-1.2 develop
# Switched to a new branch "release-1.2"
$ ./bump-version.sh 1.2
# Files modified successfully, version bumped to 1.2.
$ git commit -a -m "Bumped version number to 1.2"
# [release-1.2 74d9424] Bumped version number to 1.2
# 1 files changed, 1 insertions(+), 1 deletions(-)
```

### Finishing a release branch
```bash
$ git checkout master
# Switched to branch 'master'
$ git merge --no-ff release-1.2
# Merge made by recursive.
# (Summary of changes)
$ git tag -a 1.2
```

To keep the changes made in the release branch, we need to merge those
back into develop, though. In Git:

```bash
$ git checkout develop
# Switched to branch 'develop'
$ git merge --no-ff release-1.2
# Merge made by recursive.
# (Summary of changes)
```

Now we are really done and the release branch may be removed,
since we don’t need it anymore:

```bash
$ git branch -d release-1.2
# Deleted branch release-1.2 (was ff452fe).
```

## Hotfix branches

> Hotfix branches are very much like release branches in that they
are also meant to prepare for a new production release, albeit
unplanned. They arise from the necessity to act immediately upon
 an undesired state of a live production version. When a critical bug
in a production version must be resolved immediately, a hotfix branch
may be branched off from the corresponding tag on the master branch
that marks the production version.

May branch off from:
  - **master**

Must merge back into:
  - **develop** and **master**

Branch naming convention:
  - **hotfix-***

### Creating a hotfix

```bash
$ git checkout -b hotfix-1.2.1 master
# Switched to a new branch "hotfix-1.2.1"
$ ./bump-version.sh 1.2.1
# Files modified successfully, version bumped to 1.2.1.
$ git commit -a -m "Bumped version number to 1.2.1"
# [hotfix-1.2.1 41e61bb] Bumped version number to 1.2.1
# 1 files changed, 1 insertions(+), 1 deletions(-)
```

Don’t forget to bump the version number after branching off!

Then, fix the bug and commit the fix in one or more separate commits.

```bash
$ git commit -m "Fixed severe production problem"
# [hotfix-1.2.1 abbe5d6] Fixed severe production problem
# 5 files changed, 32 insertions(+), 17 deletions(-)
```

### Finishing a hotfix branch

```bash
$ git checkout master
# Switched to branch 'master'
$ git merge --no-ff hotfix-1.2.1
# Merge made by recursive.
# (Summary of changes)
$ git tag -a 1.2.1
```

Next, include the bugfix in develop, too:

```bash
$ git checkout develop
# Switched to branch 'develop'
$ git merge --no-ff hotfix-1.2.1
# Merge made by recursive.
# (Summary of changes)
```

The one exception to the rule here is that, **when a release branch currently exists, the hotfix changes need to be merged into that release branch, instead of** develop.

Finally, remove the temporary branch:

```bash
$ git branch -d hotfix-1.2.1
# Deleted branch hotfix-1.2.1 (was abbe5d6).
```

Taken from: http://nvie.com/posts/a-successful-git-branching-model/
