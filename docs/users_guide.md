# Workflows (revised)

## The beginning

Think about the new project and decide what 'type' it is.
All projects will normally fall into one of three 'types';

- Computer Server Projects
- Other Computer Projects
- Non-Computer Projects

Each has its own section in this document. Give them a quick review if necessary to decide
which type is appropriate for the new project. 
&nbsp;

&nbsp;

### First steps

Their are several steps that all three types have in common when first getting started.&nbsp;

&nbsp;

#### Chose a name

An obvious first step is to select an appropriate name for the project. It is going to be used for creating directories, documents and other items. Put some thought into this step before proceding. It is always possible to rename a project, but the farher you are along with devlepment, the more difficult it becomes.

For computer server projects we will often be implementing our own version of an external project. In that case the name should be "my_\<ExternalName\>". For example, I will use my implementation of Ansible in the examples so the project name is "my_ansible" or "My Ansible" depending on where it is used.

Naming for 'Other computer projects' and 'Non-computer projects' is not as straight forward. "Cloud Flare" for example is an 'OCP' project to implement a Cloud Flare account for my servers."Security Cameras" might be an 'NCP' project to install and connect one or more cameras.&nbsp;

&nbsp;

#### Development setup (This step to be replaced with Ansible code)

On my development machine (currently "enki"), use "uv" to create the initial new directory and structure rather than "mkdir". It creates a bunch of Python related stuff that you may not use, but it is easy to ignore until you are SURE you will never use Python as a part of the project. It can then easily be deleted.

```bash
go_csp
export my_new_proj=home_assistant ; echo ${my_new_proj}
mdcd ${my_new_proj}
uv init --name ${my_new_proj} --python 3.14 --vcs git --app
```

Remember DON'T use Camel Case syntax for the name (i.e. NOT "MyAnsible") instead use Snake Case (i.e. "my_ansible"). Use snake case for all directory and file names. Only use camel case where the name is purely visual like within documentation.

Use the current version of the Ansible Playbook "init-proj-files" from my "Workflows" project to automatically build what we can.

Build some stuff by hand (for now).

```
go_csp ; cd ${my_new_proj} ; pwd
mdcd ansible
ln -s ~/devl/other_computer_projects/my_ansible/ansible/inventories/devl inventories
mkdir playbooks
mkdir roles
ansible-galaxy role init common --init-path roles
```

&nbsp;

&nbsp;

Ansible

Edit the file to set the value for "hosts:" to the new projects' group name.

```
cd ~/devl/other_computer_projects/workflows/ansible
vi site.yaml
```

&nbsp;

&nbsp;

#### OmniFocus

Create a new project (normally a single action project) with our new name in the correct already existing 'type' folder. It is not necessry to start adding and scheduling task at this time (unless known). You just want to reserve the project name and to have an obivious place to add tasks as you come up with them.

#### Safari bookmarks

Use "Edit Bookmarks" to create a new folder under the exisitng approriate 'type' folder. You can use camel case here for the name. Its more "look and find" the project than "change directory" to it.

#### Github repository

Always create a repository on Synology for the project. The instructions are in Appendix A of this document. Decide if the project also needs a repository on GitHub. I will document the reasons why it might at some point, but for now it's just a gut feel.

If you are not certain about the project name you can delay this stem until the repository is actually needed.
&nbsp;

&nbsp;

## Bug fixes

Use git to pull the existing code from the master branch on GitHub:

**Note:** See if modifications needed for "git rebase".

```bash
cd ~/devl/<app>
git fetch
git checkout master
git pull github master
git branch bugfix/vX.Y.Z
git push -u github bugfix/vX.Y.Z
git checkout bugfix/vX.Y.Z
```

Fix and test all the bugs listed in the document .../shared/Project

Documentation/\<app\>/Bugs and New Features

Follow the project's documentation for testing a new release, but use the bugfix branch for the testing.

Once all testing is successful, merge the changes into the master branch.

**Note:** See if modifications needed for "git rebase".

```bash
git status
git fetch
git checkout master
git pull github master
git merge bugfix/vX.X.X
git commit -m 'Commit of bugfix/vX.X.X into master'
git push github master
```

Once all desired bug fixes and new features have been completed, create a new release tag (see below).
&nbsp;

&nbsp;

## Workflows (still needs revision)

## <a name="computer-server-projects"></a>Computer Server Projects

Use the standard Ansible playbooks to configure a server

```bash
cd /Users/jeff/devl/ansible/
make PLAYBOOK=std_ids_groups HOSTS=explorer, run-on-hosts
make PLAYBOOK=std_ids_userids HOSTS=explorer, run-on-hosts
make PLAYBOOK=std_privs HOSTS=explorer, run-on-hosts
```

## <a name="other-computer-projects"></a>Other Computer Projects

```bash
.
├── ansible
│   ├── playbooks
│   └── templates
├── LICENSE
├── pyproject.toml
├── README.md
├── src
│   └── workflows
│       ├── __init__.py:0
│       ├── config.py
│       ├── main.py
│       ├── models
│       ├── routes
│       ├── services
│       └── utils
├── tests
└── uv.lock
```

### main.py

main.py — The Entry Point Should Be Thin
This file is wiring — not logic.

In a FastAPI app, for example:

```bash
from fastapi import FastAPI
from app.routes import user_routes

app = FastAPI()
app.include_router(user_routes.router)

### config.py — Centralized Configuration
```

Environment variables should live in one place.
Example:

```bash
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    database_url: str
    secret_key: str

settings = Settings()
```

That’s it.
No database logic.
No business rules.
No heavy computation.
If main.py grows large, your architecture is leaking.

## Services directory

All of the "busniss logic" belongs in this directory.

## models/ — Data Representation Only
This folder contains:
ORM models (SQLAlchemy, Django models)
Pydantic schemas
Data contracts
Example:

```bash
models/
  user.py
  order.py
```

## routes/ — The API Layer Only
Routes should do exactly three things:
Validate request
Call service
Return response
Nothing else.
If your route file looks like:

```bash
@router.post("/users")
def create_user(user_data: UserCreate):
    user = user_service.create_user(user_data)
    return user
```

You’re doing it right.
If your route file is 500 lines long, something is wrong.

## utils/ — Use Carefully
utils/ is dangerous.
It can easily become:
The junk drawer of your project.
Only place:
Small stateless helpers
Truly reusable generic functions
If a utility grows large:
Promote it into its own module
Move it into services/
Give it a real home
Good structure is about resisting the urge to dump everything into utils.py.

## tests/ — Outside app/, Always

Tests should mirror your structure:

```bash
tests/
    test_user_service.py
    test_routes.py
```

## How This Scales as Projects Grow

As complexity increases, I evolve the structure — not by adding chaos, but by refining granularity.
Instead of:
services/user_service.py
It becomes:
services/user/
    create.py
    update.py
    delete.py

## <a name="non-computer-projects"></a>Non-Computer Projects



## Appendix A - Establishing a GIT repository on Synology

Many if not all of my projects will use GIT as the Version Control System (VCS). Rather than go thru the hassle of creating a repository on GitHub for all of them, I will use my Synology NAS. This works well for my Linux and MacOS systems. And I don't care about Windows at this time.

### Create the repository

Sign on to the Synology server and create a new directory for the project:

```bash
ssh -p 2222 synology.local

cd /volume2/GITRepo
mkdir <project_name>.git
chmod 0750 <project_name>.git
cd <project_name>
git init --bare
```
 
### Add the repository to the project


## Appendix B - Initial GIT commit and push

```bash
cd /Users/jeff/devl/<project name>/
git init
git add .
git commit -m 'Initial Commit'

git remote add github https://github.com/jasmit35/`*project_name*`.git
git remote add synology ssh://jeff@synology.local:2222/volume2/GITRepo/<Project>.git

git pull github
git push github

git pull synology
git push synology
```

# Appendix Z - ? Markdown
 
Characters That Need to Be Escaped

In Markdown, certain characters are reserved for specific formatting purposes and have special meanings. These characters are used to create headers, lists, links, emphasis, and other elements of the Markdown syntax. If you want to display these characters as part of the content (rather than triggering their formatting function), you must escape them by preceding them with a backslash (\). This tells the Markdown parser to treat these characters as regular text, rather than interpreting them as formatting instructions.

* \\ (backslash itself)
* ` (backtick)
* * (asterisk)
* _ (underscore)
* { } (curly braces)
* [ ] (square brackets)
* ( ) (parentheses)
* \# (hash mark)
* + (plus sign)
* - (minus sign or hyphen)
* . (dot)
* ! (exclamation mark)


# Enhancing an existing project
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


## Appendix C - Adding a new feature

Check the git status of the development directory. It should be on the main branch. Their may be chages to the documentation that need commited. If so commit them and push them so the working directory is totally in sync.

Use git to create a new feature branch.


```bash
git pull --rebase synology master
git branch feature/2026.X.X
git push -u synology feature/2026.X.X
git checkout feature/2026.X.X
```

Code and test the new feature.


Make sure all changes are in:

```bash
git add .
git commit -m 'Final commit before migration to test'
```

Merge the new feature into the master branch:

```bash
git checkout master
git pull synology master
git checkout feature/X.X.X
git rebase master
```

Thier ahould not be any conflicts, but if there are, edit the file to resolve the confict then add it back and continue the rebase. 

```bash
git add <file>
git rebase --continue
```

### Mergeing a bug or feature branch into master branch


git merge feature/vX.Y+1.0
git commit -m 'Commit of feature vX.Y+1.0 into master.'
git push github master
```


## None of the following have been review since being copied in



## New Features
Use git to create a new feature branch (local and on GitHub) then move to it.

```
git fetch
git checkout master
git pull github master
git branch feature/vX.X.X
git push -u github feature/vX.X.X
git checkout feature/vX.X.X
```

Code and test the new feature.


Make sure all changes are in:

```bash
git add .
git commit -m 'Final commit before migration to test.'
```
Merge the new feature into the master branch:

```bash
git checkout master
git merge feature/vX.Y+1.0
git commit -m 'Commit of feature vX.Y+1.0 into master.'
git push github master
```

## New Releases

Create a new local branch and switch to it:

```bash
git fetch
git checkout master
git branch release/vX.X.X
git checkout release/vX.X.X
```

Update the __init__.py file with the release number:

```bash
vi ~/devl/<app>/local/python/__init__.py
git add .
git commit -m 'Updated init.py with the correct release number.'
```

Create a new branch on GitHub based on the new local branch:

```bash
git push -u github release/vX.X.X
```

Switch back to the local master branch so no changes are made to the release:

```bash
git checkout master
```

## Upgrading to a new release (temporary step)

Clone the code (to test or prod):

```bash
cd /tmp
git clone https://github.com/jasmit35/<app>.git --branch release/vX.X.X
```

# Git Rebase

Make sure you’ve saved all your work.

If you have uncommitted changes:

```bash
git add .
git commit -m "WIP: current changes"
```

Switch to master and pull the latest changes

```bash
git checkout master
sudo git pull synology master
```

This ensures master is up to date.

Now rebase your new branch into master.

```bash
git rebase master feature/2026.0.2
```

What happens:
Git removes your commits from the branch temporarily.
Moves your branch to master’s latest commit.
Replays your commits one by one.

Resolve conflicts (if any).
If there are conflicts, Git will pause and tell you which files are affected.

For each conflicted file:
- Open it
- Fix the conflict
- Mark it resolved:
- git add <file>

Then continue:
git rebase --continue
Repeat until the rebase finishes.

6. Push with --force-with-lease
Because rebase rewrites history, you need to force push:


```bash
git push --force-with-lease synology
```

The --force-with-lease is safer than plain --force-it makes sure you don’t overwrite someone else’s work by mistake. Now your branch is rebased, clean, and ready for a Pull Request. The commit history will look like you started after all the latest main commits.


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

## EOD
