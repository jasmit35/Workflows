# Workflows - User's Guide

## Preliminary setup

### Determine project type

Think about the new project and decide what 'type' it is.
All projects will normally fall into one of three 'types';

- Computer Server Projects
- Other Computer Projects
- Non-Computer Projects

If it is not obvious what type the project is, you can review Appendix A. Some sections of this document apply to all three types. Other sections my only apply to one or two. 

### Chose a name (all project types)

A most important first step is to select an appropriate name for the project. It is going to be used for creating directories, documents and other items. Put some thought into this step before proceding. It is always possible to rename a project later, but the farher you are along with devlepment, the more difficult it becomes.

I refer to two types of names for each project. The internal and external name. The internal name is used for directories, files, and anything that might be part of  automation or code. We use snake case for that name. The external name is the one that will be seen in documentation and other places where it is "read" by humans rather than code. It uses camel case.

I will use my implementation of Postgres in the examples so the project's name is "my_postgres" or "My Postgres" depending on where it is used.

For computer server projects we will often be implementing our own version of an external project. In that case the name would normally be "my_\<external_name\>". As shown for Postgres. Other projects within this type are the build out of a new server or cluster. In that case you can leave off the "my_" prefix. So "menaki", "babyblue01", "devl_swarm", etc. 

For 'Other computer projects' and 'Non-computer projects' naming is not as straight forward. "Cloud Flare" for example is an 'OCP' project to implement a Cloud Flare account for my servers. "Security Cameras" might be an 'NCP' project to install and connect one or more cameras.


## Tool setup

It is time to start setting up the various "tools" that we will use for most projects. This is split into two stages explained below.

### Stage 1

The first stage is basically to reserve our new project name in the tools. This should help to identify posible conflicts or concerns that might cause me to change the project name.

#### Directories and files

On the development machine (enki) use go_csp or go_ocp to move to the correct type directory for the project. List whats their to insure the name if free. Then create just the top level directory for the project.

```
go_ocp
ll
mkdir my_postgress
```

#### OmniFocus

The first two project types are already in OF so all you need to do is add the new project to them. If the project was already added and it is the right one, review any existing task. Hold off on adding more task until later.

Non-Computer projects are handled differently. Their are top level directories for things like "Automobiels", "Financial", "House & Home", etc. Thier is a "projects" folder under each of those these. Add the new non-computer project there.

#### Safari

I add a bookmark folder to Safari to hold bookmarks related to the project. The three project types are already created, so at this stage just add a new folder to the approprate one. No need to start populating it until later when working on the project.

#### Git and GitHub

All of the first two types of projects should have a Git repository on Synology. Non-computer projects will only use Git if they contain digit documents that would benifit from version control. The instructions for setting up the repository are in Appendix A. That is because existing projects may not have a repository, so they need to reference "Appendix A" during different stages of development in their workflow. 

As for GitHub, it is a judgement call for each project. If the project has a lot of code and would benifit from GitHub features like DeependaBot, then create the GitHub repository. You would also need to decide if it will be a public or private repository.


### Stage 2

The second stage of my workflow is to "flesh out" what has already been created. This could be runnig a scrpt to create more of the standard directory structure, Adding task to OmniFocus etc.

#### Directories and files

On my development machine (currently "enki"), use "uv" to create the initial new directory and structure rather than "mkdir". It creates a bunch of Python related stuff that you may not use, but it is easy to ignore until you are SURE you will never use Python as a part of the project. It can then easily be deleted.

First lets set up some environment variables to work with our project. Then use them to run "uv".

```zsh
export my_new_proj="<project_name>"
```

```
export my_new_proj_type="computer_server_projects"
--- or ---
export my_new_proj_type="other_computer_projects"
```

```zsh
export my_new_proj_home="${HOME}/devl/${my_new_proj_type}/${my_new_proj}"
mdcd ${my_new_proj_home}
uv init --name ${my_new_proj} --python 3.14 --vcs git --app
```

Remember DON'T use Camel Case syntax for the name (i.e. NOT "MyPostges") instead use Snake Case (i.e. "my_postgres"). Use snake case for all directory and file names. Only use camel case where the name is purely visual like within documentation.


# jasmit You are here!


#### OmniFocus

#### Safari

#### Git and Github

## Initial development

## Bug fixes

## Enhancements

## Project appendixes

### Apendix 1 - Project "Types"

### Apendix 2 - Version numbering

## Tool apendixes

### Apendix A - Git and GitHub

As previously documented all CSP, OCP and even some non-computer projects will have a Git repository on Synology. A repository on GitHub is optional.

Their are three portions to get this set up. The ones needed depend on the projects workflow status at this point.

#### Git local repository

Check the home directory for the project. If it has a ".git" directory the local repository is already established and you can skip the next step.

To build a local git repository follow these commands:

```
cd ${app_project_home}
git init
git add .
git commit -m 'Initial commit.'
git status
```

#### Git remote respository - Synology

Determine if the project already uses Synology as a remote:

```
cd ${app_project_home}
git remote -v
```

If you see Synology already listed, skip to the next step. Otherwise sign on to the Synology server and create a new directory for the project. Use the directory to store a new empty Git repository.

```zsh
ssh -p 2222 synology.local

cd /volume2/GITRepo
mkdir <project_name>.git
chmod 0750 <project_name>.git
cd <project_name>.git
git init --bare
exit
```

Now get everything created so far into the new remote repository:

```
cd ${my_new_proj_home} ; pwd
git remote add synology ssh://jeff@synology.local:2222/volume2/GITRepo/${my_new_proj}.git
git remote -v
git push synology
git pull synology
git add .
git commit -m 'First push to Synology.'
git push synology
git status
```

# jasmit you are here!

If the project needs a repository on GitHub, we do basically the same thing we did for Synology.
See if it already exist. If not, create it. Get our code base set up to use it, then get them both in sync.







git remote add github https://github.com/jasmit35/`*project_name*`.git

git pull github
git push github



### Apendix B - Python - Tips and Tricks

### Appendix C - Markdown - Tips and Tricks

### Appendix D - Ansible - Tips and Tricks

## --- EOD ---



**Ansible portion:**

Build some of the Ansible stuff (by hand for now).

On the target machine for the project:

```zsh
cd ${my_new_proj_home} ; pwd
mdcd ansible
ln -s ~/devl/other_computer_projects/my_ansible/ansible/inventories/devl inventory
mkdir playbooks
mkdir roles
ansible-galaxy role init common --init-path roles
```

Use the current version of the Ansible Playbook "site.yaml" from my "Workflows" project to automatically build what we can.

First make sure you have switched to the machine and project directory for "Workflows". Create the file to hold the group_var variables for the project.

```zsh
echo ${my_new_proj}
cd ~/devl/other_computer_projects/workflows/ansible ; pwd
touch inventory/group_vars/${my_new_proj}_grp.yaml
```

Load some initial items into the file:

```
echo "---" >>inventory/group_vars/${my_new_proj}_grp.yaml
echo "app_name: ${my_new_proj}" >>inventory/group_vars/${my_new_proj}_grp.yaml
echo "..." >>inventory/group_vars/${my_new_proj}_grp.yaml
```

Edit the files to set the correct values for the new project's group name.

```zsh
echo ${my_new_proj_home}
vi ${my_new_proj_home}/ansible/site.yaml
vi inventory/group_vars/explorer_grp.yaml
```

You shuld be able to run and possibly have to debug the ansible playbook for the new app:

```zsh
cd ~/devl/other_computer_projects/workflows/ansible ; pwd
ansible-playbook site.yaml
```

Perform the first git commit. 

```
cd
git add .
git commit -m "Initial commit. The manual portion of the "Workflow" setup has been completed. This commit is just before we start the Ansible automated portion."
git status
```

#### OmniFocus

Create a new project (normally a single action project) with our new name in the correct already existing 'type' folder. It is not necessry to start adding and scheduling task at this time (unless known some already). You just want to reserve the project name and to have an obivious place to add tasks as you come up with them.

#### Safari bookmarks

Use "Edit Bookmarks" to create a new folder under the exisitng approriate 'type' folder. You can use camel case here for the name. Its more "look and find" the project than "change directory" to it.

#### Github repository

Always create a repository on Synology for the project. The instructions are in Appendix A of this document. Decide if the project also needs a repository on GitHub. I will document the reasons why it might at some point, but for now it's just a gut feel.

If you are not certain about the project name you can delay this stem until the repository is actually needed.


## Making bug fixes

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
