---
layout: post
title: "Fix Ruby 1.9.x OpenSSL Segfault on OS X"
date: 2012-07-10 22:37
comments: true
categories: macports rbenv openssl homebrew rvm ruby
---

If you install Ruby 1.9.2 or 1.9.3 on OS X, you may run in to the dreaded OpenSSL segfault:

    ~/ruby-1.9.3-p194/lib/ruby/1.9.1/net/http.rb:799: [BUG] Segmentation fault
    ruby 1.9.3p194 (2012-04-20 revision 35410) [x86_64-darwin11.4.0]

The version of OpenSSL that ships with OS X 10.6 and 10.7 is older and causes this issue.

There are a number of options solve this. Here a few:

**MacPorts**

    $ port selfupdate
    $ port install openssl
    $ port install libyaml

    # RVM
    $ rvm install 1.9.3 --with-openssl-dir=/opt/local --with-opt-dir=/opt/local

    # rbenv
    $ CONFIGURE_OPTS='--with-openssl-dir=/opt/local --with-opt-dir=/opt/local' rbenv install 1.9.3-p194


**Homebrew**

    $ brew update
    $ brew install openssl

    # RVM
    $ rvm install 1.9.3 --with-openssl-dir=`brew --prefix openssl`

    # rbenv
    $ CONFIGURE_OPTS="--with-openssl-dir=`brew --prefix openssl`" rbenv install 1.9.3-p194

**RVM**

    $ rvm pkg install openssl
    $ rvm reinstall 1.9.3 --with-openssl-dir=$rvm_path/usr
