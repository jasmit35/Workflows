# Workflows

## Bug Fixes
Use git to pull the existing code from the master branch on GitHub:

```
cd ~/devl/<app>
git fetch
git checkout master
git pull github master
git branch bugfix/vX.Y.Z
git push -u github bugfix/vX.Y.Z
git checkout bugfix/vX.Y.Z
```
Fix and test all the bugs listed in the document .../shared/Project Documentation/<app>/Bugs and New Features

Commit all changed code:

```
git status
git add .
git commit -m "Changes for bugfix/v1.0.1"
git push github
```

Follow the project's documentation for testing a new release, but use the bugfix branch rather than the release branch. 

Once all testing is successful, merge the changes into the master branch.

```
git status
git fetch
git checkout master
git pull github master
git merge bugfix/vX.X.X
git commit -m 'Commit of bugfix/vX.X.X into master'
git push github master
```
Once all desired bug fixes and new features have been completed, create a new release branch (see below).

## Appendix C - Adding a new feature

Check the git status of the development directory. It should be on the main branch. Their may be chages to the documentation that need commited. If so commit them and push them so the working directory is totally in sync.

Use git to create a new feature branch.


```
git pull --rebase synology master
git branch feature/2026.X.X
git push -u synology feature/2026.X.X
git checkout feature/2026.X.X
```

Code and test the new feature.


Make sure all changes are in:

```
git add .
git commit -m 'Final commit before migration to test'
```

Merge the new feature into the master branch:

```
git checkout master
git pull synology master
git checkout feature/X.X.X
git rebase master
```

Thier ahould not be any conflicts, but if there are, edit the file to resolve the confict then add it back and continue the rebase. 

```
git add <file>
git rebase --continue
```

### Mergeing a bug or feature branch into master branch


## You are here








git merge feature/vX.Y+1.0
git commit -m 'Commit of feature vX.Y+1.0 into master.'
git push github master
```


## None of the following have been review since being copied in



## Enhancing an existing project
Make sure the code in the devl directories  is up to date with the master branch in the repository.

`git status`

`git pull github master`

Create the new feature branch.

`git branch v0.2.0`

Check commits, tags and where HEAD is pointing.

`git log --oneline --decorate`

Switch to the new branch.

`git checkout v0.2.0`


### Finalizing the version in development

Check the System Guide for the project to see if their are any non-standard step that won't be covered by Fire Starter. Perform only any that are necessary at this time. The rest will be reviewed again and performed latter.

#### If you have been working on a Git branch, merge it with the master branch.

`git branch`

`git checkout master`

`git pull github master`

`git pull woz master`

`git merge`*branch*

`git push github master`

`git push woz master`

#### Update the defects and enhacemnts document to reflect what has been completed in this version.

#### Assign the new version number as a tag in Git.

Check the defects and enhancements document to make sure of the new application version number.

`git tag `*new application version*

`git push --tags github`

`git push --tags woz`

####  Save any new docker images

Male sure the version number is set correctly in the Makefile.

Rebuild the image to make sure the version is tages correctly.

`make dbc-build`

Then push the image to Docker Hub for distribution.

`make push-image`
  
## Test and Prod installation

Signon to the appropriate target server using my normal ID.

Use auto_update to install the code.

`auto_update -a `*project name*` -e ` *environment*

If there are non-standard migration steps, handle them.

## New Features

## New Releases

Create a new local branch and switch to it:

```
git fetch
git checkout master
git branch release/vX.X.X
git checkout release/vX.X.X
```

Update the __init__.py file with the release number:

```
vi ~/devl/<app>/local/python/__init__.py
git add .
git commit -m 'Updated init.py with the correct release number.'
```

Create a new branch on GitHub based on the new local branch:

```
git push -u github release/vX.X.X
```

Switch back to the local master branch so no changes are made to the release:

```
git checkout master
```

## Upgrading to a new release (temporary step)

Clone the code (to test or prod):

```
cd /tmp
git clone https://github.com/jasmit35/<app>.git --branch release/vX.X.X
```

