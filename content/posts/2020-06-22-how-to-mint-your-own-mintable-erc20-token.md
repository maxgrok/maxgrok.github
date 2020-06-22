---
template: post
title: How to Mint Your Own Mintable ERC20 Token
slug: one-hundred-and-sixteenth-post
draft: false
date: 2020-02-13T19:00:00.000Z
description: Learn how to mint your own mintable ERC20 token with Solidity,
  Truffle, Infura, and MetaMask with Rinkeby Test Ether.
category: "Ethereum "
tags:
  - ERC20
  - Token
  - ETH
---
<em>
I just graduated from Consensys Academy Blockchain Developer Bootcamp and am actively searching for a full time tech position, preferrably Full Stack Dapp, or Smart Contract Engineering, but flexible for awesome teams. 

</em><Br/>
<strong>TLDR;</strong> <br/>I minted an ERC20 using OpenZeppelin Contracts, Ownership, and Access roles. Please note this uses the 2.5 version of the OpenZeppelin contracts. 
Copy paste the following code: </em>


```js
 pragma solidity ^0.5.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/token/ERC20/ERC20Detailed.sol";

import "@openzeppelin/contracts/ownership/Ownable.sol";
import "@openzeppelin/contracts/access/roles/MinterRole.sol";

contract MAX is ERC20, ERC20Detailed, MinterRole, Ownable {
    /**
     * @dev See {ERC20-_mint}.
     *
     * Requirements:
     *
     * - the caller must have the {MinterRole}.
     */
     constructor() ERC20Detailed("Max", "MAX", 18) public {
        _mint(msg.sender, 5000000000000000000 );
    }

    function mint(address account, uint256 amount) public onlyMinter onlyOwner returns (bool) {
        _mint(account, amount);
        return true;
    }
}
```

Run it like a usual contract and deploy to rinkeby with your truffle-config.js looking like this; 

```js
const mnemonic = "yourMnemonicHere";

const HDWalletProvider = require('truffle-hdwallet-provider');
const infuraKey = "yourProjectKey"; 

const infuraRinkebyURL = `https://rinkeby.infura.io/v3/${infuraKey}`;
const infuraMainnetURL = `https://mainnet.infura.io/v3/${infuraKey}`;

const provider = new HDWalletProvider(mnemonic,infuraRinkebyURL);

const mainNet = new HDWalletProvider(mnemonic,infuraMainnetURL)

module.exports = {
  networks: {
    development: {
     host: "127.0.0.1",     // Localhost (default: none)
     port: 8545,            // Standard Ethereum port (default: none)
     network_id: "*",       // Any network (default: none)
    },
    
    rinkeby:{
      provider: () => provider, 
      network_id: 4, //rinkeby network_id 
      gas: 5500000
    },

    mainnet:{
      provider: () => mainNet,
      network_id: 1,  // mainnet network_id
      gas: 5500000
    }
  },

  // Set default mocha options here, use special reporters etc.
  mocha: {
    // timeout: 100000
  },

  // Configure your compilers
  compilers: {
    solc: {
    }
  }
}
```

## The Full Steps for Curious Devs

Run `truffle init` in an empty directory to initiate your truffle project. 

Run `npm install @openzeppelin/contracts` to install the OpenZeppelin contracts in your truffle project. 

Next, import the appropriate @openzeppelin contracts into your smart contract by adding the following to the top of  your smart contract after your pragma statement. 

```js
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/token/ERC20/ERC20Detailed.sol";

import "@openzeppelin/contracts/ownership/Ownable.sol";
import "@openzeppelin/contracts/access/roles/MinterRole.sol";
```

Next, name your contract and inherit the libraries into your contract. 

```js
contract MAX is ERC20, ERC20Detailed, MinterRole, Ownable {

}
```

Next, we'll create the constructor for your ERC20 token. 

```js
constructor() ERC20Detailed("Max", "MAX", 18) public {

    }
```

The first property in the ERC20Detailed is your name of the token, the second is your ticker (only three letters), and 18 decimal points for the tokens. We are making this public as a function. 

Now, if you want to make this mintable, you'll need to add the _mint function. 

```js
 function mint(address account, uint256 amount) public onlyMinter onlyOwner returns (bool) {
        _mint(account, amount);
        return true;
    }
```

Then we need to call this function within our constructor. 
Add this inside your constructor. 

```js
  _mint(msg.sender, 5000000000000000000 ); // remember 18 decimals for your token ...this mints 5 $MAX's
```

## Deployment and Truffle Config File

Make sure your truffle-config.js file looks like this: 

```js
const mnemonic = "yourMnemonicHere";

const HDWalletProvider = require('truffle-hdwallet-provider');
const infuraKey = "yourProjectKey"; 

const infuraRinkebyURL = `https://rinkeby.infura.io/v3/${infuraKey}`;
const infuraMainnetURL = `https://mainnet.infura.io/v3/${infuraKey}`;

const provider = new HDWalletProvider(mnemonic,infuraRinkebyURL);

const mainNet = new HDWalletProvider(mnemonic,infuraMainnetURL)

module.exports = {
  networks: {
    development: {
     host: "127.0.0.1",     // Localhost (default: none)
     port: 8545,            // Standard Ethereum port (default: none)
     network_id: "*",       // Any network (default: none)
    },
    
    rinkeby:{
      provider: () => provider, 
      network_id: 4, //rinkeby network_id 
      gas: 5500000
    },

    mainnet:{
      provider: () => mainNet,
      network_id: 1,  // mainnet network_id
      gas: 5500000
    }
  },

  // Set default mocha options here, use special reporters etc.
  mocha: {
    // timeout: 100000
  },

  // Configure your compilers
  compilers: {
    solc: {
    }
  }
}
```

Include your mnemonic and infura project id where appropriate for deployment via `truffle migrate`. 

So, let's go ahead and deploy 5 newly minted $MAX's! 

Before attempting the following, make sure you have test ETH in your Rinkeby address that corresponds to the mnemonic you provided. In your CLI, in the root directory, run `truffle compile && truffle migrate --network rinkeby`. This will deploy your contract to the testnet. You can then take that contract address and check it on rinkeby.etherscan.io to make sure it successfully validated and confirmed on the Rinkeby network. 

<strong>Please note deploying to the mainnet costs ETH (Read: Money). </strong>

If you want to deploy to the mainnet, then just change your network deployed to `mainnet`. 

## Congratulations! 

You've minted your very own mintable ERC20 token! 

## Connect!

Drop me a line, if you're going to ETH Denver, say hi, especially if you're hiring!
