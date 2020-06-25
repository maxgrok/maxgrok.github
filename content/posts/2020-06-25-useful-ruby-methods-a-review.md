---
template: post
title: "Useful Ruby Methods: A Review"
slug: sixty-first-post
draft: false
date: 2018-11-02T17:03:00.000Z
description: This is a post about useful ruby methods.
category: Technical
tags:
  - Technical
  - Ruby
  - Methods
---

In this post, I will be going over useful ruby methods to be familiar with as a beginner. 

These methods include: 
`.each`, `.map`, & `.select` 

Let's get started. 

`.each` is an iterator method in Ruby. It is used to perform a specific action over each member of a hash or an array. 

Here is an example of `.each` in action! 
```
    array = [0,1,2,3]
    def add_one(array)
        return array.each do |el|
          return el + 1
        end
    end

    add_one(array)
    ---> [0,1,2,3]
```

It does not alter the original array. 

For more details, please see these posts: [Ruby Iterators][https://www.tutorialspoint.com/ruby/ruby_iterators.htm] 

`.map` is an enumerable method in Ruby. 

Here is an example of `.map` in action: 

```
array =[0,1,2,3]
 def map(array)
    return array.map { |i| i+1 }
 end
map(array)

--> [1,2,3,4]
```

This alters each element of the array and returns a new array with the alterations. 

For more details, see [Enumerables in Ruby][https://ruby-doc.org/core-2.1.0/Enumerable.html] 

`.select` is an enumerable method in Ruby. 

Here is an example of `.select` in action: 
```
array = [0,1,2,3]
def select(array)
    array.select { |i| i.even? }
    end

select(array)
--> [0,2]

```

`.select` returns a new array with the elements that meet the condition specified. 

For more details, see [Enumerables in Ruby][https://ruby-doc.org/core-2.1.0/Enumerable.html]

All these methods are used frequently in Ruby. It is good to familiarize oneself with many different uses of them for your own programs!

For more information on iterators and enumerables, visit [Enumerables in Ruby][https://ruby-doc.org/core-2.1.0/Enumerable.html] & [Ruby Iterators][https://www.tutorialspoint.com/ruby/ruby_iterators.htm] 