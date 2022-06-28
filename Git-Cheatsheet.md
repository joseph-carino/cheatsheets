[back to overview](/../..)

# Git<!-- omit in toc -->

- [Mostly used](#mostly-used)
- [Getting Help](#getting-help)
    - [Within git](#within-git)
    - [outside of git](#outside-of-git)
- [Setup](#setup)
    - [Global](#global)
    - [Project](#project)
    - [New](#new)
    - [Existing](#existing)
- [Staging](#staging)
- [Commiting](#commiting)
- [Branching](#branching)
- [Teamwork](#teamwork)
    - [Teamwork Workflow:](#teamwork-workflow)
- [Removing](#removing)
- [Ignoring](#ignoring)
    - [Rules](#rules)
- [Renaming](#renaming)
- [Deploying a subfolder to GitHub Pages](#deploying-a-subfolder-to-github-pages)
- [Stashing](#stashing)
- [Patching](#patching)


# Helpful Commands
```javascript
$ git branch -a                     // show all branches and highlight the one you’re on
$ git reset 5d69206                 // the 7 first numbers of the commit you want to reset
$ git remote prune origin           // Clean-up outdated references to remote branches
$ git reset origin/<branch> --hard  // throw out local
```

# Setup
## Project
```javascript
$ git init // adds git
```
## New
```javascript
$ git add .
$ git commit -m 'initial project version'
$ git remote add origin URL
$ git push -u origin master
```
## Existing
```javascript
$ git clone URL (directory name, optional)
```

# Removing
```javascript
$ git rm -f // remove file when it’s already in the index
$ git rm --cached // keep on hard drive but remove from being tracked
$ git rm --cached -r // remove a folder keep on hard drive but remove from being tracked
$ git rm log&sol;&bsol;&ast;.log // removes all files that have the .log extension in the log/ directory
$ git rm &bsol;&ast;~ // removes all files that end with ~
$ git mv file-from file-to // to rename a file
$ git branch -d branch-name // delete the branch
```

# Patching
```javascript
$ git diff --cached > patch.patch
$ git apply patch.patch
```

# Rebase
- Rebase feature_branch on trunk_branch creates NEW commits of feature_branch commits, above trunk_branch commits
```javascript
$ git pull                  // update
$ git cd feature_branch     // branch to rebase
$ git rebase trunk_branch   // rebase feature_branch on trunk_branch, resolve conflicts
$ git push -f               // force push branch

// if trunk_branch has been rebased since branch to feature_branch, rebase just the commits changed in feature_branch and ignore previous trunk_branch commits (which have been duplicated)
$ git rebase --onto trunk_branch ${commit_before_your_first_commit_on_feature_branch} feature_branch
```
May be useful to check out [git-chain](https://github.com/Shopify/git-chain)

# Squash
```javascript
$ git reset --soft HEAD~X   // reset local back, removing X commits, but leave changes in working directory
$ git commit                // re-commit all changes
```
or, with rebase:
```javascript
$ git rebase -i [HEAD~X]|[commit-hash]  // rebase X commits, or commit-hash of commit before rebased
$ [change all pick to squash/s]
```

# Compare
```javascript
$ git diff b1..b2           // compare changes from b1 to b2
$ git log b1..b2            // compare commits from b1 to b2
```
