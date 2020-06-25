---
template: post
title: Crypto Hash Functions, Miners, Nodes, and the Anatomy of a Block
slug: seventy-sixth-post
draft: false
date: 2018-11-29T01:00:00.000Z
description: This is a post is an introduction to the basics of a block,
  cryptographic hashing, nodes, and mining.
category: Blockchain
tags:
  - Blockchain
  - Anatomy of a Block
  - P2P
---

This post will go over the very basics of blockchain, namely cryptographic hash functions, the anatomy of a single block, the blockchain, nodes (full and light), and what miners do in a blockchain. 

## Cryptographic Hash Functions

Hashing functions take in a data of any size or format and return a specific hash address that is either 128 bits or 256 bits depending on the encryption used (i.e. SHA-128 or SHA-256). 

Here is how cryptographic hashing functions work for Ethereum: 

![cryptohashether](./hashing.jpg)


## Anatomy of a Block

A block is made up of a previous hash address, data unique to that block, nonce, it's own hash address, timestamp, unique number a.k.a height. 

It might be visually represented like this: 

![block](./block.png) 

(Source: <a href="https://www.coursera.org/learn/blockchain-foundations-and-use-cases/lecture/MEq7s/lesson-4-anatomy-of-a-block">Anatomy of a Block</a>)

The previous block's hash address is included and links it all the way back to the genesis block. 

The unique number, a.k.a. height, of a block is unique to that block. 

The timestamp is for the time when the block was created. 

The nonce is an arbitrary number that miners can edit in order to make the cryptographic hash less than the "difficulty" threshold. 

"Difficulty" refers to an "arbitrary setting, managed by the [blockchain]protocol, which changes how hard it is to mine a block" (Source: <a href="https://www.coursera.org/learn/blockchain-foundations-and-use-cases/lecture/MEq7s/lesson-4-anatomy-of-a-block">Anatomy of a Block</a>)

Miners change the nonce of a block changes the resulting hash address until it is valid. 

A block is considered valid when the hash address of the block is less than the difficulty threshold defined by the blockchain protocol. 

Miners are rewarded for this validation work financially in the cryptocurrency in that protocol. For example, mining a block on Bitcoin results in Bitcoin paid to that miner. 

This is work of the proof of work protocols in blockchain networks. 

## The Chain

Each block that is valid has a link all the way back to the genesis of the blockchain that it is a part of. 

If one piece of information is edited, it must be re-mined and re-validated. This is what makes the blockchain immutable or very difficult to edit. 

## Nodes

There are two types of nodes in a peer-to-peer blockchain network: full nodes and light nodes. 

<strong> Full nodes</strong> store the entire blockchain from the genesis up until the most recent transaction and validate every single transaction and block. 

<strong>Light nodes</strong> store only the most recent transactions in teh blockchain. They verify blocks and transactions as well. 

## Miners

Miners are nodes that are either full or light nodes that also do mining, the process by which one edits the nonce of a block until it is below the difficulty threshold and considered valid, then added to the blockchain in exchange for a financial reward delivered to the miner. Once a miner finds a valid block, they include it in their own blockchain and send it to other nodes as well. 

## Conclusion

Hash functions are used to cryptographically store the data in each block, which is composed of the previous hashes address, it's own hash address, a nonce, a timestamp, data unique to that block, and a unique height. These blocks combined to make the blockchain in their respective networks governed by different protocols such as proof of work, which miners solve increasingly hard mathematical problems in order to validate a block. 

We also went over what type of nodes there are in a peer-to-peer network: full and light. Full nodes store the entire blockchain, while light nodes store the most recent transactions in the blockchain. 

Congratulations! Now, you know the basics of blockchain and what miners do to contribute to the ecosystem. 

## Sources:
-<a href="https://www.coursera.org/learn/blockchain-foundations-and-use-cases/lecture/BUttm/lesson-2-cryptographic-hash-functions">Cryptographic Hash Functions - Coursera - Consensys Academy</a><br>
-<a href="https://www.coursera.org/learn/blockchain-foundations-and-use-cases/lecture/MEq7s/lesson-4-anatomy-of-a-block">Anatomy of a Block - Coursera - Consensys Academy</a><br>

