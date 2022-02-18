# git and github 

## git basics

```bash
git init # to create a new git repository
git add filename.txt

git rm <file> # remove file in repository
git status
git diff filename.txt
git commit -m 'text comment'
```

### A local repository consists of 3 "trees" maintainened by git

	1. Working Directory: holds the actual files 
	2. Index: acts as a staging area
	3. `HEAD`: points to the last commit

​	*working directory $\rightarrow$ stage (index) $\rightarrow$ master*
​	repository includes stage & master (branch)

### To connect with a remote repository:
```bash
git remote add origin git@github.com:path/repo-name.git # connect the local repo with the remote server

# Push local master repository to remote hub
# first time push: 
git push -u origin master
# not first, then: 
git push origin master 
```

### To create a new branch besides the master branch

- Branches are used to develop features isolated from each other
- The *master* branch is the default branch when creating a repo; after done with the development in other branch, can merge them back to the master branch.

```bash
git checkout -b dev # create and switch to a new branch called dev
git checkout -b branch-name origin/branch-name # create a local brach corresponding to the remote branch

git branch # list all branches, * indicates current branch
git merge <branch> # merge <branch> to current branch
git branch -d <branch> # delete <branch>
git branch -D <branch> # force to delete a unmerged branch

# a branch is not available to other until pushed to the remote repository
git push origin <branch>

git branch -m oldname newname # rename a branch
```
### To use tag

```bash
git tag <tag-name> <commit id> # create tag for specific commit under certain branch
git tag 1.0.0 1b2e1d63ff # e.g. tag the commit start with 1b2e1d63ff (first 10 chars)

git tag # check all tags
git show <tag-name> # show detailed tag info
git tag -a <tag-name> -m 'tag description'
git tag -d <tag-name> # delete local tag
git push origin :refs/tags/<tag-name> # delete a remote tag

git push origin <tag-name> # push single tag to remote
git push origin --tags # push all un-pushed tags to remote
```

### To use log

```bash
git log --author=bob # see log from bob
git log --pretty==oneline # see a very compressed log where each commit is one line
git log --name-status # see which files have changed
git reflog #every command ever used in this repository 
```

### To replace local changes

```bash
git checkout --<filename> # to replace/discard local changes; this replaces the changes in your working tree with the last content in HEAD. Changes already added to the index, as well as new files, will be kept.

# to drop all your local changes and commits, fetch the latest history from the server and point your local master branch at it
git fetch origin
git reset --hard origin/master

git reset HEAD <file> #unstage (opposite of git add <file>), put file back to working directory

git reset --hard HEAD^ #to previous version
git reset --hard HEAD~10 #to 10th previous version
git reset --hard commit id #to version with specified id
```

### To solve merge conflicts between two repos

- When merging two repos, there may be *conflicts* that needs to be resolved manually.

1. Create a temp branch and pull the upstream repo into this temp branch (we will resolve the conflicts here and then merge this branch to our final branch that we originally would like to merge with the upstream repo)

   ```bash
   git checkout -b temp-branch master # here master is the branch we originally would like to have the merge with
   ```

2. pull the upstream repo that we would like to merge with into the temp branch

   ```bash
   git pull https://github.com/xxx.git
   ```

3. After the pull, merge conflicts will be shown, if lost can retrieve those with

   ```bash
   git status
   ```

4. Resolve those conflicts in text editor (e.g sublim text): looking for signs as

   ```python
   <<<<<<< HEAD
   open an issue
   =======
   ask your question in IRC.
   >>>>>>> branch-a
   ```

> NOTE: anything between  `<<<<<<< HEAD` and `=======` is in our file and that between `=======` and `>>>>>>> branch-a` is from the upstream file.

5. After cleaning the file, double check the status, if no conflict anymore, add the changes and commit

   ```bash
   git add <filename>
   git commit -m 'Resloved merge conflicts by doing XXX'
   ```

6. Merge the temp branch with the master branch and push to github

   ```bash
   git checkout master
   git merge --no-ff temp-branch
   git push origin master
   ```

> NOTE: all these operations can also be done with the assit of github desktop application.

### To put different local folders under the same repo on Github

- For example, there is a central Github repo call “top”, which as several sub-directories:
  - top/sub1
  - top/sub2
  - ...
- To use each sub-directory for different local project folder, do the following:
- [Reference](https://intellipaat.com/community/21245/git-clone-subdirectory-how-do-i-clone-a-subdirectory-only-of-a-git-repository)

```bash
git init <repo>
cd <repo>
git remote add origin <url> # url is the repo link to "top"
git config core.sparsecheckout true
echo "sub1/*" >> .git/info/sparse-checkout
git pull --depth=1 origin master
```



### Syncing/update a fork

- [Configure a remote that points to the upstream repo.](https://docs.github.com/en/articles/configuring-a-remote-for-a-fork)

  ```bash
  git remote -v
  git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
  ```

- In terminal, change the current working directory to local project

- Fetch the brances and their respective commits from the upstreatm repo

  ```bash
  git fetch upstream
  ```

- Check out the fork's local default branch

  ```bash
  git checkout LocalBranchName
  ```

- Merge the changes from the upstream default branch into the local default branch. This brings the fork's default branch into sync with the upstream repository, without losing local changes.

  ```bash
  git merge upstream/main
  ```
  
  - [another way](https://stackoverflow.com/questions/7244321/how-do-i-update-or-sync-a-forked-repository-on-github) is to use `rebase`, which will erase local changes; after `fetch` and `checkout` 
  
    ```bash
    git rebase upstream/master
    ```
  
    - there may be difference(s) between the remote master and local repo and need to do `git add .` followed by `git rebase --continue` to complete the process
  
    may need to force the push in order to push it to the forked repo on Github

### Others

```bash
git log --graph # branches merging graph

git stash # saving working directory and index state...
git stash list # check saved working directory

git stash apply # recover working contents 
git stash drop #delete saved stash

git stash pop # recover saved stash and remove it after recovering

git remote -v # check remote repository information

git branch --set-upstream branch-name origin/branch-name # create local and remote branch connection
```

- Use `.gitignore` file to ignore some files in the working directory. need to put it in the root git directory 

```bash
git config --global alias.<new-name> <original-name> #set alias for git command. If no --global, only works for current repository
```

- To find the configuration file: `.git/config`


## Github actions
- The main concept at the heart of GitHub actions is the workflow; 
    - this is done by creating a series of YAML files that contain a series of actions that determine your workflow in your repositories. 
  - These YAML files are stored in a special folder in the `.github/workflows\` folder inside the repository.
- The most important information about your repository is stored inside a GitHub object, but there’s a few other, what are called contexts, that you can use, including information about the job that is running or the steps that are currently running as well.
    - There’s also a very special and pretty useful context called `secrets`, which lets you store information in GitHub itself that won’t be seen by anyone else, including other people in your repository. 


## References

1. [INTRODUCTION TO GIT AND GITHUB](https://launchschool.com/books/git/read/introduction) by LaunchSchool

   > A beginner friendly guide to using git and working with Github.com. This book is for the absolute beginner and provides a gentle introduction to git and Github. Get a jump start using git on your projects, and learn how to push those projects to Github.com.

2. [Github action by example](https://www.actionsbyexample.com/index.html)