---
template: post
title: Reverse Engineer Your Public Key from Your Private Key in Python
slug: public-key-from-private-key
draft: false
date: 2020-10-27T15:13:47.348Z
description: "This is a post about how to get your public key from your private key with a python package called eth-keys."
category: Crypto
tags:
- ETH
- Eth-Keys
---

go to the command line: 
brew install pyenv
pyenv install 3.7.3
pyenv global 3.7.3

[Eth-Keys](https://pypi.org/project/eth-keys/)

Restart your bash shell. 

from eth_keys import KeyAPI
from eth_keys.backend import NativeECCBackend
keys = KeyAPI(NativeECCBackend)

DAO 

- function like a multisig
- number of shares? 

loot tokens -- in the DAO 
- shareholder who has governance
- loot holder -- has access to the bank
- funding in exchange for DSCVR? 

LootHolders --> Funders could put money, exit at anytime, but can't make any votes or sponsor proposals. 