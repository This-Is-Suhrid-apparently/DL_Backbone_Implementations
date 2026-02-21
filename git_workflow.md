# DL_Backbone_Implementations
We have scolved the merge conflicts between testing and development.
Some changes need to be made.
1) Create long‑lived branches
    main → production‑ready
    development → integration branch
    testing → staging/pre‑merge validation
    # make sure main is current
    git checkout main
    git pull

    # create development and push it (set upstream)
    git checkout -b development
    git push -u origin development

    # create testing off development (so testing contains latest integration work)
    git checkout development
    git pull
    git checkout -b testing
    git push -u origin testing
____________________________________________________________________________________________________________________________________________________________________

2) Day‑to‑day workflow

    A) Do work on feature branches (recommended)
        Rather than committing directly to testing or development, create short‑lived feature branches so reviews are clean.

        # start a feature from testing (or development if you prefer)
        git checkout testing
        git pull 
        git checkout -b feature/my-change

        # edit files, then:
        git add .
        git commit -m "Implement X"

        # keep your branch up to date
        git fetch origin
        git rebase origin/testing   # or: git merge origin/testing

        # push and open a PR to `testing`, then merge it on Github
        git push -u origin feature/my-change

    B) Promote from testing → development
        Once a set of changes has passed your testing:
                
        git checkout development
        git pull
        git merge --no-ff testing   # keeps a merge commit for traceability
        git push

    C) Promote from development → main
        When you’re ready for release:
                
        git checkout main
        git pull
        git merge --no-ff development
        git push

        Optionally tag a release:
        
        git tag -a v1.0.0 -m "First stable release"
        git push origin v1.0.0
____________________________________________________________________________________________________________________________________________________________________

3) Steps for making changes and tracking them:

    1. Create new branch : git checkout -b branch_name (typical branch name - sd/branch_name)
    2. Make the changes
    3. See the changes : git status
    4. Stage them: 
        git add -A : Stage all changes
        git add file_path : stage specific files
        git reset : unstages everything
        git reset --hard : unstages everything and removes all changes as well
    5. Commit staged changes: git commit -m "Add Commit Message" (Short and Descriptive)
    6. Push the changes:
        Use this for the first push for every new branch : git push --set-upstream origin sd/branch-jupyter-notebook (takes the branch online)
        Thereafter for further pushing commits simply use: git push
    7. Pull request:
        In Azure DevOps click on Repos/Pull Requests
        Navigate to the repository
        Click on New pull request
        Specify source branch, target branch (into master in this case). Populate comments about the pull request itself. Add reviewers if appropriate, etc
        Once the pull request is created, it can be evaluated (by the reviewers) and then comments can be issued prior to merging
        After the reviewrs approve, click on Complete (select the option which automatically deletes the branch after mergingin remote)
        This completes the process of merging the (source) branch to the (target) master
    8. Sync the changes in the local and remote repositories:
        Navigate to VSCode terminal and enter: git fetch
        Type: git checkout master to go back to the master branch
        :git pull to pull the remote changes into the local repository.
    10. Delete the used branch once its purpose is served : git branch -d branch_name to delete the branch from local repository after pulling the changes.


    Use this for the first push for every new branch
    git push --set-upstream origin sd/branch-jupyter-notebook 


    Use the following command to install the necessary libraries:
    python3 -m pip install -r requirements.txt

    To access and check details of 'torch' and all other libraries that are python dependent, first type python and engage it
    Then import the library to be accessed. e.g. import torch
    Finally check version. e.g. torch.__version__
    exit() to get out of Python
____________________________________________________________________________________________________________________________________________________________________

4) To fork a reserach repo and then clone (the fork) it for personal changes then finalize changes and send PR to the original author:

1. Terms:
    a. origin = my own fork on Github. origin is my remote repo 
    b. upstream = orginal remote repo on Github. This is the repo that I forked and then cloned the fork to use locally. (Generally cannot push to upstream without permissions)
    c. main = my stable branch. main is my local branch and the changes are pushed to its corresponding remote version called origin/main
    d. test_branch = my (feature/experiment) branch other than main. Its changes are pushed to its corresponding remote version called origin/test_branch

2. Process of forking and cloning the fork:
    a. In the original repo's Github page (github.com/ORIGINAL_OWNER/REPO) click Fork create a fork on my Github account (github.com/Me/REPO)
    b. git clone github.com/Me/REPO.git (Cloning the fork)
    c. cd REPO

3. Setting up upstream (since origin is already setup when forked repo is cloned):
    a. git remote add upstream github.com/ORIGINAL_OWNER/REPO.git
    b. Verify remotes (origin and upstream) using git remote -v

4. Keep local main in sync before starting to work:
    a. git checkout main
    b. git fetch upstream
    c. git merge upstream/main (i.e merge upstream's main branch with my local main branch)
    d. git push main (i.e push changes in main (by merging upstream/main to local main) to origin/main (which is the remote version of my local main in my forked repo))

5. Create testing branch and make changes:
    a. git checkout -b test_branch
    b. # Make changes
    c. git status
    d. git add -A (or specific files as and when required)
    e. git commit -m "Commit message."
    f. git push -u origin test_branch (here -u is short for --set-upstream, it pushes changes in local test_branch to remote origin/test_branch | This means future commands ((git push) and (git pull)) will automatically know where to push and pull only for the branch that I am in right now, since upstream tracking is configured per branch and NOT locally.) 

6. Open a PR to the original repo:
    a. Go to my fork : github.com/Me/REPO
    b. Click Pull Requests -> New Pull Request
    c. Set:
        c.1. base repo: ORIGINAL_OWNER/REPO
        c.2. base branch: main (original repo's main branch)
        c.3. compare repo: Me/REPO
        c.4. compare branch: test_branch
    d. Write a clear title + description, then Create pull request. NOTE: Pushing more commits to the same branch (test_branch) automatically updates the same PR.

7. PR gets accepted (merged) by original author.

8. Later author makes further changes -- need to sync my repo with the changes made.
    a. Update local main: git checkout main
    b. See changes: git fetch upstream (see changes made in the original repo)
    c. git merge upstream/main (merge the changes in the original repo's main branch with my local main branch)
    d. Now push the changes in my local main to the remote main: git push origin main

9. Lets say the test_branch was not deleted after its previous purpose was served and I have kept this branch for future development as well - so need to merge the changes that are present in the local main
    a. git checkout test_branch
    b. git merge main (OR directly merge from my remote main : git merge origin/main)
    c. Then push updated test_branch to remote: git push origin test_branch
____________________________________________________________________________________________________________________________________________________________________

5) Set remote origin and remote upstream
    a. git remote add upstream github.com/ORIGINAL_OWNER/REPO.git
    b. git remote add origin github.com/Me/REPO.git
    c. Verify using git remote -v
    d. In the case where repo was cloned from somewhere and I want to change the origin:
        d.1. check through git remote -v
        d.2. Change the URL : git remote set-url origin github.com/Me/new_REPO.git
    e. Remove and Re-add:
        e.1. git remote remove origin
        e.2. git remote add origin github.com/Me/new_REPO.git

____________________________________________________________________________________________________________________________________________________________________
# General git commands:

    1. Check if git is tracking a specific file : git check-ignore -v (filename)
    2. See branch statuses: git branch -vv
    3. git push [-u] <remote> <local-branch> example : git push -u origin test_branch
    4. git merge <remote>/<branch> example : git merge origin/main OR git merge upstream/main
    5. get fetch <remote> example : get fetch origin OR git fetch upstream
    6. git config --get branch.main.remote | ANSWER : origin
    7. git config --get branch.main.merge  | ANSWER : refs/heads/main
