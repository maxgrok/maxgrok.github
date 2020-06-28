---
template: post
title: Write Your First Smart Contract
slug: sixty-fifth-post
draft: true
date: 2018-11-09T19:49:00.000Z
description: This is a post about how to write and execute your first smart
  contract using Solidity and Remix IDE.
category: "Blockchain "
tags:
  - Smart Contract
  - Blockchain
  - P2P
---
Pre-requisite knowledge: Javascript ES6. General knowledge of OOP.  

This is a post that will walk you through how to write and execute your first smart contract using Solidity and the Remix IDE. 

First, we're going to introduce ourselves to the Remix IDE found [here][http://remix.ethereum.org/].

Second, we're going to write our first smart contract in Solidity. 

Third, we're going to execute our first smart contract using the Remix IDE. 

## Remix IDE: An Introduction

The Remix IDE is a developer environment built to deploy smart contracts on the Ethereum network at your leisure. 

If you haven't already, go to the [Remix IDE here][http://remix.ethereum.org/].

As you can see, there is a section for the files in our dev environment on the left. Some boilerplate code that we will get into soon in the middle. To the right, there are a variety of options in a menubar such as `Compile`, `Run`, `Analysis`, and so on. 

Go to `Run`, find the `Javascript VM` in the `Environment` setting and select it. This will give you four accounts with 100 ether each in them that you can see in the `Account` selections.  

For this next task, we are going to focus on the middle section where the code is written. 

## Write Your First Smart Contract
In this contract, we will send one message from the owner of the message to another owner. 

Click the + sign on the far left hand corner in Remix IDE. Let's name our first contract `myfirstcontract.sol`. 

Type out the following in the middle section of the screen: 
```
pragma solidity ^0.4.0;

contract myfirstcontract {
}
```

We are setting up our first contract by declaring the version of Solidity we are going to use. In this case, select the version `0.4.25`, as we can see looking at the `Compile` menubar item on the right. 

`contract myfirstcontract` simply declares the new contract and the name of the new contract, i.e. myfirstcontract. 

Let's declare a couple of variables that we'll need to execute our contract. 

```
pragma solidity ^0.4.0;

contract myfirstcontract {
    string public greeting;
    address public owner;
}
```

In this code, we're introducing both a string (which is our message or our greeting called greeting. It is a public variable.) and an address that is also public called owner. 

We will be using the greeting variable to store our message such as "Hello!" or likewise. The address variable will be used to store our sender's address such as "0xca35b7d915458ef540ade6068dfe2f44e8fa733c". 

Now, let's add the event in which every time the greeting gets changed, the change is broadcast. This event will take the old greeting and the new greeting strings.

```
pragma solidity ^0.4.0;

contract myfirstcontract {
    string public greeting;
    address public owner;
    
    event GreetingChanged(string newGreeting, string oldGreeting);
}
```

Now, let's create our first function that sets the greeting and the owner respectively. 

```
pragma solidity ^0.4.0;

contract myfirstcontract {
    string public greeting;
    address public owner;
    
    event GreetingChanged(string newGreeting, string oldGreeting);

    function myfirstcontract(string _greeting){
        greeting = _greeting;
        owner = msg.sender;
    }
}
```

This function takes in the string _greeting, sets the greeting equal to the _greeting given, as well as the public address `owner` as the message sender or msg.sender. When run, it creates the greeting and owner that is broadcast initially when the contract is run. 


```
pragma solidity ^0.4.0;

contract myfirstcontract {
    string public greeting;
    address public owner;
    
    event GreetingChanged(string newGreeting, string oldGreeting);

    function myfirstcontract(string _greeting){
        greeting = _greeting;
        owner = msg.sender;
    }

    function setGreeting(string _greeting){
        require(owner == msg.sender);
        GreetingChanged(greeting, _greeting);
        greeting = _greeting;
    }
}
```

This last function provides a setter method for the greeting. It provides a means of updating the greeting if the sender is the same as the owner of the message.


## Execute Your First Smart Contract

Let's hit `Deploy` under the `Run` tab on the right. 

Check out your `Deployed Contracts`. There should be one contract listed. Under that contract, you can click on `Greeting` and there should be your greeting next to `0: insert_greeting_here`. You can do the same for the Owner. Click on the `Owner` tab and it will display the address of the owner. 

In the Account, you can see there is less than 100 ether in there now. This is because of the gas price that it takes to execute the contract. 

## Conclusion

You're done! You've successfully written your first smart contract in Solidity using the Remix IDE. You've learned a bit about what data types are allowed in Solidity, as well as how to execute your smart contract.  





