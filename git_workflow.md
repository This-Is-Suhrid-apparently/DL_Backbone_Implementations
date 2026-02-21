# DL_Backbone_Implementations
____________________________________________________________________________________________________________________________________________________________________

1) Day‑to‑day workflow

    1. git checkout development
    2. git pull origin development (sync changes from remote development branch to local development branch)

    3. Create a new feature branch : git checkout -b sd/testing/<feature-name if required>
    4. Work and make changes inside sd/testing/<feature-name>
    5. git add -A
    6. git commit -m "Commit Message."
    7. For first git push from this new branch, we need to set this branch's remote : git push -u origin sd/testing/<feature-name>, thereafter just git push

    8. Go to Github merge PR from sd/testing/<feature-name> to developement:
        a. base: development & compare: sd/testing/<feature-name>
    
    9. Go back to development and sync changes from origin/development: 
        a. git checkout development
        b. git pull origin development

    10. Delete remote testing branch: git push origin --delete sd/testing/<feature-name>

    
    11. Delete local testing branch: git branch -d sd/testing/<feature-name>

    12. Go to Github merge PR from development to main:
        a. base: main & compare: development

    13. Go back to main and sync changes from origin/main: 
        a. git checkout main
        b. git pull origin main

    14. Optional but common - Sync development with local main to stay in sync with main (important if main is changed individually)
        a. git checkout development
        b. git merge main (merge local main to development)
        c. git push origin development (sync remote development to local development)
____________________________________________________________________________________________________________________________________________________________________

2) Steps for making changes and tracking them:

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

3) To fork a reserach repo and then clone (the fork) it for personal changes then finalize changes and send PR to the original author:

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

4) Set remote origin and remote upstream
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
    2. See branch statuses: 
        a. git branch -vv (check if branch has an upstream and what it is)
        b. git config --get branch.<branch-name>.remote and git config --get branch.<branch-name>.merge
    3. git push [-u] <remote> <local-branch> example : git push -u origin test_branch
    4. git merge <remote>/<branch> example : git merge origin/main OR git merge upstream/main
    5. get fetch <remote> example : get fetch origin OR git fetch upstream
    6. git config --get branch.main.remote | ANSWER : origin
    7. git config --get branch.main.merge  | ANSWER : refs/heads/main

____________________________________________________________________________________________________________________________________________________________________

Other Stuff ...