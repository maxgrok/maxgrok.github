---
template: post
title: How to Verify Yourself on Circles
slug: verify-yourself-circles
draft: false
date: 2020-10-19T07:03:47.348Z
description: "Want to know how verify yourself on Circles? Let's break down the technical steps, so you don't have to wait for 'trust'."
category: UBI
tags:
  - Circles
  - UBI
  - xDAI
---

Pre-requisites: Metamask and Metamask account. Internet.

### 1. Sign up for [Circles](https://www.circles.garden).

### 2. Obtain [DAI](https://www.uniswap.io/) in your Metamask account while connected to Ethereum Mainnet. 

### 3. Convert your DAI to xDAI on the [POA Bridge UI](https://dai-bridge.poa.network/), while connected to the Ethereum Mainnet.
- Select how much DAI you want to transfer. Hint: you only need 0.12 (0.02 to cover costs of sending the xDAI to your circle account address). 
- Confirm the transaction in your Metamask account. 

### 4. Add xDAI Network to your Metamask
- Select the network tab in your metamask. CustomRPC
- Select ‘Custom RPC’
- Enter in the following: 
    - Network name: xDAI 
    - New RPC URL: rpc.xdaichain.com/ 
    - Chain ID: 100 
    - Symbol: xDAI 
    - Block Explorer URL: https://blockscout.com/poa/xdai
    - Select the xDAI network in Metamask in the same dropdown.
    
### 5. Add xDAI token to your Metamask on xDAI network
- 0x6B175474E89094C44Da98b954EedeAC495271d0F -- xDAI token address
- Click 'Add Token' in your Metamask account while connected to the xDAI network. 
- Select 'Custom Token'
- Enter in the address: 0x6B175474E89094C44Da98b954EedeAC495271d0F (xDAI token address on Ethereum mainnet)

### 6. Obtain your Circle's address from the 'Share Profile' link
- Back in Circle, you should see the verification page requesting 3 trust verifications. 
- Copy and paste your share profile address into a text editor of your choice. 
- The address after 'profile/' in the Circles URL is your Circles address. 
    ex.  https://circles.garden/profile/0x5fcffCf56D228B245803FEe1d623a256710B4053
    The 0x5fcffCf56D228B245803FEe1d623a256710B4053 is your address. 

### 7. Send 0.1 xDAI to your Circle's address
- Use Metamask on the xDAI network to send 0.1 xDAI to your Circles Address.
- Wait for success on the etherscan tx. 
- Click on xDAI
- Click on the pending transaction, 
- Click the arrow sign to the upper right hand corner to see another browser open with the transaction status. 
- Wait for 'Success', and probably about 8 block confirmations, and then ...

### 8. Refresh your Circle verification page!

### 9. Congratulations! You've verified yourself on Circles! Enjoy your UBI!

