[back to overview](/../..)

# Git<!-- omit in toc -->

- [Helpful Commands](#helpful-commands)
- [Setup](#setup)
    - [Project](#project)
    - [New](#new)
    - [Existing](#existing)
- [Removing](#removing)
- [Patching](#patching)
- [Rebase](#rebase)
- [Push --force](#push---force)
- [Pull (from force push)](#pull-from-force-push)
- [Squash](#squash)
- [Compare](#compare)
- [Graphite](#graphite)
    - [Common Commands](#common-commands)
    - [Common Workflows](#common-workflows)
        - [New work](#new-work)
        - [Sync](#sync)


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

# Push --force
[see here](https://stackoverflow.com/questions/65837109/when-should-i-use-git-push-force-if-includes)
When pushing, use -f or --force to overwrite origin with your copy of branch. If this may cause issues, try:
```javascript
$ git push --force-with-lease [--force-if-includes] origin branch
```
Force push **branch**, but ONLY if remote/origin/**branch** has the same tip commit as local/origin/**branch**. If --force-if-includes, ONLY if remote/origin/**branch** has the same tip commit as the branch point of local/**branch** from local/origin/**branch**

# Pull (from force push)
git reset --hard origin/branch

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

# Graphite
[cheatsheet](https://graphite.dev/docs/cheatsheet)

## Common Commands
```javascript
gt log short / gt ls // review stack log

// Navigate
gt co [branch] // checkout branch. interactive without branch name
gt up [n] / gt u [n] // move up n branches
gt down [n] / gt d [n] // move down n branches
gt top / gt t // top-most branch
gt bottom / gt b // bottom-most branch
gt co -t // checkout trunk (i.e. main)

gt create -[a][m] [name] // create commit and create commit. If no name specified, generate name from commit message.
// -a adds all files
// -m add commit message inline

gt modify -[a] / gt m -[a] // amend commit
// -a adds all files

gt absorb -[a] // absorb changes into the correct commits downstream
// -a adds all files

gt submit -[u] // push branches from trunk to here, creating/updating PR for each branch.
// -u only update existing PRs
gt submit -s / gt ss // push descendant branches as well

gt get [branch] // checkout an existing branch and restack from trunk to tip
// if no branch provided, uses current branch

gt sync // pull changes for all branches and restack all branches
```

## Common Workflows

### New work
```javascript
gt co [branch] // interactive without branch

...write some code

gt create -am [name] // create commit and branch

gt ss -u // create PR
```

### Sync
