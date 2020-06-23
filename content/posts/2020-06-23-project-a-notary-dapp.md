---
template: post
title: "Project: A Notary DApp"
slug: one-hundred-and-thirteenth-post
draft: false
date: 2020-02-04T13:40:00.000Z
description: "In this post, I explain the final project I built for Consensys
  Academy Blockchain Bootcamp 2020. "
category: Consensys Academy
tags:
  - Solidity
  - Truffle
  - Consensys Academy
  - Netlify
---
This is a brief post about my final project for Consensys Academy Blockchain Developer Bootcamp 2019-2020.

## What did I build? 

I built a Notary/Proof of Existence DApp that works on Rinkeby and another app that works on Ganache. I will be discussing the Rinkeby version. 

## Demo? 

App Demo: http://bit.ly/36DWMbW <br/><em>Requires: MetaMask & Internet Connection & Rinkeby ETH</em>

Rinkeby contract deployed: https://rinkeby.etherscan.io/address/0x5b0c51dDA0263bBffEb3093D7F664305FB075e08/  

<a href="https://youtu.be/fNtoz6l-YF8">!['notary-dapp-rinkeby'](https://media.giphy.com/media/f6JmPFaArTS71zduv5/giphy.gif)</a>

## What does this project do? 

It allows users to upload a file to IPFS and notarize it with their MetaMask Rinkeby Ethereum Account and post it on Rinkeby Testnet with the simple click of a single deploy button. Complete with toast messages for user status updates. 

## How do I set it up locally? 

### Pre-Requisites

<em>Required:</em> MetaMask, Node.js, Npm/yarn, Internet Connection (for Contract Deployment and IPFS Upload)<br/>
<em>Optional:</em> Truffle (only for debugging and deploying smart contracts included)

### 1st
Update .secret file with your Mnemonic for your MetaMask account and set your MetaMask extension to custom network `rinkeby network`. <em>Make sure you have test ETH in your Rinkeby Account</em>. If not, get some from a Rinkeby faucet. 

### 2nd
Install dependencies with `npm i` and start the app from `client/` directory with `npm start`

### 3rd
Navigate to `localhost:3000` and upload and notarize documents with one click of a button!
 
#### Credits
Made with <3 from <a href="https://github.com/ConsenSys/rimble-app-demo">Rimble App Demo</a>.

## Enjoy and notarize and upload your documents to IPFS! 

<a href="https://github.com/maxgrok/notary-Dapp-rinkeby">Github Link to Notary-DApp Rinkeby edition</a>



