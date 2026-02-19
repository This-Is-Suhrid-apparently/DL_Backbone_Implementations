# DL_Backbone_Implementations
We have solved the merge conflicts between testing and development.
<<<<<<< HEAD
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

=======

>>>>>>> origin/development
