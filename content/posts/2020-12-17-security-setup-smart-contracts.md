---
template: post
title: Setup a Smart Contract Security Auditing Local Environment in OS X
slug: security-audit-setup-explained
draft: true
date: 2020-12-17T09:39:47.348Z
description: "Want to run basic security auditing tools on your smart contracts? Here's what you need to know about how to setup your local environment to test the contracts."
category: Smart Contract Security
tags:
  - Smart Contract
  - Ethereum
  - Security
---

Requirements: Latest OS X (at the time of writing this: Big Sur), npm, node, python 3.9, ganache-cli, truffle. 

We'll be installing MythX and Cryptic's Slither: 2 well-known tools for static analysis auditing of smart contracts. 

There are two ways to use MythX. One is with the remix plugin, which we will not be going over, and the other is by installing it locally, which we will cover. 

So, let's get started! 
Open your shell and run the following: 
```pip install mythx-cli```

This will install the Mythx CLI. After this, you will be able to run ```mythx``` and see a bunch of helpful commands. 

![mythx-help-commands](https://imgur.com/859ovlt.png)



