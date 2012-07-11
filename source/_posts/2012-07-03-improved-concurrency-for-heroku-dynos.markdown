---
layout: post
title: "Improved concurrency for Heroku Dynos"
date: 2012-07-03 10:34
comments: false
categories: puma unicorn thin heroku ruby
---

A Rack application running on [Heroku](http://heroku.com) can improve performance by using different application servers with different performance characteristics.

Some good choices include [Thin](http://code.macournoyer.com/thin/) (evented I/O), [Unicorn](http://unicorn.bogomips.org/) (multi-process) and [Puma](http://puma.io/) (multi-threaded).

The exact configuration settings will vary by application and how much memory each app takes. A default Rails app with a basic scaffold can easily support 4 unicorn workers or 4 puma threads. A small Sinatra app can support several times that number.

Here are the `Procfile` settings you can use for each:

```ruby Procfile
# thin
web: bundle exec thin -p $PORT -e $RACK_ENV start

# puma
web: bundle exec puma -t 1:4 -b tcp://0.0.0.0:$PORT

# unicorn
web: bundle exec unicorn -p $PORT -c ./config/unicorn.rb
```

Use the [New Relic RPM Add-on](https://addons.heroku.com/newrelic) to monitor your application resources.
