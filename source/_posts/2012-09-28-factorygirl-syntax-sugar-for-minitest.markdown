---
layout: post
title: "FactoryGirl syntax sugar for MiniTest"
date: 2012-09-28 00:52
comments: true
categories: ruby minitest factory_girl
---

`minitest` is a great testing library and `factory_girl` is a handy fixture replace library, especially when working with Rails.

It can get tedious to repeat `FactoryGirl.` for every invocation of `build`, `create`, etcetera. Here is a handy shortcut to help reduce your Carpal Tunnel pain:

```ruby
# for Test::Unit assertion style
class MiniTest::Rails::ActiveSupport::TestCase
  include FactoryGirl::Syntax::Methods
end

# for Spec expectation style
class MiniTest::Spec
  include FactoryGirl::Syntax::Methods
end
```

Place this in a file such as `test/support/factory_girl.rb`.

This allows you to use the core set of syntax methods (`build`, `build_stubbed`, `create`, `attributes_for`, and their `*_list` counterparts) without having to call them on `FactoryGirl` directly:

```ruby
describe Awesome do
  # do this
  subject { create(:awesome) }

  # don't do this
  subject { FactoryGirl.create(:awesome) }
end
```
