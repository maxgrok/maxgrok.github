---
template: post
title: How do I setup my Ethereum Dev Environment in OS X Mojave?
slug: seventyith-post
draft: true
date: 2018-11-24T17:13:00.000Z
description: This is a post about setting up your development environment for
  Ethereum with ganache, truffle, solc, web3, and more!
category: Ethereum
tags:
  - Smart Contracts
  - Blockchain
  - P2P
  - Ethereum
---

This is a quick post. All you need to do is follow the commands below, then you're all set!

In your root ~ directory, in the terminal, type the following: 

## Npm
to install from scracth type 
`npm install install`

`npm i -g npm` to update npm

## Ganache-CLI
`npm install -g ganache-cli`

## Truffle
`npm install -g truffle`

## web3
`npm install -g web3@0.20.0`

## solc

`npm install -g solc`

## git 
[Getting Started Installing Git][https://git-scm.com/book/en/v2/Getting-Started-Installing-Git]

## Homebrew
Run in the terminal: `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

[More Brew info][https://brew.sh/]

## nvm 
After homebrew is installed, then run:
`brew install nvm`

Do not forget to follow the instructions and edit your .bash_profile to include an export link from $NVM_DIR to the prescribed place. Read your terminal after installation for more instruction from Homebrew. 

## Node.JS 

`nvm ls-remote` to list available node.js versions
use `nvm install v10.13.0` for the latest stable version
`nvm use v10.13.0` - to designate this version for use
`node -v` command should return the current version of node being used is `v10.13.0`

## Sublime Text
[Download][https://www.sublimetext.com/]

If you use Sublime Text, download the [Ethereum linter][https://github.com/davidhq/SublimeEthereum], and [EthereumSnippets][https://github.com/chevdor/EthereumSoliditySnippets/blob/master/README.adoc] through package control. 

Otherwise, google around for linters for Solidity for your own code editor. 

## Conclusion

Now, you're all setup for your first smart contract!