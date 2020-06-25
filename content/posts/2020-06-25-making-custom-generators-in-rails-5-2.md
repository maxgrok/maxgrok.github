---
template: post
title: Making Custom Generators in Rails 5.2
slug: seventy-second-post
draft: false
date: 2018-11-26T12:20:00.000Z
description: This is a post about how to make custom generators in your Rails 5.2.1 app.
category: Rails
tags:
  - Rails
  - Generators
  - Ruby
---

First, let's go ahead and assume you have an app already made, but you haven't done `rails g scaffold` yet. We will be editing the `rails g scaffold` generator. 

## Editing the Generator Preferences 

First, let's go to our `config/application.rb` file. 
You should see something like this:

```ruby
    require_relative 'boot'

require "rails"
# Pick the frameworks you want:
require "active_model/railtie"
require "active_job/railtie"
require "active_record/railtie"
require "active_storage/engine"
require "action_controller/railtie"
require "action_mailer/railtie"
require "action_view/railtie"
require "action_cable/engine"
require "sprockets/railtie"
# require "rails/test_unit/railtie"

# Require the gems listed in Gemfile, including any gems
# you've limited to :test, :development, or :production.
Bundler.require(*Rails.groups)

module GeneratorApp
  class Application < Rails::Application
    # Initialize configuration defaults for originally generated Rails version.
    
    # Settings in config/environments/* take precedence over those specified here.
    # Application configuration can go into files in config/initializers
    # -- all .rb files in that directory are automatically loaded after loading
    # the framework and any gems in your application.

    # Don't generate system test files.
    config.generators.system_tests = nil
  end
end

```

Let's go ahead and add some code to make our preferences known to the app. 
Copy the following code: 

```ruby
require_relative 'boot'

require "rails"
# Pick the frameworks you want:
require "active_model/railtie"
require "active_job/railtie"
require "active_record/railtie"
require "active_storage/engine"
require "action_controller/railtie"
require "action_mailer/railtie"
require "action_view/railtie"
require "action_cable/engine"
require "sprockets/railtie"
# require "rails/test_unit/railtie"

# Require the gems listed in Gemfile, including any gems
# you've limited to :test, :development, or :production.
Bundler.require(*Rails.groups)

module GeneratorApp
  class Application < Rails::Application
    # Initialize configuration defaults for originally generated Rails version.
    config.generators do |g|
        g.orm :active_record
        g.template_engine :erb
        g.test_framework :test_unit, fixture: false
        g.stylesheets false
        g.javascripts false
    end

    # Settings in config/environments/* take precedence over those specified here.
    # Application configuration can go into files in config/initializers
    # -- all .rb files in that directory are automatically loaded after loading
    # the framework and any gems in your application.

    # Don't generate system test files.
    config.generators.system_tests = nil
  end
end

```

Alright, we have created our preferences for what to use when generating the ORM, templating engine, test framework (which you can change to anything, hint: rspec), and stated not to make the stylesheets (no SCSS files) or javascript. 

## Editing Content of a Generator

We've edited the preferences of our generator, however we can edit the content of what is generated as well. Let's go ahead and edit an `index.html.erb` file. 

Let's go into our `lib/` folder in our app directory. You should see `assets/` and `tasks/` as folders with nothing in them. Leave them like that. 

Add a nested directory within `lib/` called `templates/`, which is our templating, then create a folder within `templates/` called `erb/`, then within `erb/` create another folder called `scaffold`. 

We will be editing the `rails g scaffold` process. 

Create a file within `scaffold` called `index.html.erb`
Then go to <a href="https://github.com/rails/rails/blob/master/railties/lib/rails/generators/erb/scaffold/templates/index.html.erb.tt">here</a> to find the usual code from Rails for the `index.html.erb` file. 

We will be editing this file directly. Copy and paste it into the `index.html.erb`. 

You should see the following code: 
```ruby
<p id="notice"><%%= notice %></p>

<h1><%= plural_table_name.titleize %></h1>

<table>
  <thead>
    <tr>
<% attributes.reject(&:password_digest?).each do |attribute| -%>
      <th><%= attribute.human_name %></th>
<% end -%>
      <th colspan="3"></th>
    </tr>
  </thead>

  <tbody>
    <%% @<%= plural_table_name %>.each do |<%= singular_table_name %>| %>
      <tr>
<% attributes.reject(&:password_digest?).each do |attribute| -%>
        <td><%%= <%= singular_table_name %>.<%= attribute.column_name %> %></td>
<% end -%>
        <td><%%= link_to 'Show', <%= model_resource_name %> %></td>
        <td><%%= link_to 'Edit', edit_<%= singular_route_name %>_path(<%= singular_table_name %>) %></td>
        <td><%%= link_to 'Destroy', <%= model_resource_name %>, method: :delete, data: { confirm: 'Are you sure?' } %></td>
      </tr>
    <%% end %>
  </tbody>
</table>

<br>

<%%= link_to 'New <%= singular_table_name.titleize %>', new_<%= singular_route_name %>_path %>
```

Let's make some edits. 

```ruby
<p id="notice"><%%= notice %></p>

<h2><%= plural_table_name.titleize %></h2>
<hr>

<div>
    <%% @<%= plural_table_name %>.each do |<%= singular_table_name %>| %>
    <div>
        <% attributes.reject(&:password_digest?).each do |attribute| -%>
        <p><%%= <%= singular_table_name %>.<%= attribute.column_name %> %></p>
        <% end -%>
        <p><%%= link_to 'Show', <%= model_resource_name %> %></p>
        <p><%%= link_to 'Edit', edit_<%= singular_route_name %>_path(<%= singular_table_name %>) %></p>
        <p><%%= link_to 'Destroy', <%= model_resource_name %>, method: :delete, data: { confirm: 'Are you sure?' } %></p>
    </div>
    <%% end %>
</div>

<br>

<%%= link_to 'New <%= singular_table_name.titleize %>', new_<%= singular_route_name %>_path %>
```

Instead of a table, we replaced them with div's and instead of an h1 for the table name, we have an h2. Now, you can insert whatever you want into this file as long as it's ERB and HTML5, it will run. 

Now, we've created our own custom generator! 

We're ready to run `rails g scaffold`! 

## Conclusion

Congratulations! You've created your first custom generator in Rails 5.2!

For further information on generating your own generators, please see this [Rails Guide][https://guides.rubyonrails.org/generators.html]

