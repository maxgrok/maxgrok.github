---
template: post
title: How to Setup RSpec for a Rails App
slug: ninety-fourth-post
draft: false
date: 2019-05-03T13:22:00.000Z
description: This is a short post is about how to setup testing with RSpec and
  basic RSpec commands for a Rails application.
category: RSpec
tags:
  - RSpec
  - TDD
  - Testing
---

After setting up your new rails app, add 'rspec-rails' to your Gemfile, then rebundle. 

Generate your rspec files with the command `rails g rspec:install` in your terminal. This will give you all the setup testing files that you need to get started with testing your Rails application. 

Write your tests in your spec/ file, then run `rspec` in your command line. This will run all of your tests in the spec/ folder. 

## File Organization for RSpec

Convention says that feature tests go into a features folder, controller tests in a controller/ folder, models in a models/ folder, etc. Naming conventions for test files are to always end them in `_spec.rb`. 

## Naming Conventions in RSpec

If you create a controller spec file, name it `thisismycontroller_controller_spec.rb` and place it into a controllers folder within spec, whereas for models just name it `goal_spec.rb` within the models folder within spec. The same goes for features, except you name a feature close to what it does instead of any other name. For example, writing an associations feature, like I recently did, I called the feature test `associating_X_with_Y_spec.rb` within the features/folder. 

## Run Specific Tests with RSpec

If you want to run specific tests in the spec folder, simply run `rspec path/to/your/test`. 

## Congratulations

You are up and running with RSpec in your Rails application!

## More Information on RSpec

[RSpec Documentation](http://rspec.info/documentation/) <br/>
[RSpec Getting Started Documentation](https://relishapp.com/rspec/rspec-rails/docs/gettingstarted)

