---
template: post
title: How to Deploy Your Smart Contract to Production
slug: one-hundred-and-eighth-post
draft: false
date: 2019-11-10T16:29:00.000Z
description: This is a post about how to deploy your Solidity smart contract to
  the mainnet via Infura and Buddy CI and Github.
category: Ethereum
tags:
  - Solidity
  - Buddy
  - Mainnet
---

In this post, I will teach you how to deploy your Solidity smart contract to the mainnet via Infura with BuddyCI and Github. 

Prior requirements: 
  - knowledge of javascript, git, solidity, CLI <br/>
  - have a <a href="https://www.github.com/">github account, <a href="www.infura.io">infura</a> account, and <a href="https://buddy.works/">buddy CI </a> account. <br/>
  - smart contracts already written, audited, and ready for deployment<br/>
      - here's a sample of a <a href="https://github.com/maxgrok/SimpleStorage/">smart contract</a> that you can deploy. <br/>

## Select Github Repo

![buddy-ci(1)](./buddy-ci(1).png)<br/>

![buddy-ci(2)](./buddy-ci(2).png)<br/>

## Add a Pipeline to Buddy CI
![buddy-ci(3)](./buddy-ci(3).png)<br/>

Execute it "On Push". 

![buddy-ci(4)](./buddy-ci(4).png)<br/>

Name it "SimpleStorage Deploy to Production"

Pick "Single Branch" and "Master"

## Install Truffle-HDWallet-Provider

Run `npm install truffle-hdwallet-provider` on your local computer. <br/>

## Edit your truffle-config.js file on your computer

Copy and paste the following into your truffle-config.js file, replacing the <project_id> with your Infura project id. <br/>

```javascript
const path = require("path");
const HDWalletProvider = require("truffle-hdwallet-provider");

const infuraURL = "https://mainnet.infura.io/v3/<project_id>";
const infuraRinkebyURL = "https://rinkeby.infura.io/v3/<project_id>";
const mnemonic = process.env.PRIV_SEED;

module.exports = {
  // See <http://truffleframework.com/docs/advanced/configuration>
  // to customize your Truffle configuration!
  contracts_build_directory: path.join(__dirname, "client/src/contracts"),
  networks: {
    develop: {
      host: "127.0.0.1",
      port: 7545,
      network_id: "*"
    },
    "rinkeby":{
      provider: ()=> new HDWalletProvider(mnemonic, infuraRinkebyURL),
      network_id: 4
    },
    "mainnet":{
      provider: () => new HDWalletProvider(mnemonic, infuraURL),
      network_id: 1
    }
  }
};

```

## Grab your mnemonic seed phrase! 

Get your mnemonic seed phrase from your Metamask account by selecting Settings and Security then enter your password to gain access to your seed phrase and add it as an environment variable in Buddy CI pipeline called "PRIV_SEED". Save it there. 

## Grab Test Ether from Rinkeby Faucet 

If you do not have Rinkeby test ether in your account, then now is a good time to get some here at the <a href="https://faucet.rinkeby.io/">Rinkeby Faucet</a> by following the instructions there. 

## Add the install truffle-hdwallet-provder script

Add `npm install truffle-hdwallet-provider` to your environment on the Buddy CI pipeline you created below the `npm i -g truffle` script. 

## Rinkeby or Mainnet

Decide if you want to deploy your smart contract to the testnet (Rinkeby in this case) or mainnet (warning: costs real ETH). 

Depending on what you decide, you want to configure your pipeline action to run the following: 
### Mainnet Command

`truffle migrate --network mainnet`

### Rinkeby Command

`truffle migrate --network rinkeby`

## Make a Github Push to Deploy to Production

run `git init` within your project directory to start a git <br/>
run `git add .` to add the changes in your truffle-config.js file to your staging area. <br/>
run `git commit -m 'changing truffle-config.js to include mainnet and rinkeby'`<br/>
run `git remote add origin <your github repo address here>`<br/>
run `git push origin master`<br/>

This will push all the files and history up to your github repo, which will automaticaly trigger the Buddy CI to run. 

## Check the logs

Check the logs on your Buddy CI pipeline by logging into your account and selecting your project, if you don't already have it open in another window. 

## Success! 

![buddy-ci(success)](./buddy-ci(success).png)

Hopefully, you have succeeded in deploying your contract to the mainnet or rinkeby. It's your choice. Depending on what you choose, you will see some variation of the above and it will take a variable amount of time to complete the mining of the transaction/contract on the network (ethereum mainnet takes approximately ~12 seconds per block). 

## Etherscan 

Check your contract deployed by visiting Etherscan.io and copy pasting your Rinkeby or Mainnet account address from Metamask. 

You should see the contract creation transaction hash and a timestamp for when it was migrated to the Rinkeby or Mainnet. 

![buddy-ci(ether)](./buddy-ci(etherscan).png)

Enter in your Rinkeby wallet address into the search bar. 

![buddy-ci(etherscan)](./buddy-ci(etherscan)(2).png)

Check out the transactions on the account. 

![buddy-ci(etherscan)(3)](./buddy-ci(etherscan)(3).png)

You can check the Rinkeby deployment of this contract by going to <a href="https://rinkeby.etherscan.io/address/0xA951C2baa16caC2ce1128f6b608F13c164923e37"> rinkeby.etherscan.io</a>

## Extra Credit

Explore Buddy CI and see how to send yourself a notification email on production deploy or to setup an approval requirement for admins to execute deploy. 




