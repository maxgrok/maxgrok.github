---
template: post
title: How to Write Doctests in Elixir
slug: ninety-third-post
draft: false
date: 2019-04-25T11:30:00.000Z
description: This is a post is about how to make doctests in Elixir.
category: Elixir
tags:
  - Elxiir
  - Doctest
---

If you want to make doctests in Elixir, then it's really simple. This is going to be a short post, assuming you already have Elixir installed and have started your own project running `mix new projectname`.  

In your .exs file, add this very specific syntax to your code in the form of a comment. 

```elixir
@doc """

## Examples 

            iex>1+1 #insert 3 tabs
            2 
    #make sure to have a new line here
"""
```

This will add whatever example you list to your doctests automatically. 

So, when you run `mix test`, it will run those test files to see if that is the result of your input. 

In this case, 1+1 calculation equals 2. So, the result would be 2. This test would pass. 

Try and run it! 

Use the `mix test` command in your CLI. 

You can write more doc tests by adding another segment of @doc plus the quotes for comment syntax and the exact syntax in the above example duplicated with a different example! 

Congratulations! You now know how to write doctests in Elixir projects! 