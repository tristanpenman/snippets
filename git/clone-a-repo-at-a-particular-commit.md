# Clone a repo at a particular commit

Sometimes you want to clone a repo, at a particular commit:

    mkdir my_fancy_repo
    cd my_fancy_repo
    git init
    git remote add origin https://github.com/tristanpenman/my_fancy_repo.git
    git fetch origin <commit-id>
    git reset --hard FETCH_HEAD

Instructions found here: https://stackoverflow.com/questions/3489173/how-to-clone-git-repository-with-specific-revision-changeset
