---
layout: post
title: "Fixing a Previous Git Commit"
date: 2015-04-01 15:58:49 -0400
comments: true
categories: git
---

Have you ever been hammering out some code, making commits, only to realize 
you've botched something up a few commits ago? Happens to me quite often and
I just realized there is an easy way to fix it via an interactive rebase.

As long as you haven't pushed your commits upstream, follow these steps and
you'll be golden. If you have pushed, just make your fix in a new commit 
because the steps below will result in a new sha being generated for the commit
you're modifying. 

1. Save or stash your existing work.
2. Use `git log` to find the first few characters of the ID of the commit
   you'd like to edit.
3. Begin the interactive rebase by running `git rebase -i ID^`.
4. Find your commit in the list you've been presented and change the word 
   'pick' to 'edit'. Save and quit the editor.
5. Make the necessary changes to your files.
6. Run `git commit -a --amend`.
7. Proceed with the rebase by running `git rebase --continue`.
8. Resolve any conflicts created by your new changes.
9. Re-apply any stashed changes.
