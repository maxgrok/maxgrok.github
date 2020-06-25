---
template: post
title: Make Your First Blockchain Trade
slug: fifty-sixth-post
draft: true
date: 2020-06-25T14:08:20.670Z
description: This is a post about how to make your first blockchain trade using
  Docker, nvm, Node, Yeoman, XCode, IBM Hyperledger Composer, and more.
category: Flatiron School
tags:
  - Flatiron School
  - Immersive
  - Coding School
---


Today, I'm going to teach you how to setup your blockchain dev environment and make your first blockchain trade on a locally hosted business network that we will create from scratch. All in all, this should take about an hour or two. So, let's get started. 

If you are new to the blockchain, here is a resources about what blockchain is: <a href="https://developer.ibm.com/dwblog/2017/what-is-blockchain-hyperledger-fabric-distributed-ledger/">What is Blockchain? </a>. Read this first before trying to build anything. 

Second, this post is going to be a bit of a long post and assumes that you already know how to use the command line, have a recent mac os x operating, and have basic knowledge of javascript. 

I'm breaking this up into three steps. The first step is setting up the configuration on your computer, then the second step is creating your own business network, and the third step is making your first trade.    

<h2>Step One: Installing and Configuring your Developer Environment</h2>

1a) Install Docker. 

Go to <a href="https://www.docker.com/">Docker</a>. Setup a free account. Download Docker for Mac OS X. Setup Docker. Start running docker in the background after install. 

1b) Install nvm. 

```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash
```

1c) Update or create your `.bash_profile`  in the root directory of your computer. 

1d) Rerun the command in 1b)

1e) Check to see that nvm was installed by running this command. 

Run `nvm --version` in your console. 
It should return 0.33.11, which is the latest version as of this blog post. 

1f) Install node. 

Run `nvm install --lts` in your console. 

1g) Switch to using the latest node. 

Run `nvm use --lts` in your console. 

1h) `node --version` to check that node is using the latest version. 
It should be version 8.11.3 as of this blog post. 

1i) Install Visual Studio Code 

Go to `https://code.visualstudio.com` and download Visual Studio Code. 

1j) Install the Visual Studio Extension for Hyperledger Composer

Go to the startup screen of Visual Studio, press the "Extensions" button. Search for "Hyperledger Composer". Click Install. Once it is installed, you need to click the Reload button, which will appear where the Install button was before. 

1k) Install the Composer CLI tool. 

Run `npm install -g composer-cli` in your console. 

1l) Install the utility for building and running a REST server to expose your business networks as APIs 

Run `npm install -g composer-rest-server` in your console. 

1m) Install utility for generating application assets

Run `npm install -g generator-hyperledger-composer` in your console.

1n) Install Yeoman which uses hyperledger-composer to generate applications:

Run `npm install -g yo` in your console. 

1o) Install the Composer Playground. 

Run `npm install -g composer-playground` in your console. 

1p) In your root directory, make the directory where you will store your Hyperledger Fabric runtime to deploy your business networks to. Run the following commands in order. 

    1p1) `mkdir ~/fabric-dev-servers && cd ~/fabric-dev-servers`

    1p2) `curl -O https://raw.githubusercontent.com/hyperledger/composer-tools/master/packages/fabric-dev-servers/fabric-dev-servers.tar.gz`

    1p3) `tar -xvf fabric-dev-servers.tar.gz`

    1p4) `cd ~/fabric-dev-servers`

    1p5) `./downloadFabric.sh`

<h2>Step Two: Creating and Deploying Your Business Network</h2>

1) Let's enter your directory of fabric development servers. 

Run `cd ~/fabric-dev-servers` in your console. Then run `pwd` to check to make sure you are in the right directory. 

2) Let's start a fabric! 

Run `./startFabric.sh` in your console. 

3) Let's make a PeerAdminCard. 

Run `./createPeerAdminCard.sh` in your console. 

4) Let's get started with someone we can actually work on. 

Run `composer-playground` in your console. 
You should see in your web browser a screen with a GUI that has several options. You should also see your peerAdminCard. There is an option to develop your business network using only the GUI. We will not be following that track. We will be following the developer track. If you would prefer the GUI, then please follow this tutorial called the <a href="https://hyperledger.github.io/composer/latest/tutorials/playground-tutorial.html">Playground Tutorial</a>. 

5) To create the scaffolding for your skeleton/empty business network, run `yo hyperledger-composer:businessnetwork` in your console. 
   5a) Enter `tutorial-network` as the name
   5b) Enter project description
   5c) Enter name of person developing the network (i.e. you)
   5d) Enter your email for this project 
   5e) Select Apache-2.0 for the license
   5f) Select org.example.mynetwork
   5g) Select 'No' when asked if you want to develop an empty business network. 

6) If you have sublime text and the command line utility installed, run `subl .` in your project directory named 'tutorial network'. 

7) Open your `org.example.mynetwork.cto` file, which is our Model file. 

8) Replace it's contents with the following: 

```
/**
 * My commodity trading network
 */
namespace org.example.mynetwork
asset Commodity identified by tradingSymbol {
    o String tradingSymbol
    o String description
    o String mainExchange
    o Double quantity
    --> Trader owner
}
participant Trader identified by tradeId {
    o String tradeId
    o String firstName
    o String lastName
}
transaction Trade {
    --> Commodity commodity
    --> Trader newOwner
} 
```

9) Open up your `index.js` file within your `lib` folder within the project. 

10) Replace it's contents with the following and save the file afterwards: 

```
/**
 * Track the trade of a commodity from one trader to another
 * @param {org.example.mynetwork.Trade} trade - the trade to be processed
 * @transaction
 */
async function tradeCommodity(trade) {
    trade.commodity.owner = trade.newOwner;
    let assetRegistry = await getAssetRegistry('org.example.mynetwork.Commodity');
    await assetRegistry.update(trade.commodity);
}
```

11) Open `permissions.acl` in your text editor. 

12) Replace the contents with the following: 

```
/**
 * Access control rules for tutorial-network
 */
rule Default {
    description: "Allow all participants access to all resources"
    participant: "ANY"
    operation: ALL
    resource: "org.example.mynetwork.*"
    action: ALLOW
}

rule SystemACL {
  description:  "System ACL to permit all access"
  participant: "ANY"
  operation: ALL
  resource: "org.hyperledger.composer.system.**"
  action: ALLOW
}
```

13) Generate the business network archive or BNA file within the `tutorial-network` folder. 

Run `composer archive create -t dir -n .` (do not forget the . at the end of the command) in your console. 

14) Install the business network within the `tutorial-network` directory. 

Run `composer network install --card PeerAdmin@hlfv1 --archiveFile tutorial-network@0.0.1.bna` in your console. 

15) Start the business network using the following command. 

Run `composer network start --networkName tutorial-network --networkVersion 0.0.1 --networkAdmin admin --networkAdminEnrollSecret adminpw --card PeerAdmin@hlfv1 --file networkadmin.card` in your console. 

16) Import the network admin identity as the usable business network card by running the following command: 

Run `composer card import --file networkadmin.card` in your console. 

17) Check that it has been deployed successfully with the following command: 

Run `composer network ping --card admin@tutorial-network` in your console. 

You should see a response or an error. If an error, listen to the error and debug. 

Your successful response should look like this: 

```markdown

The connection to the network was successfully tested: tutorial-network-generated
    Business network version: 0.0.1
    Composer runtime version: 0.19.11
    participant: org.hyperledger.composer.system.NetworkAdmin#admin
    identity: org.hyperledger.composer.system.Identity#somenumbersandletters

Command succeeded
```

18) Let's generate a REST server. 

Run `composer-rest-server` within the directory of your BNA / `tutorial-network` in your console. 
    18a) Enter `admin@tutorial-network` for your card name
    18b) Select 'never use namespaces' when asked about generating namespaces in the generated API
    18c) Select 'no' to the request for securing the generated API
    18d) Select 'no' to the request with the API
    18e) Select 'yes' when asked about to enable event publication 
    18f) Select 'no' when asked to implement TLS security.

19) Let's generate an application! 
 Run `yo hyperledger-composer:angular` in your console. 
 19a) Select 'yes' when asked to connect to existing business network 
 19b) Enter your project name, description, author name, author email for `package.json` file. 
 19c) Enter 'admin@tutorial-network' for your admin card name 
 19d) Select 'Connect to existing API'
 19e) Enter `http://localhost` for your REST server address
 19f) Enter `3000` for your port
 19g) Select 'namespaces are not used'

20) Navigate to the directory named 'tutorial-network' within 'tutorial-network', i.e. the directory without the .bna in it. 

21) Run the following to install dependencies of the app. 
    `npm install` in your console, while inside the 'tutorial-network' directory where the `package.json` file was generated. 

22) Run `npm start` within the same directory ('tutorial-network'). 

23) Open your web browser and navigate to `http://localhost:4200` to see your interface for your newly created business network. 

<h2>Congratulations! You Deployed And Created Your First Blockchain Business Network!</h2>

<h2>Step Three: Making Your First Trade</h2>

1) Navigate to Participants > Traders in your API. 

2) Click create Participants. 

3) Create Two Participants with unique id numbers

4) Navigate to Asset. Create an asset. 

5) Go to your REST API on `localhost:3000/explorer`

6) Navigate to Trade, click show/hide. 

7) Click on  `/Trade` next to `POST`. 
    You should see a set of example JSON for a successful trade. 
    You should also see parameters for you to try it out. 

    Copy and paste the following code into the value textarea: 

```javascript
    {
  "$class": "org.example.mynetwork.Trade",
  "commodity": "resource:org.example.mynetwork.Commodity#ABC",
  "newOwner": "resource:org.example.mynetwork.Trader#TRADER2"
}
```

Replace TRADER2 with the name of your trader. (if there are spaces, separate them by adding %20 instead of a space). 
Replace the commodity name with your own commodity name after your resource:org.example.mynetwork.Commodity. 
Keep the $class as it is. 


8) Click on Try it out!
    If you get a 'Response Code' of 200, then your trade has been successfully completed!

    You can now go back to your Angular front end and check the commodity's owner. It should represent the new owner that you have requested in your JSON. 

<h2>Congratulations! You just completed your first trade on your very own blockchain network!</h2>

If you'd like to learn more about blockchain, then take a look at these resources below. 

<h2>Resources</h2>

<a href="https://developer.ibm.com/courses/all/blockchain-essentials/?course=begin#8629">Blockchain Essentials Course</a>

<a href="https://hyperledger.github.io/composer/latest/applications/web.html">Writing Web Applications - Hyperledger Composer</a>

<a href="https://hyperledger.github.io/composer/latest/tutorials/developer-tutorial">Developer Tutorial - Hyperledger Composer</a>

<a href="https://stackoverflow.com/questions/44617968/error-while-deploying-bna-file-to-local-hyperledger-fabric">BNA deployment error - StackOverflow</a>

<a href="https://solidity.readthedocs.io/en/develop/introduction-to-smart-contracts.html#simple-smart-contract">Introduction to Smart Contracts - Solidity Docs</a>

<h2>Sources used in this tutorial</h2>

<a href="https://hyperledger.github.io/composer/latest/installing/development-tools.html">Installing Development Tools - Hyperledger Composer</a><br>

<a href="https://hyperledger.github.io/composer/latest/installing/installing-prereqs.html">Installing Pre-requisites for Hyperledger Composer</a><br>

<a href="http://www.duckduckgo.com">DuckDuckGo</a><br>
<a href="http://www.stackoverflow.com/">StackOverflow</a><br>
