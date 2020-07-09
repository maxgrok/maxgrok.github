---
template: post
title: Clr.Fund:Explained (Part 3)
slug: clrfundexplainedpartthree
draft: false
date: 2020-07-09T10:54:48.733Z
description: "This is a post about how MACI fits into Clr.Fund's MVP and how
  Clr.Fund works. "
category: Raid Guild
tags:
  - Solidity
  - Clr.Fund
  - Ethereum
---
<em>This post is Part 3 of a series entitled "clr.fund:Explained".</em><br/><br/>
<em>Special thanks to Auryn MacMillan and Kirill Goncharov for reviewing this post. </em><br/>

<em>Disclaimer: This is a living process. For more up to date proof of concept information, please see the <a href="https://github.com/clrfund/monorepo/blob/3225adf672d0bffcce19160d8b65fca1f45cc42e/docs/clrfund.mmd">sequence diagram</a>.</em><br/>

<p>In this post, I will explain how MACI fits into clr.fund's flow and what clr.fund's flow is. Let's get familiar with the key ingredients to our clr.fund's flow. According to our sequence diagram (see link or end of post), we have an Owner, Pool Contributor, Recipient, Funding Round Factory, MACI Factory, Contributor, Funding Round, MACI, and a Coordinator.</p>
<hr/>

# Defining the Contracts
Each contract is integral to clr.fund. The main contracts are as follows:
## Funding Round Factory
The Funding Round Factory is the generic vehicle through which a funding round is started. It is used to register recipients, collect matching funds, deploy new rounds of funding, deploy MACI, transfer the matching funds, finalize the round (preventing additional funding), as well as potentially cancel the round.
## MACI Factory
MACI Factory is a vehicle for creating replicable instances of MACI to be used for the funding round that is created. When Owner calls the ```deployMaci()``` function in the `FundingRoundFactory`, it creates a new MACI instance. After this instance is created, the `FundingRoundFactory` contract calls ```setMaci()```. The `MACIFactory` is deployed first by the Owner before any other actions are taken.
## Funding Round
The `FundingRound` contract is used for contributors to donate to the pool of funding (not the matching pool), to sign up users for voting, and finalize the round, as well as submit the vote tally and a proof (```claimFunds()```), and tells MACI to verify the proof given by ```claimFunds()```, then transfers the funds to the Recipient.
## MACI
MACI is in charge of the signing up of users/voters for a funding round, voting, processing messages/votes, tallying votes, and verifying the proof that is created by ```claimFunds()``` in `FundingRound`.
<hr/>

# Defining the Key Roles
Each role is represented by a separate public key and does different actions. Here's an overview:
## The Owner
Initially, the Owner is the instantiator of clr.fund. The Owner deploys the MACI factory, deploys the Funding Round Factory, transfers ownership to Funding Round Factory, sets the MACI parameters (if necessary), and is to whom the Coordinator provides their public key. Once the Owner has the public key of the Coordinator, then they set the coordinator. The Owner can also set a new address as the Owner.
## Pool Contributor
The Pool Contributor is someone, or something (app, protocol, etc), who donates to the matching pool of funding for allocation after voting takes place.. 
## Recipient
The Recipient(s) are the ones who receive the funding once the voting is done. Adding a recipient can only be done by the Owner by calling ```addRecipient()``` on the `FundingRoundFactory`.
## Coordinator
The Coordinator is in charge of providing their public key to the Owner for registration in the funding round as a Coordinator. They process the messages (votes) after the voting deadline has passed, tally the votes and prove the correctness of it and publish the results of vote tally after processing the messages/votes. They provide the Recipient with the voting tally and a proofs which can be used by the Recipient to claim their share of the funds each round.
## Contributor
After the Owner deploys the `FundingRound` and calls ```setMaci()```, the Contributor donates to the regular pool for the funding round by calling ```contribute()``` function on the `FundingRound` contract. After the contribution is made and the voting period has started, contributors create a message to vote and call  by calling ```publishMessage()```. Voting proceeds until the voting deadline.
<hr/>

## Let's visualize what's happening step by step
Now that we know all the roles and contracts that get called, let's visualize the whole process of setting up clr.fund and finalizing a funding round from start to finish.
![Step 1-4](https://imgur.com/xH7Gsm4.jpg)

First, the Owner does 4 things: deploys the `MACIFactory` and `FundingRoundFactory` contracts. Second, they transfer ownership of the `FundingRoundFactory` contract to the `MACIFactory`. Lastly, if necessary, they set the MACI parameters, which include, among others, the ```signUpDuration``` and ```votingDuration``` . For more information on MACI parameters, see the <a href="https://github.com/clrfund/monorepo/blob/master/contracts/contracts/MACIFactory.sol">clr.fund MACIFactory contract</a>.

![Step 5-6](https://imgur.com/SelKv3c.jpg)
After clr.fund's basic setup, the Recipients ask the Owner to be added to the registration for projects eligible for receiving funds from clr.fund after a funding round has been finalized. This happens on a loop with as many projects as there are that get vetted and participate in the funding round.
![Step 7-11](https://imgur.com/qZWoR3M.jpg)
After adding all the Recipients, the Owner deploys a new funding round by calling ```deployNewRound()``` on ```FundingRoundFactory```. This creates a new funding round for users to come and contribute funds to the funding pool for different projects. When the Owner calls ```deployMaci()``` on ```FundingRoundFactory``` contract, the ```FundingRoundFactory``` contract calls ```deployMaci()``` on ```MACIFactory```, which creates a new instance of MACI for the funding round's use. ```FundingRoundFactory``` calls ```setMaci()``` on the ```FundingRound``` contract to link the MACI instance to the funding round. This completes the setup of the funding round. Now, we need to enroll the Contributors in the funding round before the ```signUpDuration``` is over.
![Step 12-15](https://imgur.com/ISqyrpa.jpg)
In the proof of concept, only vetted Contributors are registered. The Contributors are vetted by clr.fund's team. In later rounds, this curation process will be more permissionless. The Contributor can then donate to the funding pool for projects, at that moment, they  ```signUp()``` to vote/send a message (an encrypted command). The Contributor creates a message and calls ```publishMessage()```. This process happens with each Contributor until all Contributors are signed up and have published a message/voted. Once all the voting happens, then we hit the voting deadline.
![Step 17-21](https://imgur.com/FGGwlfN.jpg)
Once the voting deadline has passed, the Coordinator can process and tally the votes, as well as provide the vote tally to the Recipient. The Owner proceeds to call ```transferMatchingFunds()``` on the ```FundingRoundFactory``` contract, which then transfers funds and finalizes the round of funding. The Recipient submits the vote tally and a proof via the ```claimFunds()``` function on the ```FundingRound``` contract. The ```FundingRound``` contract passes the proof that the Recipient sent to MACI for verification. Once verified, the ```FundingRound``` contract transfers the funds to the Recipient. This completes the funding round.

## Conclusion
MACI fits into clr.fund’s flow by providing the minimum architecture needed to prevent collusion; mainly, it provides a way for voters to hide their votes via encryption to prevent impact of bribery. By now, we know how all the different parts fits together in the 21 step process to complete a funding round. We know that there is an Owner who instantiates the entire process as well as registering vetted Contributors, a Coordinator who is in charge of processing and tallying the votes, Recipients who receive funding, PoolContributors who contribute to the matching pool for funding allocation, and Contributors who provide funds and votes for different Recipients/projects. This whole process is the entire process behind clr.fund's proof of concept. I hope you enjoyed learning this as much as I did!
In case you want more information, I've done a ton of research for you to fall down different rabbit holes.
<hr/>

## References/Further Readings:
<p><small><strong>BrightID.</strong> <a href="https://www.brightid.org/">https://www.brightid.org/</a>. Accessed: June 17th, 2020.</small><br/>
<small><strong>Buterin, Vitalik.</strong> "On Collusion". <a href="https://vitalik.ca/general/2019/04/03/collusion.html">https://vitalik.ca/general/2019/04/03/collusion.html</a>. April 3rd, 2019.</small><br/>
<small><strong>Buterin, Vitalik, Hitzig, Zoe, and Weyl, Glen</strong>. "Liberal Radicalism: A Flexible Design for Philanthropic Matching Funds". <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3243656">https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3243656</a>. September 18th, 2018. Accessed: June 19th, 2020. </small><br/>
<small><strong>Clr.fund Data Flow Image</strong>.<a href="https://imgur.com/RAeSksA.jpeg">https://imgur.com/RAeSksA.jpeg</a>.</small><br/>
<small><strong>"Collusion"</strong>. Wikipedia. <a href="https://en.wikipedia.org/wiki/Collusion">https://en.wikipedia.org/wiki/Collusion</a>. Accessed: June 17th, 2020.</small><br/>
<small><strong>"edDSA"</strong> -- <a href="https://en.wikipedia.org/wiki/EdDSA">Wikipedia</a>. Accessed: June 17th, 2020</small><br/>
<small><strong>"ECDH" </strong>-- <a href="https://en.wikipedia.org/wiki/
Elliptic-curve_Diffie%E2%80%93Hellman">Wikipedia</a>. Accessed: june 17th, 2020.</small><br/>
<small><strong>Eason, Brian</strong>. "$120 million in requests and $40 million in the bank. How an obscure theory helped prioritize the Colorado budget." <a href="https://coloradosun.com/2019/05/28/quadratic-voting-colorado-house-budget/">https://coloradosun.com/2019/05/28/quadratic-voting-colorado-house-budget/</a>. May 28th, 2019. Accessed: June 17th, 2020.</small><br/>
<small><strong>FlatOutCrypto</strong>. "Crypto Intro: Sybil Attacks". <a href="https://flatoutcrypto.com/home/cryptointrosybilattack">https://flatoutcrypto.com/home/cryptointrosybilattack</a>. Accessed: June 18th, 2020. </small> <br/>
<small><strong>Kronovet, Daniel, Fischer, Aron, and du Rose, Jack.</strong> "Decentralized Capital Allocation via Budgeting Boxes". <a href="https://colony.io/budgetbox.pdf">Paper</a>. December 11th, 2018. Accessed: June 19th, 2020.</small><br/>
<small><strong>Lalley, Steven and Weyl, Glen</strong>. "Quadratic Voting: How Mechanism Design Can Radicalize Democracy". American Economic Association Papers and Proceedings, Vol. 1, No. 1, 2018. February 13th, 2012. <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2003531">https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2003531</a>.  Accessed: June 16th, 2020.</small><br/>
<small><strong>"MACI"</strong>. <a href="https://github.com/appliedzkp/maci">https://github.com/appliedzkp/maci</a>. Accessed: June 19th, 2020.</small></br>
<small><strong>"maci/specs/"</strong>. <a href="https://github.com/appliedzkp/maci/tree/master/specs">https://github.com/appliedzkp/maci/tree/master/specs</a>. Accessed: June 19th, 2020.</small></br>
<small><strong>"Protocol"</strong>. <a href="https://en.wikipedia.org/wiki/Protocol_(object-oriented_programming">Wikipedia</a>. Accessed: June 17th, 2020.</small><br/>
<small><strong>"Quadratic Voting"</strong>.<a href="https://en.wikipedia.org/wiki/Quadratic_voting">Wikipedia</a>. Accessed: June 17th, 2020.</small><br/>
<small><strong>Rogers, Adam</strong>. "Colorado Tried A New Way to Vote: Make People Pay -- Quadratically". Wired. <a href="https://www.wired.com/story/colorado-quadratic-voting-experiment/">https://www.wired.com/story/colorado-quadratic-voting-experiment/</a>. Accessed: June 17th, 2020.</small><br/>
<small><strong>Rosic, Ameer</strong>. "What are zk-SNARKs?: The Comprehensive Spooky Moon Math Guide". <a href="https://blockgeeks.com/guides/what-is-zksnarks/">https://blockgeeks.com/guides/what-is-zksnarks/</a>. June 22nd, 2017. Accessed: June 22nd, 2020.</small>
<small><strong>"Sybil Attack"</strong>. <a href="https://en.wikipedia.org/wiki/Sybil_attack">Wikipedia</a>. Accessed: June 17th, 2020.</small><br/>
<small><strong>Weyl, Glen</strong>."Radical Markets". 2018. <a href="https://www.amazon.com/Eric-Posner-ebook/dp/B07TP5HLWQ/ref=sr_1_2?dchild=1&keywords=radical+markets&qid=1592406215&sr=8-2">Amazon</a>.</small><br/>
<small><strong>"What are zk-SNARKs?"</strong>. <a href="https://z.cash/technology/zksnarks/">https://z.cash/technology/zksnarks/</a>. Accessed: June 17th, 2020.</small><br/>
<small><strong>Wei Jie Koh.</strong> "Minimum Anti-Collusion Infrastructure". <a href="https://www.youtube.com/watch?v=sKuNj_IQVYI">YouTube</a>. May 2020.</small><br/></p>

### Here is a technical sequence diagram of how clr.fund works:
<a href="https://raw.githubusercontent.com/clrfund/monorepo/master/docs/clrfund.svg">![SequenceDiagram](https://raw.githubusercontent.com/clrfund/monorepo/master/docs/clrfund.svg) </a>  

### Here is clr.fund's code repo:
<a href="https://github.com/clrfund/monorepo/blob/master/docs/clrfund.mmd">clr.fund</a>
