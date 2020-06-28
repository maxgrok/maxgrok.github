---
template: post
title: Differences in Solidity
slug: eighty-first-post
draft: false
date: 2019-02-04T13:49:00.000Z
description: This is a post going over the differences between various elements
  of Solidity that are commonly mistaken such as mappings vs. arrays, visibility
  options, and interfaces vs. contracts.
category: Solidity
tags:
  - Blockchain
  - Decentralized
  - Ethereum
---

In this post, I will be going over the differences between various elements of Solidity that are commonly mistaken such as mappings vs. arrays, visibility options, and interfaces vs. contracts.

## Mappings vs. Arrays

Mappings are like arrays, except they cannot be directly iterated through. 

Mappings are key-value pairs used for storage. They cannot be iterated through without an external helper variable. Keys cannot be structs, mappings, dynamically-sized arrays, contracts, or enums. The key data is not actually stored in a mapping, only its keccak256 hash can be used to look up a value. 

Arrays can be dynamic or fixed-sized and store data in Solidity. These can be iterated through without an issue. 

Dynamic arrays can be resized by using the `.length` member in storage. Also, `.push` may be used to add elements to the dynamic arrays, whereas for fixed-sized arrays you can use the bracket notation to access values and edit them. For example: `dynamicArray.push(1) // adds 1 to the dynamic array` or `fixedsizedArray[1] = 3 // adds 3 to the index of 1 in the fixed sized array`.

## Visibility: Public, Private, External, Internal

In Solidity, there are four visibility keywords: external, public, private, and internal. <br>

<strong>External:</strong> <br>
- Can be called from other contracts and by transactions <br>
- Cannot be called internally<br>
- useful for large arrays of data && may be more efficient<br>

<strong>Public:</strong><br>
- so long as it is apart of the contract interface, can be called by message calls and internally<br>
- in the case of public state variables, they receive an automatic getter function <br>

<strong>Internal:</strong><br>
- only accessible internally (from within the contract, without using <em>this</em>)<br>

<strong>Private:</strong><br>
- only visible for the contract that they are declared in<br>
- not available for even derived contracts<br>

(source: [Solidity Visibility](https://www.bitdegree.org/learn/solidity-visibility-and-getters)) 

## Interfaces Vs. Contracts Vs. Base Contracts

<em>Interfaces</em> are like contracts except that they cannot hold any state variables and only carry function signatures (rather than implementations of functions). 

<em>Contracts</em>, on the other hand, have the implementation of functions and state variables. 

<em>Base Contracts</em> have the implementation of certain functions, state variables, and lack specific implementation of all functions required for the smart contract. This is used in the "Auction Contract" lesson of Blockgeeks in Ethereum 101 to create a sort of framework for basic functions used for auction contracts. 

Sources: 

[Solidity Visibility](https://www.bitdegree.org/learn/solidity-visibility-and-getters)<br>
[CBDE-Ethereum Udemy Course](https://www.udemy.com/ethereum-blockchain-certification/learn/v4/t/lecture/10172644?start=0)<br>
