---
template: post
title: "Github 2FA: Personal Access Tokens"
slug: /posts/one-hundred-and-seventeenth-post
draft: false
date: 2020-04-01T04:00:00.000Z
description: Having trouble making commits with 2FA authorized on your Github?
  Here's how to commit without trouble.
category: Github
tags:
  - Github
  - 2FA
  - Git
---
First, make a personal access token in Github by going to Github > Settings > Developer Settings > Personal access tokens > Generate New Token. 

Second, in the Generate New Token screen, select all the checkboxes. You want complete access for your Github project or whatever access you need. 

Third, copy and paste your personal token as a password in your authentication for your Github account in your terminal. 

For example, when it says login username, enter your username, then for the password instead of your password, enter the personal access token. 

Finally, commit away! Now, you should be all setup with your HTTP personal access token to make commits moving forward with your project! 

