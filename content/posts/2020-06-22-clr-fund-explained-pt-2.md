---
template: post
title: "Clr.Fund: Explained Pt. 2"
slug: clrfund
draft: false
date: 2020-06-22T18:32:30.736Z
description: "This post dives into how Clr.Fund works as part 2 of Clr.Fund: Explained."
category: Raid Guild
tags:
  - Clrfund
  - Ethereum
  - Raid Guild
---
<strong>TL:DR;
Clr.fund uses hack-resistant methods to create a way to allocate funding for public goods projects in the Ethereum ecosystem. It's coming soon.</strong>
-----------------------------------------------

<h4>MACI -- Minimal Anti-Collusion Infrastructure </h4><p> In order to understand anti-collusion infrastructure, we must understand collusion. <a href="https://en.wikipedia.org/wiki/Collusion">Collusion</a> = "a secret cooperation or deceitful agreement in order to deceive others". Collusion in clr.fund's case is to coordinate the domination of funding allocation to one project or another. <br/><br/>
MACI seeks to eliminate basic collusion by following the suggestion in <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3243656">Buterin, Hitzig, and Weyl 2018</a> to "not only [make] votes private, but a voter cannot prove to anyone else who they voted for (or even, ideally, whether or not they voted)even if they wanted to" (Section 5.2 Collusion and deterrence, p.16). However, there is a vulnerability at setup time, as pointed out by <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3243656">Buterin, Hitzig, and Weyl 2018</a> where a coordinator of the funding campaign itself could be bribed to give the keys to the attackers for decrypting the messages/votes from various members to gain control over funding allocation and collude. Clr.fund works by trusting coordinators not to do this.<br/><br/>
<strong>How exaclty does the Minimal Anti-Collusion Infrastructure (MACI) work? </strong>

There are two zk-SNARK circuits in MACI: one for the quadratic voting trie and one for the state root transition trie. 

In Wie's video, he breaks down MACI. 
<p>TO DO: Go through a MACI video walk through. Break down the MACI process. ---> There are two zk-SNARKs circuits used in MACI. One for the state root transition proof and another for the quadratic vote tallying. 
TO DO: Make a diagram? </p>

The flow of MACI goes like this:
1. XYZ
2. ABC
3. GHI

   <strong>edRSA</strong> Wikipedia article. Their role in MACI. TO DO: Write up.<br/><br/>
    <strong>ECDH</strong> Wikipedia article. Their role in MACI.  TO DO: Write up.

<p>For more information on MACI, see also:<br/><strong>Wei Jie Koh.</strong> "Minimum Anti-Collusion Infrastructure". <a href="https://www.youtube.com/watch?v=sKuNj_IQVYI">YouTube</a>. May 2020. Accessed: June 19th, 2020.<br/>
<strong>Code Repo</strong>: "MACI". <a href="https://github.com/appliedzkp/maci">Github</a>. Accessed: June 19th, 2020.<br/>
<strong>Code Specs:</strong> "maci/specs/". <a href="https://github.com/appliedzkp/maci/tree/master/specs">Github</a>. Accessed: June 19th, 2020. <br/>
</p>

<h4>Sybil Attack Resistance </h4><p><a href="http://www.brightid.org/">BrightID</a> is used for sybil resistance in the first version of Clr.fund's MVP by providing proof of unique identities on the blockchain. This prevents the sybil attack from occuring on a permissionless funding platform like clr.fund.</p>

<h3>How Clr.Fund Prototype Works</h3>

Here's a flow of how the contracts will work:
<a href="https://imgur.com/RAeSksA">
![Include picture of the flow of the contracts here.](https://imgur.com/RAeSksA.jpeg)</a>
<em>^Click to visit Imgur and zoom into image. </em>

Let's walk through this. 

TO DO: ---> Needs explanation in steps with code snippets?
TO DO: Verify with Kirill that your steps and understanding is correct.  

<h3>Congratulations! You now understand the Clr.Fund Basics!</h3>
<p>
Phew, that was a long couple of posts. By now, you have an understanding of quadratic voting, quadratic funding, zk-SNARKs, MACI, and how it all fits together to build clr.fund's funding allocation protocol. I've only touched on each of these topics. They are rabbit holes in and of themselves. There are more attacks that could potentially happen and yet more terminology, but this is the basics of what clr.fund is and why clr.fund is better than current solutions for funding public goods projects in Ethereum. Please read the references below to fall down some great rabbit holes and continue your learning! 
</p>
 
<strong><h4>Further Reading/References*: </h4></strong>
<p><small><strong>Buterin, Vitalik.</strong> "On Collusion". <a href="https://vitalik.ca/general/2019/04/03/collusion.html">https://vitalik.ca/general/2019/04/03/collusion.html</a>. April 3rd, 2019.</small><br/>
<small><strong>Buterin, Vitalik, Hitzig, Zoe, and Weyl, Glen</strong>. "Liberal Radicalism: A Flexible Design for Philanthropic Matching Funds". <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3243656">https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3243656</a>. September 18th, 2018. Accessed: June 19th, 2020. </small><br/>
<small><strong>BrightID.</strong> <a href="https://www.brightid.org/">https://www.brightid.org/</a>. Accessed: June 17th, 2020.</small><br/>
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
<small><strong>"Sybil Attack"</strong>. <a href="https://en.wikipedia.org/wiki/Sybil_attack">Wikipedia</a>. Accessed: June 17th, 2020.</small><br/>
<small><strong>Weyl, Glen</strong>."Radical Markets". 2018. <a href="https://www.amazon.com/Eric-Posner-ebook/dp/B07TP5HLWQ/ref=sr_1_2?dchild=1&keywords=radical+markets&qid=1592406215&sr=8-2">Amazon</a>.</small><br/>
<small><strong>"What are zk-SNARKs?"</strong>. <a href="https://z.cash/technology/zksnarks/">https://z.cash/technology/zksnarks/</a>. Accessed: June 17th, 2020.</small><br/>
<small><strong>Wei Jie Koh.</strong> "Minimum Anti-Collusion Infrastructure". <a href="https://www.youtube.com/watch?v=sKuNj_IQVYI">YouTube</a>. May 2020.</small><br/></p>
 
 
<p><small>* Not using any particular citation format like MLA, APA, etc. Just doing alphabetical order by title or author name. </small></p>


