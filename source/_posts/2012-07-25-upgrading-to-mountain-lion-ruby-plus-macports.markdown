---
layout: post
title: "Upgrading to Mountain Lion - Ruby + MacPorts"
date: 2012-07-25 23:25
comments: true
categories: macports rvm osx ruby
---

My colleague Kenny Johnston wrote a nice piece about [Upgrading to Mountain Lion](http://coderwall.com/p/dtbuqg) with an emphasis on using Homebrew. Here are the relevant steps using MacPorts:


## XCode:

* Update to the latest XCode 4.4 from the App Store
* Install the Command Line Tools

Of course, since it is zero-day, there was an issue fetching the Command Line Tools from the server and I had to install manually from a `.pkg` file retrieved from the Apple Developer Portal. If you install the CLI tools package manually, you may need to run the following:

    sudo xcodebuild -license

That is required to accept the license terms system-wide.


## MacPorts

First, you'll need to update the base MacPorts installation:

    sudo port -v selfupdate

If you run in to a compilation error, you may need to edit the file `/opt/local/etc/macports/macports.conf` and set the `developer_dir` option to be empty:

    sudo vim /opt/local/etc/macports/macports.conf

```bash /opt/local/etc/macports/macports.conf
# Directory containing Xcode Tools (default is to ask xcode-select)
#developer_dir       /Developer
developer_dir
```

You should now be able to update any outdated ports (grab a beverage while you wait):

    sudo port -vu upgrade outdated


## GCC 4.2

Now install the `apple-gcc42` port to replace the missing GCC 4.2 in Mountain Lion:

    sudo port install -vu apple-gcc42

Once that has been installed, you will need to link it to the expected system location:

    sudo ln -s /opt/local/bin/gcc-apple-4.2 /usr/bin/gcc-4.2


## RVM

Make sure you have the latest RVM version installed:

    curl -L https://get.rvm.io | bash -s stable

You should now ensure that you can rebuild your installed Ruby versions:

    rvm reinstall 1.9.3
    rvm reinstall rbx
    ...

You may need to reference my earlier tip: [Fix Ruby 1.9.x OpenSSL Segfault on OS X](http://phlippers.net/blog/2012/07/10/fix-ruby-1-dot-9-x-openssl-segfault-on-os-x/)
