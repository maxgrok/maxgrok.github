---
template: post
title: "Clr.Fund: Explained (Part 2)"
slug: clrfund
draft: false
date: 2020-06-22T21:39:00.000Z
description: "This post dives into how MACI works as part 2 of Clr.Fund: Explained."
category: Raid Guild
tags:
  - Clrfund
  - Ethereum
  - Raid Guild
---
<em> Special thanks to Kirill Goncharov, Koh Wei Jie, and Auryn MacMillan for reviewing this post</em>

<strong>TL:DR;
MACI prevents basic collusion by making the voting encrypted and not able to be decrypted by anyone except the coordinator (at the end of the voting period). <br/><br/>In part three of Clr.Fund:Explained, we will go over how MACI fits into the Clr.Fund MVP flow.</strong>
-----------------------------------------------
In this post, we will go over how Minimum Anti-Collusion Infrastructure (MACI) works. What is MACI? 
 
<h4>Minimal Anti-Collusion Infrastructure (MACI) </h4><p> In order to understand MACI, we must understand collusion. <a href="https://en.wikipedia.org/wiki/Collusion">Collusion</a> is "a secret cooperation or deceitful agreement in order to deceive others". The case for basic collusion that MACI takes on is bribery for voting purposes. MACI tackles collusion by making it impossible (read, very costly and not scalable) to know how someone else voted. In clr.fund’s case, that means it is impossible for a briber to verify a bribee’s actions.<br/><br/>
 
<strong>How does MACI prevent this basic collusion? Let’s find out!</strong> <br/><br/>
 
Here’s how a simple bribing attack might occur: 

![Before MACI flow attack](https://imgur.com/wv77NZr.jpg)

<p>In this example, Eve wants Alice to vote for Party B and will pay Alice 5 ETH to do so, but Alice needs a way to prove that Alice indeed voted for Party B, if she accepts the bribe, which we are assuming she does here. Therefore, Alice must provide the transaction hash of her vote to Eve, proving that she indeed voted for Party B, then Eve will -- if honest -- pay Alice 5 ETH for her vote. The result is that the bribe had an impact on the election. </p>

MACI seeks to eliminate basic collusion by following the suggestion in <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3243656">Buterin, Hitzig, and Weyl 2018</a> to "not only [make] votes private, but a voter cannot prove to anyone else who they voted for (or even, ideally, whether or not they voted) even if they wanted to" (Section 5.2 Collusion and deterrence, p.16). <br/><br/>
<strong>How exactly does the Minimal Anti-Collusion Infrastructure (MACI) work? </strong>
 
Here is a diagram of an example of how MACI could be used in a voting scenario:

![MACI Demo Flow](https://imgur.com/yixYwDT.jpg)
Adapted from: <a href="https://www.youtube.com/watch?v=sKuNj_IQVYI">Minimum Anti-Collusion Infrastructure, 2020, Koh Wei Jie</a>
 
This scenario above eliminates the ability of Alice to prove she voted for Party B because her vote is encrypted and can only be decrypted by the coordinator.
 
Here is how MACI flow works: 
<a href="https://imgur.com/gXGrny9.jpg">
![MACI Flow Diagram](https://imgur.com/gXGrny9.jpg)</a>
 ^<em>(Click on the above to zoom into image)</em>

The creator of the instance of the MACI contract instantiates MACI with the ```coordinators/signUpGatekeeper’s address```, along with the other parameters required by MACI to setup the voting scenario <em>(see diagram above)</em>. After this, the users can use ```signup()``` to sign up for voting so long as it is before the deadline for signing up as a voter. After signing up, users can then publish messages/votes before the voting deadline for the voting round. After all the votes are completed from all users, then the coordinator batch processes the messages. Through a CLI, the coordinator tallies the votes, then verifies the results on-chain using ```verifyTallyResult()```.
 
<strong>Here is a technical overview of how MACI works: </strong>
![MACI How it Works](https://imgur.com/yy91vC6.jpg)<br/>
<a href="https://www.youtube.com/watch?v=sKuNj_IQVYI">Minimum Anti-Collusion Infrastructure, 2020, Koh Wei Jie</a>

As you can see, the smart contract is instantiated with a state tree root and a message tree root. The message tree contains all the leaves that are the commands/votes that have been encrypted. Commands are instructions about how to modify a state leaf. The state tree contains all the user's public keys, voice credit balance, and the vote option tree roots associated with the user, as well as nonces, up to date as leaves on the state tree. A state leaf contains the user's EdDSA public key that was generated or provided by the user, their voice credit balance, their vote option tree root, and a nonce. A user can vote/issue a command by providing the state tree index, their EdDSA public key, the vote option tree index, the vote weight value, a nonce, and a random salt. The vote effectively provides instructions on how to modify a state leaf (what their vote is, how many voice credits/votes they have left, a nonce, and a random salt). 

 <h3>What do zk-SNARKs do for MACI? </h3>

![zk-SNARKs and MACI](https://imgur.com/dsprNZK.jpg)

<p>As we learned in the first post, MACI uses zk-SNARKs. How? By using circuits for the state root transition tree and the quadratic voting tree. They prevent the trusted coordinator from incorrectly processing the votes or incorrectly tallying the votes, as well as provide vote secrecy until the end of the voting period. Votes are encrypted, but can be unencrypted in the zk-SNARK circuit <a href=”https://www.youtube.com/watch?v=sKuNj_IQVYI”>Koh Wei Jie, 2020</a>.  </p>

<h3>Vulnerabilities with MACI</h3>
 
<p>There is a vulnerability at setup time, as pointed out by <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3243656">Buterin, Hitzig, and Weyl 2018</a> and 
<a href=”https://www.youtube.com/watch?v=sKuNj_IQVYI”>Koh Wei Jie 2020</a> where a coordinator of the voting itself could be bribed to give the keys to the attackers for decrypting the messages/votes from various members to gain control over voting. </p>
 
<h3>How can the coordinator mess up the voting though?</h3>

![Trusted Coordinator](https://imgur.com/mPiEyx5.jpg)
 
<a href="https://www.youtube.com/watch?v=sKuNj_IQVYI">Minimum Anti-Collusion Infrastructure, 2020, Koh Wei Jie</a><br/>

A coordinator can decrypt all the commands/encrypted messages/votes, withhold or delay the message processing, withhold or delay the vote tallying, collude with bribers, but they cannot censor commands/encrypted votes, process messages incorrectly, or tally votes incorrectly. The only real threat to the voting is bribing the coordinator. So, if untrustworthy, they may accept a bribe at the time of setting up the voting, which would give the briber the keys to decrypt the votes from various members, gaining control over the voting. The way to prevent this from happening is with a trusted setup, but we will not go into depth in this post on trusted setups.

For more on trusted setups in MACI, see <a href="https://www.youtube.com/watch?v=YbJw8_liYyo">zkPodcast Episode 133</a>.</p>

<h3>How MACI prevents collusion</h3>
<p>MACI keeps the users’ votes for funding allocation secret from anyone except the coordinator who can decrypt the votes, preventing collusion by bribery of voting, as an individual user cannot prove whether they voted one way or the other after having voted. By using zk-SNARKs and Ethereum, MACI is able to prevent the type of bribery we went over in our first diagram on potential bribery above.  </p>
  
<h3>In the future, MACI may...</h3>
 
<p>have a trusted setup or moves to another zero knowledge system that does not need a trusted setup to account for coordinator collusion situations (<a href="https://www.youtube.com/watch?v=sKuNj_IQVYI">Minimum Anti-Collusion Infrastructure, 2020, Koh Wei Jie</a>).</p>

<p>For more information on MACI, see also:<br/><strong>Wei Jie Koh.</strong> "Minimum Anti-Collusion Infrastructure". <a href="https://www.youtube.com/watch?v=sKuNj_IQVYI">YouTube</a>. May 2020. Accessed: June 19th, 2020.<br/>
<strong>Code Repo</strong>: "MACI". <a href="https://github.com/appliedzkp/maci">Github</a>. Accessed: June 19th, 2020.<br/>
<strong>System Spec:</strong> "maci/specs/". <a href="https://github.com/appliedzkp/maci/tree/master/specs">Github</a>. Accessed: June 19th, 2020. <br/>
</p>

<h3>How does this work with Clr.Fund?</h3>
<p>Now that we understand zk-SNARKs and MACI, as well as collusion, bribery, and Sybil attacks, we can explain the flow of Clr.Fund. In part three of Clr.Fund: Explained, we will go over exactly this. <br/><br/>Stay tuned for the next post! </p>
