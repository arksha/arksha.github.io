---
layout: post
title: Blog messy build up log
date: 2016-03-21
categories: web
---

##About git I don't know much##

`git help`  = `man git-log`
`git help log` will show specific usage of command, type `space bar` will roll to next page
`git diff filename.html` to check difference, use`f`or `b`to move forward and backward to see whole log, it is pager, a page shows the results.

`git mv a b` to rename a file

`git ls -la` special -la operation will show the hidden dash file, any file has a . before will be hidden(like .git).

`git log -n 2` get log of 2 log
`git log --since=2016-03-21` get log since that date
`git log --until=2016-03-21` get log until that date
`git log --author="Ting"`get log by certain author
`git log --grep="keyword"`get log search commit message with keyword,**this is why good commit message is really improtant**

`git checkout -- file.html (or directory)` stay on the same branch and find this file or dir
`git reset HEAD file.html` go HEAD pointer go last commit and reset as what last happend to undo changes 
`git commit --amend -m "message"` to amend commit 

