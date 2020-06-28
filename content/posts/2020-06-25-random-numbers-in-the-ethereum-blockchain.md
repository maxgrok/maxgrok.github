---
template: post
title: Random Numbers in the Ethereum Blockchain
slug: eightieth-post
draft: false
date: 2019-01-23T19:49:00.000Z
description: This is a post going over the solutions to the true randomness
  number generator problem in Ethereum blockchain.
category: Blockchain
tags:
  - Blockchain
  - Decentralized
  - Ethereum
---

In Ethereum, there is no clear, true random number generator. 

One might think to use `block.timestamp`, but this is not an actual true random number because it can be manipulated by the miner to a certain extent, as can all metadata. 

Three solutions exist to this problem now:
(1) Bitcoin Block Headers
(2) Hash-Commit-Reveal
(3) RANDAO 

Let's explain each of these solutions. 

(1) Bitcoin Block Headers are - 80-byte headers 'belonging to a single block which is hashed repeatedly to create proof-of-work' (Source: Bitcoin Block Header).  

(2) Hash-Commit-Reveal is a scheme that generates a true random number. It involves the first person creating a random number, hashing that number, then the hash is posted, then at a later point in time, the person reveals the number behind the hash so it can be verified by all parties. containing a timestamp that stores it's hash rather than the actual timestamp, then commits the hash to the blockchain, which is possible to be revealed to the miner after completing a proof-of-work problem. 

(3) RANDAO is a random decentralized organization smart contract that uses all participants to ultimately generate a random number. 

Which is the best solution? 

The RANDAO smart contract is what is recommended, as it is the most secure and reliable method of generating a true random number. 

Sources: 

[Bitcoin Block Header](https://bitcoin.org/en/glossary/block-header)<br>
[Udemy Blockchain Course. Lecture 61.](https://www.udemy.com/ethereum-blockchain-certification/learn/v4/t/lecture/10078990?start=0)

