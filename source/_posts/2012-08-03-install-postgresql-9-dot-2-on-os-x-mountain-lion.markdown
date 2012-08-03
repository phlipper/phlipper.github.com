---
layout: post
title: "Install PostgreSQL 9.2 on OS X Mountain Lion"
date: 2012-08-03 13:08
comments: true
categories: osx postgresql macports
---

The [PostgreSQL](http://www.postgresql.org) database keeps getting better with each release. With 9.2 now at beta2, it's a great time to jump in and get up to speed.


## Prerequisites

This guide assumes the following:

* OS X Mountain Lion
* The latest XCode
* XCode Command Line Tools
* The latest [MacPorts](http://www.macports.org)


## Installation

Make sure MacPorts is up to date, then install the `postgresql92-server` port:

    $ sudo port -v selfupdate
    $ sudo port -v install postgresql92-server


## Setup the Initial Database

Once the server has been installed, you will need to create the initial database:

    $ sudo mkdir -p /opt/local/var/db/postgresql92/defaultdb
    $ sudo chown postgres:postgres /opt/local/var/db/postgresql92/defaultdb
    $ sudo su postgres -c '/opt/local/lib/postgresql92/bin/initdb -D /opt/local/var/db/postgresql92/defaultdb'


## Starting the Server

Once the initial database has been created, you can start the server.

To run the server manually, execute the following command:

    $ sudo su postgres -c '/opt/local/lib/postgresql92/bin/postgres -D /opt/local/var/db/postgresql92/defaultdb'

If you want the server to automatically start at boot time, execute the following command:

    $ sudo launchctl load -w /Library/LaunchDaemons/org.macports.postgresql92-server.plist


## Setup your PATH

The executable files for PostgreSQL are in a non-standard location, so you you'll want to update your `PATH` to make things easier. Most likely, you'll want to edit your `~/.bashrc` or `~/.zshrc` (or similar) profile, though you can set apply the changes for all users system-wide by editing `/etc/profile`.

Make sure you set the new path _before_ `/usr/bin` to ensure that you are using the latest versions and not the default apple-supplied tools:

```bash
export PATH=/opt/local/lib/postgresql92/bin:$PATH
```

You can verify this works by running `which psql` and you should see `/opt/local/lib/postgresql92/bin/psql` as the output.


## Create a New User

You will be setup with a `postgres` user by default, but it is good practice to create a different user account.

To make things easy, create a new database user to match your OS X username:

    $ createuser --superuser <my username> -U postgres

You should now be able to create a new database:

    $ createdb my_app


Do please consider setting a password for your newly-created user ;)


## Configuring a Rails Application

To setup your Rails application with PostgreSQL, you will need to do the following:

* Add the `pg` gem to your `Gemfile` and run the `bundle` command

```ruby Gemfile
source :rubygems

gem "rails"
gem "pg"
# ...
```

* Configure your `database.yml` to use PostgreSQL:

```yaml config/database.yml
development:
  adapter: postgresql
  encoding: unicode
  host: localhost
  database: myapp_development
  username: <%= ENV["USER"] %>
  password:
  allow_concurrency: true
  pool: 5
  min_messages: warning

test:
  adapter: postgresql
  encoding: unicode
  host: localhost
  database: myapp_test
  username: <%= ENV["USER"] %>
  password:
  allow_concurrency: true
  pool: 5
  min_messages: error
```

Even if you have set the `min_messages` option, you may still see console output like the following:

    WARNING:  there is already a transaction in progress

Edit the file `/opt/local/var/db/postgresql92/defaultdb/postgresql.conf` and set the following:

    client_min_messages = error

Everything should now be running smoothly.
