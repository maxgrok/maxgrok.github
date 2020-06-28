---
template: post
title: Deployment Issues with Rails 6
slug: eighty-seventh-post
draft: false
date: 2019-03-07T17:00:00.000Z
description: This is a post complications that may arise while deploying to
  Heroku with Rails 6 & Ruby 2.6.0 as well as how to fix them.
category: Rails
tags:
  - Rails 6
  - Ruby 2.6.1
  - Heroku
  - Deployment
---

Recently, I was building a yelp clone and deploying it to Heroku. I was using Ruby 2.6.0 and Rails 6 to build the application and came across some difficulties. 

Some errors you may get while deploying to Heroku using my dependencies and the `rails new` command. 

(1) Heroku does not support sqlite3 

Fix this with the right db/config file for postgresql and removing 'sqlite3' from your Gemfile and adding 'gem 'pg'' to your Gemfile. 

Replace your current config file with the following for example: 

```ruby
development:
  adapter: postgresql
  database: my_database_development
  pool: 5
  timeout: 5000
test:
  adapter: postgresql
  database: my_database_test
  pool: 5
  timeout: 5000

production:
  adapter: postgresql
  database: my_database_production
  pool: 5
  timeout: 5000
```

(2) Bundler 2 is required for Heroku to run the build. 

Fix: Upgrade Ruby version from 2.6.0 => 2.6.1
 This is done to fix the Bundler 2 install error from Heroku on attempting git push heroku master. 

(3) For any resource that does not exist error for your database in your heroku logs --tail 

Fix: run `heroku run rake db:migrate`. 

Congratulations! Your app is now deployed to Heroku!

More Resources: 

<a href="https://devcenter.heroku.com/articles/bundler-version#app-not-using-the-currently-supported-bundler-version">Heroku Bundler 2 issues</a>

<a href="https://devcenter.heroku.com/articles/sqlite3">Heroku SQLite3 issues</a>