---
template: post
title: Build Your Own Ethereum Block Display Site
slug: seventy-eighth-post
draft: false
date: 2018-11-30T21:30:00.000Z
description: This is a post about how to build a ethers.io API empowered site to
  explore the Ethereum blockchain.
category: "Blockchain "
tags:
  - Blockchain
  - IPFS
  - Ethereum
---

## Pre-requisites: 
git<br>
nvm<br>
node.js<br>
MetaMask<br>
Basic knowledge of blockchain<br>

---------------------

Install ethers-cli<br>

`npm install -g ethers-cli`<br>
`mkdir explorer`<br>
`cd explorer`<br>
`touch index.html`<br>

```html
    <html>
        <head>
            <title>Block Explorer</title>
        </head>
        <body>
            Hello World!
            <script type="text/javascript" src="https://cdn.ethers.io/scripts/ethers-app-v0.2.min.js"></script>
        </body>
    </html>
```
To get the deploy to work with the v0.5 of ethers-cli, you need to deploy this: <br><br>

`ethers-deploy serve --rpc http://localhost:8080`<br>

Go to your browser and navigate to `localhost:8080/`.

Then navigate to the localhost:8080. Make sure you have MetaMask running as well. Also, enable insecure dApps to run otherwise your site will not work! 

You should see your 'Hello World!' displayed on screen!<br>

Now, let's add some Javascript code in a separate script tag to the HTML document to get the latest 10 blocks validated on the Ethereum blockchain. <br>

```html
<script>
            ethers.onready = function() {
                updateBlocks();
            }

            function updateBlocks() {
                var lastBlockNumber = ethers.blockchain.getBlockNumber();
                lastBlockNumber.then(function(number) {
                    console.log('The last block number: ' + number);
                    for(var i=0; i<10; i++) {
                        ethers.blockchain.getBlock(number-i).then(function(block) {
                            printBlock(block);
                        });
                    }
                });
            }

            function printBlock(block) {
                var table = document.getElementById('blocks');
                var row = table.insertRow(-1);
                var cell1 = row.insertCell(0);
                var cell2 = row.insertCell(1);
                var cell3 = row.insertCell(2);
                cell1.innerHTML = block.number;
                cell2.innerHTML = block.hash;
                cell3.innerHTML = block.timestamp;
            }
        </script>     
```

Let's go ahead and remove the `hello world!` from our application and replace it with the following code to display the latest 10 blocks!<br>

```html
<table id="blocks" width="100%">
            <tr>
                <th>Number</th>
                <th>Hash</th>
                <th>Timestamp</th>
            </tr>
        </table>
```

Now, let's deploy our app on the localhost!

Go to your command line and type `ethers-deploy serve`<br>

Go to your `localhost:8080` and you should see all the last 10 latest blocks validated by the Ethereum blockchain! <br>

## Congratulations! You've built your first blockchain site interacting with the ethers API. 

## Bonus Points

Google around and find out how to install and add your files to `ipfs` for a decentralized site!