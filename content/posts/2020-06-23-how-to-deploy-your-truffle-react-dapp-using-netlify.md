---
template: post
title: How to Deploy Your Truffle React DApp Using Netlify
slug: one-hundred-and-twelfth-post
draft: false
date: 2020-02-01T21:00:00.000Z
description: This is a quick post about how to deploy your DApp using Netlify.
category: Ethereum
tags:
  - Solidity
  - Truffle
  - Netlify
---
Here is a short post on how to deploy your Dapp using Netlify.

<em>Pre-requisites</em>: Github account. Netlify account. Truffle project with Solidity smart contracts all ready deployed to Rinkeby through the Truffle React box. A front end in ReactJS. Metamask. Rinkeby testnet Ether or mainnet Ether. 

Setup a Github repo with your project uploaded via Git. 

Within your repo, init git and upload your files to the repo. 

```js
git init
git remote add origin http://www.github.com/username/repo.git
git add . 
git commit -m "first commit"
git push origin master
```

After this, then setup a Netlify account on Netlify.com. Log in with your Github account. 

Setup the build command to be `npm run-script build` within the `client/` directory. 

And the base directory to be `client/`. 

The publish directory should be `client/build/`.

Go to `deploys` and click `trigger deploy` and select `deploy site`. 

Wait for it to build, then visit your custom domain! 

## Congratulations! 

If all went well, you have deployed your Dapp to Netlify!


