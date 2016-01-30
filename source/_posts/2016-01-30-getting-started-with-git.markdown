---
layout: post
title: "Getting Started With Git"
date: 2016-01-28 00:06:21 +0800
comments: true
categories: Git
---

Find out if Git is installed

    git --version

Create a new repository:

    git init my_first_repository

如果已經有一個特定的資料夾想要用git追蹤，也可以直接在該資料夾下使用:

    $ cd my_project
    $ /my_project > git init
    $ /my_project > ls -a (這邊會發現一個.git的隱藏檔)

Adds files to the repository so that Git knows to track their changes:

    git add README.md

Commits all added files to the repository as a change:

    git commit -a -m "This is my first commit"

Committing everything that's currently in the staging area:

    git commit -m "only commit the stuff in the staging area"

Make configuration changes to Git

    git config --global user.name "Lex Teng"
    git config --global user.email "lex@gmail.com"

Show the current status of the git repository

    git status

Show us a chronological log of all of our commits

    git log

"Check out" a different version

    git checkout f44567 (<= version number)

Create a "diff" view to demonstrate what has changed between two different versions

    git diff f44567 d34563

