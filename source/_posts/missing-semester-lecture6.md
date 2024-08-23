---
title: "Missing Semester Lecture6: Version Control(git)"
date: 2024-04-26 22:03:57
tags: git
categories:
- [missing semester]
---
### Lecture 6:  Version Control(git)

link:https://missing.csail.mit.edu/2020/version-control/

```bash
31:42    
git add . 
git commit -m <msg>
git log
git cat-file -p <8d-commit-hash/ branch-hash/ file-hash>
32:47    
git commit -a
git add :/
35:23    git log --all --graph --decorate
36:12    git status (have staged or commited or not)
41:41    git-checkout -f <8d-commit-hash / 8d-branch-hash>  (switch branch)
43:11    git diff hello.txt (show the changes in the file compared to the last commit)
43:28    git diff <8d-commit-hash> hello.txt (compared to the branch)
44:33    git diff HEAD hello.txt
46:22    git diff <8d-commit-hash) HEAD hello.txt (change from to)
50:06    
git branch (print all the branches)
git branch -vv (extra verbose)
50:17    
git branch cat (create new branch that points to the HEAD you are currently looking)
git checkout cat
53:56    git branch -b dog
52:53    git log --all --graph --decorate --oneline (more compact view)
56:27    git merge cat (can do cat dog) 
58:51    git merge --abort
1:00:41    
git add . 
git merge --continue > 
git commit -m <msg> / git commit
59:37    <<<<< HEAD points to last snapshot in master branch, >>>> dog points to the branch you're trying to merge.
1:04:17    git init --bare (initiatialize empty git repo in current dir) 
1:04:20    
git remote add <remote name> <remote repository URL>
git push <remote name> <local branch>: <remote branch>
1:07:54    git clone <url> <folder name>
1:10:33    git branch --set-upstream-to=origin/master
1:18:12    
git blame .config.yml (who edit the file on which commit message by who, when) 
git show <commit hash> (to get line changes like git diff)
1:19:22    
git stash (changes saved somewhere) 
git stash pop (get saved back)
1:20:46    git bisect 
1:21:48    git ignore (put file name or *.extension)
```

## Basics

- `git help <command>`: get help for a git command

- `git init`: creates a new git repo, with data stored in the `.git` directory

- `git status`: tells you what’s going on

- `git add <filename>`: adds files to staging area

- ```plaintext
  git commit
  ```

  : creates a new commit

  - Write [good commit messages](https://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)!
  - Even more reasons to write [good commit messages](https://chris.beams.io/posts/git-commit/)!

- `git log`: shows a flattened log of history

- `git log --all --graph --decorate`: visualizes history as a DAG

- `git diff <filename>`: show changes you made relative to the staging area

- `git diff <revision> <filename>`: shows differences in a file between snapshots

- `git checkout <revision>`: updates HEAD and current branch

## Branching and merging

- `git branch`: shows branches

- `git branch <name>`: creates a branch

- ```plaintext
  git checkout -b <name>
  ```

  : creates a branch and switches to it

  - same as `git branch <name>; git checkout <name>`

- `git merge <revision>`: merges into current branch

- `git mergetool`: use a fancy tool to help resolve merge conflicts

- `git rebase`: rebase set of patches onto a new base

## Remotes

- `git remote`: list remotes
- `git remote add <name> <url>`: add a remote
- `git push <remote> <local branch>:<remote branch>`: send objects to remote, and update remote reference
- `git branch --set-upstream-to=<remote>/<remote branch>`: set up correspondence between local and remote branch
- `git fetch`: retrieve objects/references from a remote
- `git pull`: same as `git fetch; git merge`
- `git clone`: download repository from remote

## Undo

- `git commit --amend`: edit a commit’s contents/message
- `git reset HEAD <file>`: unstage a file
- `git checkout -- <file>`: discard changes

# Advanced Git

- `git config`: Git is [highly customizable](https://git-scm.com/docs/git-config)
- `git clone --depth=1`: shallow clone, without entire version history
- `git add -p`: interactive staging
- `git rebase -i`: interactive rebasing
- `git blame`: show who last edited which line
- `git stash`: temporarily remove modifications to working directory
- `git bisect`: binary search history (e.g. for regressions)
- `.gitignore`: [specify](https://git-scm.com/docs/gitignore) intentionally untracked files to ignore
