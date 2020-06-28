---
template: post
title: "Clr.Fund: Explained (Part 2)"
slug: clrfund
draft: true
date: 2020-06-22T21:39:00.000Z
description: "This post dives into how Clr.Fund works as part 2 of Clr.Fund: Explained."
category: Raid Guild
tags:
  - Clrfund
  - Ethereum
  - Raid Guild
---
<strong>TL:DR;
Clr.fund uses attack-resistant methods to create a way to allocate funding for public goods projects in the Ethereum ecosystem. It's coming soon.</strong>
Here is how MACI fits into Clr.Fund's flow: 
(INSERT DIAGRAM HERE)
-----------------------------------------------
In this post, we will go over how Minimum Anti-Collusion Infrastructure (MACI) works and how it fits into the basic overview of Clr.Fund’s user flow. Let's tackle how MACI works first. 
 
<h4>MACI -- Minimal Anti-Collusion Infrastructure (MACI) </h4><p> In order to understand MACI, we must understand collusion. <a href="https://en.wikipedia.org/wiki/Collusion">Collusion</a> is "a secret cooperation or deceitful agreement in order to deceive others". The case for basic collusion that MACI takes on is bribery for voting purposes. MACI tackles collusion by making it impossible (read, very costly and not scalable) to know how someone else voted. In clr.fund’s case, that means it is impossible for a briber to verify a bribee’s actions.
 
<strong>How does this prevent collusion? Let’s find out!</strong> <br/><br/>
 
Here’s how a simple bribing attack might occur: 
![Before MACI flow attack](https://imgur.com/wv77NZr.jpg)

MACI seeks to eliminate basic collusion by following the suggestion in <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3243656">Buterin, Hitzig, and Weyl 2018</a> to "not only [make] votes private, but a voter cannot prove to anyone else who they voted for (or even, ideally, whether or not they voted) even if they wanted to" (Section 5.2 Collusion and deterrence, p.16). <br/><br/>
 
<strong>How exactly does the Minimal Anti-Collusion Infrastructure (MACI) work? </strong>
 
Here is a diagram of an example of how MACI could be used in a voting scenario:

![MACI Demo Flow](https://imgur.com/yixYwDT.jpg)
Adapted from: <a href="https://www.youtube.com/watch?v=sKuNj_IQVYI">Minimum Anti-Collusion Infrastructure, 2020, Koh Wei Jie</a>
 
This eliminates the ability of Alice to prove she voted for Party B because her vote is encrypted and can only be decrypted by the coordinator.
 
Here is how MACI flow works: 
![MACI Flow Diagram](diagram here)
 
Users and coordinators register their pubkey with MACI by calling the constructor in the MACI contract, then …[TO DO: to be filled in] 
 
Here is an overview of how MACI works: 
![MACI How it Works](https://imgur.com/yy91vC6.jpg)<br/>
<a href="https://www.youtube.com/watch?v=sKuNj_IQVYI">Minimum Anti-Collusion Infrastructure, 2020, Koh Wie Jie</a>
 
<h3>What do zk-SNARKs do for MACI? </h3>
<p>As we learned in the first post, MACI uses zk-SNARKs. How? By using circuits for the state root transition tree and the quadratic voting tree. </p>
 
<strong>EdRSA</strong> Wikipedia article. Their role in MACI. TO DO: Write up.<br/><br/>
<strong>ECDH</strong> Wikipedia article. Their role in MACI.  TO DO: Write up.
 
<h3>Vulnerabilities with MACI</h3>
 
There is a vulnerability at setup time, as pointed out by <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3243656">Buterin, Hitzig, and Weyl 2018</a> and 
<a href=”https://www.youtube.com/watch?v=sKuNj_IQVYI”>Koh Wei Jie 2020</a> where a coordinator of the funding campaign itself could be bribed to give the keys to the attackers for decrypting the messages/votes from various members to gain control over funding allocation and collude. Right now, Clr.fund works by trusting coordinators not to do this. 
 
Here are the coordinators exact abilities with MACI and how they can interfere with funding allocation voting: 
<a href="https://www.youtube.com/watch?v=sKuNj_IQVYI">Minimum Anti-Collusion Infrastructure, 2020, Koh Wei Jie</a>
 
For more on trusted setups in MACI, see <a href="https://www.youtube.com/watch?v=YbJw8_liYyo">zkPodcast Episode 133</a>. [MAY WANT TO GO MORE IN-DEPTH]

<h3>How does it work in Clr.Fund?</h3>
 
Here is a basic overview of how it fits into Clr.Fund’s flow: 
[Basic Flow of Clr.Fund here](INSERT DIAGRAM))
 
MACI keeps the users’ votes for funding allocation secret from anyone except the coordinator who can decrypt the votes, preventing collusion by bribery of voting, as an individual user cannot prove whether they voted one way or the other after having voted. By using zk-SNARKs and Ethereum, MACI is able to prevent the type of bribery we went over in our diagram above.  
 
<p>For more information on MACI, see also:<br/><strong>Wei Jie Koh.</strong> "Minimum Anti-Collusion Infrastructure". <a href="https://www.youtube.com/watch?v=sKuNj_IQVYI">YouTube</a>. May 2020. Accessed: June 19th, 2020.<br/>
<strong>Code Repo</strong>: "MACI". <a href="https://github.com/appliedzkp/maci">Github</a>. Accessed: June 19th, 2020.<br/>
<strong>System Spec:</strong> "maci/specs/". <a href="https://github.com/appliedzkp/maci/tree/master/specs">Github</a>. Accessed: June 19th, 2020. <br/>
</p>
  
<h3>In the future, MACI may...</h3>
 
have a trusted setup or moves to another zero knowledge system that does not need a trusted setup to account for coordinator collusion situations (<a href="https://www.youtube.com/watch?v=sKuNj_IQVYI">Minimum Anti-Collusion Infrastructure, 2020, Koh Wei Jie</a>).
 

<h3>What's next? </h3>
Now that we understand zk-SNARKs and MACI, as well as collusion, bribery, and Sybil attacks, in part three of Clr.Fund: Explained, we will go over exactly how the Clr.Fund MVP is slated to work. Stay tuned for the next post! 
