---
template: post
title: How to Send Mail in the Future with Rails
slug: ninety-seventh-post
draft: false
date: 2018-05-20T15:30:00.000Z
description: This is a post about how to write a new mailer and get it running
  with a Sidekiq worker to send in the future.
category: Rails
tags:
  - Rails
  - ActionMailer
  - ActionMailer::Base
  - Sidekiq
---

This post assumes you already have setup Sidekiq, Sidetiq, Rails 5.1.2, and are familiar with the command line. 

Let's run `rails generate mailer InsertMailerName` in the command line in the root directory of your Rails app. 

This will create the following output in your terminal (minus the 'identical' files, it will just say created then the file name): 

![mailer](screenshot.png)



```ruby
# app/mailers/insert_name_mailer.rb
class InsertNameMailer < ActionMailer::Base
  default from: "from@example.com"

  def welcome_email
    mail(to: "to@example.com", subject: "Your Future Email")
  end
end
```

Create a Mailer View by making a new file called `app/views/insert_name_mailer/welcome_email.html.haml` (or just .html if you do not have a .haml linter setup). 

Inside that `welcome_email.html.haml` file, place the following code: 

```html
<!DOCTYPE html>
<html>
  <head>
    <meta content='text/html; charset=UTF-8' http-equiv='Content-Type' />
  </head>
  <body>
    <h1>Welcome to example.com!</h1>
    <p>
      You have successfully signed up to example.com.<br>
    </p>
    <p>
      To login to the site, just follow this link:<a href="http://www.example.com/">example.com</a>.
    </p>
    <p>Thanks for joining and have a great day!</p>
  </body>
</html>
```

Now, we can make a sidekiq worker that runs daily with `Sidetiq` and `Sidekiq`. 

Let's make a new worker by running `rails g sidekiq:worker NameOfWorker`. 

You will see the following files created: 

![worker-creation](screenshot(1).png)

Go to `app/workers/name_of_worker_worker.rb`

You will see the following: 
```ruby
class NameOfWorkerWorker
  include Sidekiq::Worker

  def perform(*args)
    # Do something
  end
end
```

Let's add the mailer in the perform method. 

```ruby
class NameOfWorkerWorker
  include Sidekiq::Worker

  def perform(*args)
    InsertNameMailer.welcome_email.deliver # calls the mailer we made's method and delivers the email immediately
  end
end
```

Now, whenever the worker runs, it will send a welcome email. 

Let's test this out. 

Go to `rails c test` to load your test environment. 

Inside the rails console, run `NameOfWorker.new.perform`. 

If successful, you will see a load of text followed by an output that is green and looks like this: 

```ruby
=> #<Mail::Message:LongNumberHere>, Multipart: true, Headers: <Date: TodaysDateHere>, <From: "from@example.com">, <To: "to@example.com">, <Message-ID: <RandomStringOfLettersAndNumbersHere@YourComputer'sName>>,<Subject: "Your Future Email">, <Mime-Version: 1.0>, <Content-Type: multipart/alternative; boundary="--==mimepart_(randonstringoflettersandnumbers)"; charset=UTF-8>, <Content-Transfer-Encoding: 7bit>>
```

Now, let's add the scheduling to the Sidekiq Worker with one line thanks to Sidetiq!

```ruby
class NameOfWorkerWorker
  include Sidekiq::Worker
  include Sidetiq::Schedulable # new line includes the Schedulable module from Sidetiq

  recurrence { daily } # makes the worker run daily

  def perform(*args)
    InsertNameMailer.welcome_email.deliver # calls the mailer we made's method and delivers the email immediately
  end
end
```

Congratulations your Sidekiq Worker now sends email daily! 
Where ever you call your Sidekiq Worker, you will send an email daily in the future.

<h2>Resources: </h2> <br>

<a href="https://guides.rubyonrails.org/action_mailer_basics.html">Ruby on Rails Guides: ActionMailer</a><br>

<a href="https://github.com/mperham/sidekiq/wiki/Getting-Started">Getting Started Sidekiq </a><br>

