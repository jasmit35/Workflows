# OCP - Bugs and Enhancements

## Bug Fixes

### Preliminary

Make sure the code in the devl directories is up to date with the master branch in the repository.

`git status`

If necessary commit any changes until the working directory and synology are both in sync.

If up to date but on a branch other than master, follow the instructions in the "rebase branch to master" section below.

Check the System Guide for the project to see if their are any non-standard step that won't be covered in this document. Perform only those that are necessary at this time. The rest will be reviewed again and performed latter.

Update the defects and enhacemnts document to reflect what we plan for this bug fix and to determine the new version number.

Check commits, tags and where HEAD is pointing.

`git log --oneline --decorate`

Make sure you are on the master branch.

`git pull synology master`

Create a new local branch and switch to it:

```
git branch v26.0.0.0
git checkout v26.0.0.0
git status
```

Update the __init__.py file with the release number:

```
vi ~/devl/other_computer_projects/<app>/src__init__.py
git add .
git commit -m 'Updated init.py with the new version number.'
```

### Fix the bugs


# jasmit you are here


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

### Rebase branch into master

Run git status again and insure the branch is up to date with synology and the working directory.

`git status`

Start the rebase.

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


## Enhancements

Check the git status of the development directory. It should be on the master branch. Their may be chages to the documentation that need commited. If so commit them and push them so the working directory is totally in sync.

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


## You are here








git merge feature/vX.Y+1.0
git commit -m 'Commit of feature vX.Y+1.0 into master.'
git push github master
```


## None of the following have been review since being copied in



## Enhancing an existing project
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


## New Releases

## Upgrading to a new release (temporary step)

Clone the code (to test or prod):

```
cd /tmp
git clone https://github.com/jasmit35/<app>.git --branch release/vX.X.X
```

# Git Rebase
1. Make sure you’ve saved all your work

If you have uncommitted changes:

```
git add .
git commit -m "WIP: current changes"
```
2. Switch to main and pull the latest changes

```
git checkout main
git pull origin main
```

This ensures main is up to date.

3. Switch back to your feature branch

git checkout feature/login-form4. Rebase onto main
git rebase main
What happens:
Git removes your commits from the branch temporarily.
Moves your branch to main’s latest commit.
Replays your commits one by one.

5. Resolve conflicts (if any)
If there are conflicts, Git will pause and tell you which files are affected.
For each conflicted file:
Open it
Fix the conflict
Mark it resolved:
git add <file>
Then continue:
git rebase --continue
Repeat until the rebase finishes.

6. Push with --force-with-lease
Because rebase rewrites history, you need to force push:
git push --force-with-lease origin feature/login-form
The --force-with-lease is safer than plain --force-it makes sure you don’t overwrite someone else’s work by mistake.
Now your branch is rebased, clean, and ready for a Pull Request.
The commit history will look like you started after all the latest main commits.

Difference between git pull and git pull --rebase
git pull (default behavior)
When you run:
git pull
Git does two things:
Fetch: It downloads the latest commits from the remote branch (e.g., origin/main).
Merge: It merges those commits into your current branch.
If there are new commits on both branches, Git creates a merge commit.
This can cause those “merge bubbles” we talked about earlier.
Example history after git pull:
A---B---C---M
     \     /
      D---E
M is the merge commit.
2. git pull --rebase
When you run:
git pull --rebase
Git still does:
Fetch: Downloads the latest commits.
Rebase: Moves your local commits to start after the fetched commits.
No merge commit is created - the history is linear.
Example history after git pull --rebase:
A---B---C---D'---E'
D' and E' are your original changes, replayed on top of the latest commits.
Which should you use?
git pull
Simpler if you don’t care about history being messy.
Common in teams where merge commits are fine.
git pull --rebase
Keeps commit history clean and linear.
Easier for git log and git bisect later.
Preferred before opening a PR.
Fin.
:)

Characters That Need to Be Escaped

In Markdown, certain characters are reserved for specific formatting purposes and have special meanings. These characters are used to create headers, lists, links, emphasis, and other elements of the Markdown syntax. If you want to display these characters as part of the content (rather than triggering their formatting function), you must escape them by preceding them with a backslash (\). This tells the Markdown parser to treat these characters as regular text, rather than interpreting them as formatting instructions.

\\ (backslash itself)
` (backtick)
* (asterisk)
_ (underscore)
{ } (curly braces)
[ ] (square brackets)
( ) (parentheses)
# (hash mark)
+ (plus sign)
- (minus sign or hyphen)
. (dot)
! (exclamation mark)




## Files that contain the version number:

env
ansible/playbooks/start_test_service.yaml
ansible/playbooks/start_prod_service.yaml
services/version.py


### EOD
