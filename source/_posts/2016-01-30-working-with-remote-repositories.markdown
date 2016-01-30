---
layout: post
title: "Working with Remote Repositories"
date: 2016-01-30 01:40:21 +0800
comments: true
categories: Git
---

Git 的clone, pull和push 是很常會看的用法，這邊以本機端執行的方式來大概說明一下其中的用法，照下面的步驟實際做一次，應該就會對git remote的概念有初步的瞭解。
(本篇文章是整理Treehouse中的Git Basics)：

首先假設我們的資料夾中已經有一個專案good_project

    $ ls
    documents   hello.txt good_project

我們可以使用git clone將這整個資料夾做複製 (記得在git的世界中資料夾的名稱是沒有影響的！)

    $ git clone -/good_project our_clone_project

你會發現在資料夾中多了一個我們clone的folder

    $ ls
    documents   hello.txt good_project         our_clone_project

切換到該資料夾，並使用git log，你會發現這個clone的資料夾完全複製了good_project

    $git log

接下來就會有remote的概念了！在our_clone_project中，下git remote你會發現git 在clone的同時，也幫我們設定了一個remote repository

    ~/out_clone_project $ git remote
    origin

但是這邊注意，good_project卻不會將our_clone_project設定為remote repository，意味著our_clone_project所做的任何變動，good_project都不會收到通知（這在某些專案開發中是好的，但是在這邊我們希望能讓兩的專案資料夾可以互相溝通）

    ~/good_project $ git remote
    (nothing pop out)

    ~/good_project $ git remote add our_clone ~/our_clone_project
    (our_clone是remote repository的名稱，~/our_clone_project可以是網址，一般來說也應該要是網址)

    ~/good_project $ git remote
    our_clone (看到remote的資料夾了！)

做完以上的步驟，就表示我們已經將兩個專案資料夾接起來了，接著我們要來看看他們如何使用push以及pull做溝通。

-------------------

假設我們在一開始，資料夾中就有三個檔案file1, file2 and file3，我們試著修改其中的file2並且將修改做commit:

    ~/our_clone_project $ git checkout –b new_feature (開一個新的branch)
    ~/our_clone_project $ nano file2

    編輯file2中的檔案
    原本：
    This is file2

    修改成：
    This is file2, you guys!!

    ~/our_clone_project $ git commit –a –m “Edit file2 for new_feature”

這邊就完成的我們的修改，但是要怎麼要讓remote的repository知道我們已經做了修改呢？第一個想到的就是用push指令了

    ~/our_clone_project $ git push
    Everything up-to-date

發現沒有任何反應，這是因為這個new_feature branch是我們剛剛建立的，remote的repository (good_project)，並不知道你有這個branch。加入new_feature在push 的指令後：

    ~/our_clone_project $ git pust new_feature
    fatal: ‘new_feature’ does not appear to be a git repository
    fatal: The remote end hung up unexpectedly

還是噴錯，原因是因為git push後面第一個參數必須是remote repository 的name，如果什麼都不加，預設是使用origin。

    ~/our_clone_project $ git push origin new_feature

    (push成功，切回good_project，會發現多了new_feature的branch)
    ~/good_project $ git branch
    ~/good_project $ git checkout new_feature
    ~/good_project $ git log
    (這邊會看到我們剛剛針對file2的commit的紀錄)

---------------


這時候我們在切回我們原本的專案good_project，試著對對file2做一些修改


    ~/good_project $ nano file2
    編輯file2中的檔案
    原本：
    This is file2, you guys!!

    修改成：
    This is file2, you guys!! Edited by good_project

    (接著再把code commit)
    ~/good_project $ git commit –a –m “Edit file2”

    ~/good_project $ git checkout master

以上我們模擬good_project對file2做了一些修改，並且commit。假設現在另一方，也就是our_clone_project也對file2做了修改：

    ~/our_clone_project $ nano file2

編輯file2中的檔案
    原本：
    This is file2, you guys!!

    修改成：
    This is file2, you guys!! Edited by our_clone_project

    (接著再把code commit)
    ~/good_project $ git commit –a –m “Edit file2”

接著一樣，我們一樣要對remote repository做sync（push）：

    ~/our_clone_project $ git push

這時候會發生錯誤了！原因很簡單，因為remote repository (good_project)剛剛已經有先修改過file2了，因此git會發現你要修改的這個file2並不是最新的，所以會把你檔下來。這時候就要先把另一端的修改pull下來

    ~/our_clone_project $ git pull origin new_feature

還是噴錯，因為git發現在our_clone_project的file2跟要pull下來file2都已經有修改紀錄，兩邊不sync。基本上git會幫你自動作merge，但是有時候必須手動（舉例來說，兩邊都針對同一個檔案的同一行做修改）。所以皆下來的動作就是自己手動解決merge的問題：

    ~/our_clone_project $ nano file2

    （修改檔案，手動merge）

接著在push之前別忘了還是要先commit：

    ~/our_clone_project $ git add file2
    ~/our_clone_project $ git commit


在push就成功嚕！

    ~/our_clone_project $ git push

    (push 成功，成功將程式碼發佈到good_project)


接下來我們來看看另一端good_project。假設今天在good_project中我們修改了file1的檔案：
    ~/good_project $ nano file1

    (修改file1檔案後commit change)

    ~/good_project $ git commit –a –m “change file1 content”

修改完後當然要和remote端做sync

    ~/good_project $ git push

錯誤發生了，這個錯誤之前也遇過。其實原因就是git push 預設是使用origin當作remote repository 的名稱，但是現在我們的remote repository並非origin，而是我們自己命名的our_clone，所以加上名稱就成功了！

    ~/good_project $ git push our_clone






