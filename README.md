- [Git & GitHub Class Activity](#git---github-class-activity)
    * [Creating a Fork](#creating-a-fork)
    * [[Optional] Keeping Your Fork Up to Date](#-optional--keeping-your-fork-up-to-date)
    * [Coding your own changes](#coding-your-own-changes)
        + [Create a Branch](#create-a-branch)
    * [Submitting a Pull Request](#submitting-a-pull-request)
        + [Cleaning Up Your Work](#cleaning-up-your-work)
        + [Submitting](#submitting)
    * [Accepting and Merging a Pull Request](#accepting-and-merging-a-pull-request)
        + [Checking Out and Testing Pull Requests](#checking-out-and-testing-pull-requests)
        + [Automatically Merging a Pull Request](#automatically-merging-a-pull-request)
        + [Manually Merging a Pull Request](#manually-merging-a-pull-request)
        

# Git & GitHub Class Activity
<small><i><a href=''>Global Software Development - DePaul University - Vahid Alizadeh</a></i></small>


Whether you want to collaborate on an open source project or on your own projects, knowing how to properly fork and generate pull requests is essential.
In this short class activity, you will practice the required steps: Forking, Cloning, Coding, Issuing a pull request, and merging it back to the main repository.

## Creating a Fork
- Fork this repository into your GitHub account. click the "Fork" button. Once you've done that, 
you can use your favorite git client to clone your repo or just head straight to the command line.

- Clone the forked project (using the git repository link from your own account)

```shell
# Clone your fork to your local machine
git clone git@github.com:USERNAME/FORKED-PROJECT.git
```

## [Optional] Keeping Your Fork Up to Date

This step is not necessary, nut if you plan to work on a code base for more than just a quick fix or small modification, you'll want to make sure you keep your fork up to date by tracking the original "upstream" repo that you forked. To do this, you'll need to add a remote:

```shell
# Add 'upstream' repo to list of remotes
git remote add upstream https://github.com/UPSTREAM-USER/ORIGINAL-PROJECT.git

# Verify the new remote named 'upstream'
git remote -v
```
Whenever you want to update your fork with the latest upstream changes, you'll need to first fetch the upstream repo's branches and latest commits to bring them into your repository:

```shell
# Fetch from upstream remote
git fetch upstream

# View all branches, including those from upstream
git branch -va
```

Now, checkout your own master branch and merge the upstream repo's master branch:

```shell
# Checkout your master branch and merge upstream
git checkout master
git merge upstream/master
```

If there are no unique commits on the local master branch, git will simply perform a fast-forward. However, if you have been making changes on master, you may have to deal with conflicts. When doing so, be careful to respect the changes made upstream.

Now, your local master branch is up-to-date with everything modified upstream.

## Coding your own changes

### Create a Branch
Whenever you begin work on a new feature or bugfix, it is important that you create a new branch. Not only is it proper git workflow, but it also keeps your changes organized and separated from the master branch so that you can easily submit and manage multiple pull requests for every task you complete.

To create a new branch and start working on it:

```shell
# Checkout the master branch - you want your new branch to come from master
git checkout master

# Create a new branch named [FirstnameLastnameDev] for example: VahidAlizadehDev (give your branch its own simple informative name)
git branch FirstnameLastnameDev

# Checkout to your new branch
git checkout FirstnameLastnameDev
```

- Modify [TestApp.java](src\main\java\com\gsd\TestApp.java) and add your simple output as instructed in the TODO line.

## Submitting a Pull Request

### Cleaning Up Your Work

Prior to submitting your pull request, you might want to do a few things to clean up your branch and make it as simple as possible for the original repo's maintainer to test, accept, and merge your work.

If any commits have been made to the upstream master branch, you should rebase your development branch so that merging it will be a simple fast-forward that won't require any conflict resolution work.

```shell
# Fetch upstream master and merge with your repo's master branch
git fetch upstream
git checkout master
git merge upstream/master

# If there were any new commits, rebase your development branch
git checkout FirstnameLastnameDev
git rebase master
```

Now, it may be desirable to squash some of your smaller commits down into a small number of larger more cohesive commits. You can do this with an interactive rebase:

```shell
# Rebase all commits on your development branch
git checkout 
git rebase -i master
```

This will open up a text editor where you can specify which commits to squash.

### Submitting

Once you have committed and pushed all of your changes to GitHub, go to the page for your fork on GitHub, select your development branch, and click the pull request button. If you need to make any adjustments to your pull request, just push the updates to GitHub. Your pull request will automatically track the changes on your development branch and update.

## Accepting and Merging a Pull Request

Take note that unlike the previous sections which were written from the perspective of someone that created a fork and generated a pull request, this section is written from the perspective of the original repository owner who is handling an incoming pull request. Thus, where the "forker" was referring to the original repository as `upstream`, we're now looking at it as the owner of that original repository and the standard `origin` remote.

### Checking Out and Testing Pull Requests
Open up the `.git/config` file and add a new line under `[remote "origin"]`:

```
fetch = +refs/pull/*/head:refs/pull/origin/*
```

Now you can fetch and checkout any pull request so that you can test them:

```shell
# Fetch all pull request branches
git fetch origin

# Checkout out a given pull request branch based on its number
git checkout -b 999 pull/origin/999
```

Keep in mind that these branches will be read only and you won't be able to push any changes.

### Automatically Merging a Pull Request
In cases where the merge would be a simple fast-forward, you can automatically do the merge by just clicking the button on the pull request page on GitHub.

### Manually Merging a Pull Request
To do the merge manually, you'll need to checkout the target branch in the source repo, pull directly from the fork, and then merge and push.

```shell
# Checkout the branch you're merging to in the target repo
git checkout master

# Pull the development branch from the fork repo where the pull request development was done.
git pull https://github.com/forkuser/forkedrepo.git FirstnameLastnameDev

# Merge the development branch
git merge FirstnameLastnameDev

# Push master with the new feature merged into it
git push origin master
```

Now that you're done with the development branch [FirstnameLastnameDev], you're free to delete it.

```shell
git branch -d newfeature
```

======================================================

**Additional Reading**
* [Atlassian - Merging vs. Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)

**Sources**
* [GitHub - Fork a Repo](https://help.github.com/articles/fork-a-repo)
* [GitHub - Syncing a Fork](https://help.github.com/articles/syncing-a-fork)
* [GitHub - Checking Out a Pull Request](https://help.github.com/articles/checking-out-pull-requests-locally)