---
layout: post
title:  "Git: Common commands"
date:   2014-04-10 17:30:00
categories: git
---

### Undo the last Git commit? ###

```bash
$ git commit ...              # You messed up, huh?
$ git reset HEAD^    		  # Done when you remembered what you just committed is incomplete or wrong. Leaves working tree as it was before sending commit. Since default is --soft the changes done after commit will remain in working tree.
$ edit                        # Make corrections to working tree files.
$ git add ...                 # Stage changes for commit.
$ git commit -c ORIG_HEAD 	  # "reset" copies the old head to .git/ORIG_HEAD; redo the commit by starting with its log message. If you do not need to edit the message further, you can give -C option instead.
```
[Source](http://stackoverflow.com/questions/927358/how-to-undo-the-last-git-commit)

---

### Add all files to a commit except a single file? ###

```bash
$ git add --all
$ git reset -- [path/to/file]
```

[Source](http://stackoverflow.com/questions/4475457/add-all-files-to-a-commit-except-a-single-file)

---