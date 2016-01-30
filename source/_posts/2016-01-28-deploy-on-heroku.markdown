---
layout: post
title: "Deploy on Heroku"
date: 2016-01-28 00:06:21 +0800
comments: true
categories: Heroku
---

Deploy到Heroku基本上會有下面的步驟：

    $ heroku login
    $ heroku create happy-octopress

    $ git add .
    $ git commit -am "make it better"
    $ git push heroku master

而在使用上面的指令之前，先加入下面的gems:

    group :production do
      gem 'pg'
      gem 'rails_12factor'
    end

-----------

PostgreSQL錯誤訊息：

    An error occurred while installing pg (0.18.4), and Bundler cannot continue.
    Make sure that `gem install pg -v '0.18.4'` succeeds before bundling.

需先安裝postgresql:

    $ brew uninstall postgresql
    $ brew install postgresql
    $ gem install pg
-----------

在本機端使用postgre遇到錯誤訊息：

    psql: could not connect to server: No such file or directory
    Is the server running locally and accepting
    connections on Unix domain socket "/tmp/.s.PGSQL.5432"?

需啟動postgres(參考 http://stackoverflow.com/questions/13410686/postgres-could-not-connect-to-server) :

    $ pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start
