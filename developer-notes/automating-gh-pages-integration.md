---
title: Automating gh-pages branch updating with changes to the master branch
layout: wikistyle
---

Automating gh-pages branch updating with changes to the master branch
================

How to setup a git repo so gh-pages automatically tracks commits to master and 'git push origin' updates the remote master and gh-pages branches.

Setup a test repo:

    mkdir pages-test
    cd pages-test/
    git init .
    echo 'readme' > readme
    git add readme
    git commit -m 'add readme' readme
    git branch gh-pages master

Now there are two identical branches: master and gh-pages. Make a commit to master.

    echo 'readme changed' > readme
    git add readme
    git commit -m 'change readme' readme

Now master is ahead by one commit:

    $ git diff gh-pages --stat
     readme |    2 +-
     1 files changed, 1 insertions(+), 1 deletions(-)

You can force update gh-pages to master like this to make them identical:

    git branch -f gh-pages master

Now add a post-commit hook that does this and make it executable:

    echo '#!/bin/sh
    git branch -f gh-pages master' > .git/hooks/post-commit
    chmod u+x .git/hooks/post-commit

and commit another change on master:

    echo 'another change to readme' > readme
    git add readme
    git commit -m 'another change' readme

The two branches should now be identical:

I've setup my local [energy2d-js](https://github.com/concord-consortium/energy2d-js) repo this way and added two push elements to the origin remote in .git/config:

    [remote "origin"]
        url = git@github.com:concord-consortium/energy2d-js.git
        fetch = +refs/heads/*:refs/remotes/origin/*
        push = refs/heads/master:refs/heads/master
        push = refs/heads/gh-pages:refs/heads/gh-pages

This means anytime I execute: 'git push origin' I'll push both master and gh-pages to their respective branches.

    $ git commit -m 'revert text change, testing hooks' model2d-demo.html
    [master 852fdfb] revert text change, testing hooks
     1 files changed, 1 insertions(+), 1 deletions(-)
    [energy2d-js-git ruby-1.9.2-p180 (master)]$ git push origin
    Counting objects: 5, done.
    Delta compression using up to 4 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (3/3), 314 bytes, done.
    Total 3 (delta 2), reused 0 (delta 0)
    To git@github.com:concord-consortium/energy2d-js.git
       340c2c1..852fdfb  gh-pages -> gh-pages
       c741225..852fdfb  master -> master
