---
layout: post
title:  "Git: Common commands"
date:   2014-04-10 17:30:00
categories: "2014"
---

### Undo the last Git commit ###

```bash
$ git commit ...              # You messed up, huh?
$ git reset HEAD^    		  # Leaves working tree as it was before sending commit. Since default is --soft the changes done after commit will remain in working tree.
$ [edit]                      # Make corrections to working tree files.
$ git add ...                 # Stage changes for commit.
$ git commit -c ORIG_HEAD 	  # "reset" copies the old head to .git/ORIG_HEAD; redo the commit by starting with its log message. If you do not need to edit the message further, you can give -C option instead.
```
[Source](http://stackoverflow.com/questions/927358/how-to-undo-the-last-git-commit)

---

### Add all files to a commit except a single file ###

```bash
$ git add --all
$ git reset -- <path-to-file>
```

[Source](http://stackoverflow.com/questions/4475457/add-all-files-to-a-commit-except-a-single-file)

---

### Create remote branch ###

First, you create your branch locally

```bash
$ git checkout -b your_branch
```

The remote branch is automatically created when you push it to the remote server. So when you feel for it, you can just do:

```bash
$ git push -u origin <branch-name>
```

Your colleagues would then just pull that branch, and it's automatically created locally.

* `origin` means "the server I got the rest of this repo from".
* `-u` option sets up an upstream branch, so that a subsequent `git pull` will know what to do.

[Source](http://stackoverflow.com/questions/1519006/git-how-to-create-remote-branch)

---

### Revert all local changes to previous state ###

Revert changes made to your working copy:

```bash
$ git checkout .
```

Revert changes made to the index (i.e., that you have added):

```bash
$ git reset <...>
```

Revert a change that you have committed:

```bash
$ git revert <...>
```
Remove *untracked* files:

```bash
$ git clean -fdn
```

* `-f` : --force -> If the Git configuration variable clean.requireForce is not set to false, git clean will refuse to run unless given this
* `-d` : Remove untracked directories in addition to untracked files. 
* `-n` : --dry-run -> Don’t actually remove anything, just show what would be done.

And then when you are sure that you want to delete those files:

```bash
$ git clean -fd
```

[Source](http://stackoverflow.com/questions/1146973/how-do-i-revert-all-local-changes-in-a-git-managed-project-to-previous-state)

---

### Recover dropped stash ###

See: [Source](http://stackoverflow.com/questions/89332/recover-dropped-stash-in-git)