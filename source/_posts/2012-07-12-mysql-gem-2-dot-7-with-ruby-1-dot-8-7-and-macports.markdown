---
layout: post
title: "MySQL gem 2.7 with Ruby 1.8.7 and MacPorts"
date: 2012-07-12 13:37
comments: true
categories: ruby mysql macports osx
---

I recently had to reinstall the `mysql` gem version 2.7 in to following environment:

* Ruby 1.8.7
* OS X 10.7
* MacPorts MySQL 5

I needed to look up this incantation for the Nth time so I am recording it here:

```bash gem install
env ARCHFLAGS="-arch x86_64" gem install mysql -v 2.7 -- --with-mysql-config=/opt/local/lib/mysql5/bin/mysql_config
```
