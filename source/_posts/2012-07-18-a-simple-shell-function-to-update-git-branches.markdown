---
layout: post
title: "A simple shell function to update git branches"
date: 2012-07-18 16:17
comments: true
categories: git shell
---

A client of mine was having a hard time remembering the project workflow to keep topic branches updated, so I put together this simple shell function as a workflow reference:

```bash git-sync-master
function git-sync-master() {
  git checkout master &&
    git fetch origin &&
    git pull origin master &&
    git checkout $1 &&
    git rebase master
}
```

As written, that could be added to a `~/.bash_profile` or similar location allowing you to run something like:

    $ cd myproject
    $ git-sync-master mybranch

Taking this one step further, you can "integrate" this command in to git with the following minor modifications:

    $ cat > ~/bin/git-sync-master
    git checkout master &&
      git fetch origin &&
      git pull origin master &&
      git checkout $1 &&
      git rebase master
    ^D

    $ chmod +x ~/bin/git-sync-master
    $ cd myproject
    $ git sync-master mybranch

