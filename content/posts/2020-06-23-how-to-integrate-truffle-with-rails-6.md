---
template: post
title: How to Integrate Truffle with Rails 6
slug: one-hundred-and-eleventh-post
draft: false
date: 2020-01-26T21:00:00.000Z
description: "In this post, I wrote about how to integrate Truffle into a Rails
  6 application. "
category: Ethereum
tags:
  - Solidity
  - Truffle
  - Rails
---
Here is a short post on how to setup your Rails 6 app to work with Truffle.

<em>Pre-requisites</em>: Truffle project with Solidity smart contracts all ready to be deployed. Truffle already installed. Infura project ID. Metamask. Rinkeby testnet Ether or mainnet Ether. Rails 6 app already generated. 

Run `truffle init` within the same root directory as the Rails app. 

Run `truffle compile`.

Setup your `truffle-config.js` with the following: 

```js
const fs = require('fs');
const mnemonic = fs.readFileSync(".secret").toString().trim();
const HDWalletProvider = require('truffle-hdwallet-provider');
const infuraKey = "118951e57cf64dc5a614119f6a85ae10";

const infuraRinkebyURL = `https://rinkeby.infura.io/v3/${infuraKey}`;
const infuraMainnetURL = `https://mainnet.infura.io/v3/${infuraKey}`;

const provider = new HDWalletProvider(mnemonic,infuraRinkebyURL);
const mainNet = new HDWalletProvider(mnemonic,infuraMainnetURL);

module.exports = {
  networks: {
    development: {
     host: "127.0.0.1",     // Localhost (default: none)
     port: 8545,            // Standard Ethereum port (default: none)
     network_id: "*",       // Any network (default: none)
    },
    rinkeby:{
      provider: () => provider, 
      network_id: 4,
      gas: 5500000
    },
    mainnet:{
      provider: () => mainNet,
      network_id: 1, 
      gas: 5500000
    }
  }
}
```

Run `truffle migrate --network rinkeby` to deploy your contract to the testnet or `truffle migrate --network mainnet` to deploy your contract to the mainnet.  

Use the `gem install ethereum.rb` to install the `ethereum.rb` gem. This gem provides an Ethereum ruby library to access the Ethereum blockchain asap.

Use these gems to make RPC calls and interact with contracts on Ethereum through Rails. 

## Congratulations! 

If all went well, you have Truffle configured to work with your Rails app. 
