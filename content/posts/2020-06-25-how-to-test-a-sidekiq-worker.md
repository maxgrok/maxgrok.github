---
template: post
title: How to Test a Sidekiq Worker
slug: ninety-nineth-post
draft: false
date: 2019-06-04T13:30:00.000Z
description: This is a post about how to test a Sidekiq worker with Rails and rspec-sidekiq.
category: Sidekiq
tags:
  - Rspec-sidekiq
  - Sidekiq
  - Rails
---

## Introduction 
 
Sidekiq runs background jobs for Rails apps. Officially, Sidekiq is "simple, efficient background processing for Ruby"(<a href="https://github.com/mperham/sidekiq">Sidekiq</a>).<br/>

In a Rails app, I was working on I was tasked with building a Sidekiq worker that sent an in-app notification and an email in the future to users for a To Do list. <br/>

After spending some time Googling, I've decided to help others who may Google for how to test a Sidekiq worker, especially at testing a worker at a specific time, by writing about how to do it, in one blog post.<br/>

We are going to use Sidekiq and also rspec-sidekiq to do testing. I will go over configuration for `rspec-sidekiq`, but not Sidekiq (as this is well documented).<br/>

## Configure Your Rails App for 'rspec-sidekiq'

Run `gem install rspec-sidekiq` within your project root directory. 

Then, in `rails_helper.rb`, include the following:  

```ruby
require 'sidekiq/testing/inline'
require 'sidekiq-status/testing/inline'

RSpec::Sidekiq.configure do |config|
  # Clears all job queues before each example
  config.clear_all_enqueued_jobs = true # default => true

  # Whether to use terminal colours when outputting messages
  config.enable_terminal_colours = true # default => true

  # Warn when jobs are not enqueued to Redis but to a job array
  config.warn_when_jobs_not_processed_by_sidekiq = true # default => true
end
```

## Writing Your Tests 

Name your RSpec file in `spec/workers/worker_name_here_spec.rb` by convention, then 
add the following at the top of your RSpec file: 

```ruby 
require 'rails_helper' 
require 'sidekiq/testing'
Sidekiq::Testing.fake!
``` 

Now, let's see some examples of tests you might like to run on your Sidekiq worker. 

### For testing the queue

```ruby
it "job in correct queue" do 
  described_class.perform_async
  assert_equal :queuenamehere, described_class.queue
end
```

### For testing the timing of the task

```ruby
let(:time) { Time.zone.today + 6.hours).to_datetime } # define at the top of your rspec file
let(:scheduled_job) { described_class.perform_in(time, 'Awesome', true) } # define in the top of your rspec file

it 'occurs at expected time' do #define within a describe block
  scheduled_job
  assert_equal true, described_class.jobs.last['jid'].include?(scheduled_job)
  expect(described_class).to have_enqueued_sidekiq_job('Awesome', true)
end
```

## Full Example Test

```ruby
require 'rails_helper' # include in your RSpec file
require 'sidekiq/testing' #include in your Rspec file
Sidekiq::Testing.fake! #include in your RSpec file

RSpec.describe ActionItemWorker, type: :worker do
  let(:time) { (Time.zone.today + 6.hours).to_datetime }
  let(:scheduled_job) { described_class.perform_at(time, 'Awesome', true) }

  describe 'testing worker' do
    it 'ActionItemWorker jobs are enqueued in the scheduled queue' do
      described_class.perform_async
      assert_equal :scheduled, described_class.queue
    end

    it 'goes into the jobs array for testing environment' do
      expect do
        described_class.perform_async
      end.to change(described_class.jobs, :size).by(1)
      described_class.new.perform
    end

    context 'occurs daily' do
      it 'occurs at expected time' do
        scheduled_job
        assert_equal true, described_class.jobs.last['jid'].include?(scheduled_job)
        expect(described_class).to have_enqueued_sidekiq_job('Awesome', true)
      end
    end
  end
end

```

## Running Your Tests

Run individual tests like this from your terminal: `rspec path/to/file`

You can also run tests manually by running: `rails c` , then `require 'sidekiq/testing'`. Your console should return `true`, after running `require sidekiq/testing`. 

Then, you can run your `WorkerNameHere.new.perform` to run your worker inline. Depending on your worker, you will get back a variety of responses. Expect whatever you are anticipating it to return here to be returned in the console. 

Alternatively, you can also run `WorkerNameHere.perform_in(5.seconds.from_now)` and the worker will perform in the future (exactly 5 seconds from now)! This will return a "jid" such as "9ef9392c92aee2540caf46ae". So, when you check your jobs array, created by the test environment, it should include your job. 

To test your Sidekiq Worker jobs array, run `WorkerNameHere.jobs` in terminal and see if it contains your job with your jid. If it does, then it was enqueued in Sidekiq to be run as a job. 

Here is an example of the jobs array:
```ruby
[{"class"=>"Worker",
  "args"=>[],
  "at"=>1559661046.951212,
  "retry"=>false,
  "queue"=>"scheduled",
  "jid"=>"9ef9392c92aee2540caf46ae",
  "created_at"=>1559661041.951413}]
```

<br/>

## Further Reading

For further reading on testing Sidekiq Workers in RSpec, please follow these links: <br/>
<a href="https://github.com/mperham/sidekiq">Sidekiq Docs</a><br/>
<a href="https://github.com/mperham/sidekiq/wiki/Testing">Sidekiq Testing Docs</a><br/>
<a href="https://github.com/philostler/rspec-sidekiq">rspec-sidekiq Docs</a><br/>