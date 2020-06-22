---
title: "Clr.Fund: Explained Pt. 1"
date: "2020-06-22T11:48:13.121Z"
layout: post
draft: false
path: "/posts/one-hundred-and-nineteenth-post/"
category: "Learn"
tags:
- "Learning"
- "Ethereum"
- "zk-SNARKs"
- "Quadratic Funding"
- "MACI"
description: "A post explaining the use of quadratic funding, zk-SNARKs, MACI, and more... in clr.fund"
---
<strong>TL:DR; 
Clr.fund uses hack-resistant methods to create a way to allocate funding for public goods projects in the Ethereum ecosystem. It's coming soon.</strong>

<em>Disclaimer: I'm assuming knowledge of Ethereum here, including Solidity, as an engineer.
I'm new in my understanding of many of the topics in this post and am using the <a href="https://www.maxgrok.com/posts/one-hundred-and-eighteenth-post/">Feynman Technique</a>. in this post to solidify my own evolving learning on these topics. Read: I've only spent a few days researching them.

Feel free to connect and/or correct on this post by reaching out via email (<a href="mailto: max@maxgrok.com>">max@maxgrok.com</a>) or by DMing my twitter is (<a href="https://www.twitter.com/maxxgrok/">@maxxgrok</a>). </em><br/><br/>
Now, onwards!
--------------------------------------------------------------

This post explains clr.fund in technical terms for engineers seeking a better understanding of primarily quadratic funding, quadratic voting, zk-SNARKs, and MACI. First, I'll explain what clr.fund is, then I'll go into why clr.fund instead of traditional sources of funding. Third, I'll tackle the top relevant attacks to a protocol like clr.fund. Fourth, I'll explain exactly how clr.fund works under the hood, step by step.

<h3>What is Clr.fund?</h3>

<p>Clr.fund is creating a protocol for funding public goods projects in the Ethereum ecosystem. Using a combination of quadratic funding, zk-SNARKs, MACI, sybil resistance, and more, it aims to provide better, less biased funding for these public goods projects in Ethereum. In other words, it sorts the signal vs. the noise in public goods funding in the Ethereum ecosystem by allowing the strength of preferences to be heard by individual community members at aggregate.</p>

<h3>Why Clr.fund?</h3>
<p>Compared to traditional grants available (say by the Ethereum Foundation or Consensys) -- clr.fund is more accessible, has a quicker turn around between voting/allocating funding quadratically to projects and also receiving funding as a project. The cons of traditional grant funding can best be explained by Colony: 
"These [traditional grants] programs, while not ineffective, do have some shortcomings: they centralize decision-making power in a small grants committee; they are not self-perpetuating and require ongoing administration on the part of the protocol; they provide only a finite amount of funding to execute on a specific project: essentially a large bounty, not sustainable income, so the incentive to maximize the utility of the application is limited." (<a href="https://colony.io/budgetbox.pdf">Kronovet, Fischer, and du Rose 2018</a>).

Compared to Gitcoin -- Clr.fund is more hack-resistant.

In the past, attempts at quadratic funding have been more vulnerable to hacks, specifically collusion, <a href="https://flatoutcrypto.com/home/cryptointrosybilattack">sybil attacks/sockpuppet attack</a>, bribery, and many other attacks (<a href="https://colony.io/budgetbox.pdf">Kronovet, Fischer, du Rose 2018</a>). Using 1 person 1 vote (1p1v), people have not been able to signal the strength of their preferences to do with different project funding. Clr.fund seeks to solve for all of these issues by creating a protocol that is hack-resistant and has more effective voting and funding mechanisms (quadratic voting/quadratic funding). In other words, the deserving projects get the funds as allocated by the Ethereum community in a timely fashion. </p>

--------------------------------------------------------------
<em>Please note: I'm only going to go over technical terms that are relevant to the smart contract development, not economic terms like "public goods". These terms will potentially be defined in an upcoming release by the Clr.Fund Constitution for further clarification.</em>
--------------------------------------------------------------

<h3>Part One: What exactly are the top attacks?</h3>
<p>There are a variety of attacks relevant to peer to peer, permisionless, and decentralized applications, but we will focus on the main three that are particular threats clr.fund: sybil attacks, bribery, and collusion.</p>

<h4>Sybil-attacks</h4>
<p>A Sybil Attack is "an attack orchestrated by creating multiple identities and using them to gain undue influence" (<a href="https://en.wikipedia.org/wiki/Sybil_attack">Wikipedia</a>). This is a problem because clr.fund could get multiple users swinging the quadratic funding in one way or another so as to destroy the equitable notion of quadratic funding: to allocate funding to public goods projects that deserve it. So, sybil resistance is a prevention of these attacks by proof of uniqueness of each account. In Clr.fund, <a href="www.brightid.org/">BrightID</a> is used for sybil resistance by providing proof of unique identities.
<br/><br/>For more detailed information on Sybil attacks, see also <a href="https://flatoutcrypto.com/home/cryptointrosybilattack">FlatOutCrypto</a>. 
</p>
<h4>Bribery</h4>
<p>Bribery is when someone is given money to do something illegal or unethical or exercise undue influence. In clr.fund, the only foreseeable way for bribery to happen is for the coordinator of the funding round to be bribed. While the members of the community voting to allocate the funding do not have access to any of the messages for voting and cannot decrypt them, the coordinator can, which is where discretion needs to be in place. To counter this, clr.fund coordinators must be trustworthy by the community.</p>

<h4>Collusion</h4>
<p>Collusion as defined by <a href="https://en.wikipedia.org/wiki/Collusion">Wikipedia</a> is essentially a secret agreement to coordinate in order to deceive others. In clr.fund, particularly the worry is for funding rounds to devolve into a coordinated effort to get one project more funded than the others through devious means. This is where the MACI (Minimum Anti-Collusion Infrastructure) comes in, which I will explain in the upcoming "Part Two: Main Concepts of How Clr.Fund Works". 
<br/><br/>
For more information on Collusion, check out:<br/>
<a href="">"On Collusion" by Vitalik Buterin</a>. <br/>
<a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3243656">Buterin, Hitzig, and Weyl 2018</a> Section: 5.2 Collusion and deterrence. 
</p>

<h3>Part Two: The Main Mechanisms </h3>

Clr.Fund is made up of the following: quadratic voting, quadratic funding, zk-SNARKs, MACI (which uses edRSA and ECDH), and sybil-attack resistance. Let's dive into what each of these means and how Clr.Fund works. First, we'll start with quadratic voting and quadratic funding, then dive into the details of the technicality of how exactly we're implementing it at in a hack-resistant way at clr.fund. 

<h4>Quadratic Voting</h4>
 <p>In 2019, Colorado faced the same difficulty it had faced before: bills without priorities within the Democratic party that needed to be sorted through and voted on. This usually takes a very long time and is very expensive. However, last year, they tried a new approach created by Glen Weyl called quadratic voting. Quadratic voting's goal is to signal the strength of each individual's preference for -- in this case -- each bill to qualify which are most important to pass and which can go by the way side.
 <br/><br/>
 <strong> What is it? How does it work?  </strong><br/>
 Quadratic voting is a mechanism by which the strength of the voters preference is captured as well as the votes for specific initiatives.<br/><br/>
 Each person is given a number of voice credits. The square root of the voice credits equals the tallied votes per option. There are a limited number of voice credits allotted. Each person casts their voice credits towards an option out of X number of options.</br><br/>
   <em>Cost of vote = (number of votes)^2 (<a href="https://coloradosun.com/2019/05/28/quadratic-voting-colorado-house-budget/">Colorado Sun</a>)</em><br/><br/>
  The result is that options voted for get prioritized according to all the votes tallied by all the voters with their voice credits. The result in Colorado was that they were able to prioritize the bills in a more timely manner through anonymous quadratic voting (<a href="https://coloradosun.com/2019/05/28/quadratic-voting-colorado-house-budget/">Colorado Sun</a>). 
  <br/> 
  <br/>For more detailed information, here is the academic paper that quantifies quadratic voting in more detail: <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2003531">Quadratic Voting</a>. <br/><br/>
  For even more details, check out Glen Weyl's book: <a href="https://www.amazon.com/Eric-Posner-ebook/dp/B07TP5HLWQ/ref=sr_1_2?dchild=1&keywords=radical+markets&qid=1592406215&sr=8-2">"Radical Markets" in the "Radical Democracy" chapter</a>.</p>
<h4>Quadratic funding</h4><p>Based on <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2003531">Quadratic Voting</a>, it is a way of allocating funding based on the strength of preference of each individual within a community. The result "limit[s] the cost [of coordination and to] ...help protect against collusion" (<a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3243656">Buterin, Hitzig, and Weyl 2018</a>). Quadratic funding aims to "create a system that is as flexible and responsive as the market, but avoids free-rider problems"(<a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3243656">Buterin, Hitzig, and Weyl 2018</a>). It works like this, in general, each individual contributor, in a community, gets a series of votes on where the funding gets allocated within a community in proportion to the funding they contributed to the pool of funds available for various public goods projects.</p>

<strong>Why quadratic funding?</strong>
<p>Traditional matching funds such as in corporate or government contexts lack a "systematic design, and set the funding ratios and maximums in arbitrary ways" (<a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3243656">Buterin, Hitzig, and Weyl 2018</a>).  There is a complex mathematical analysis presented in <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3243656">Buterin, Hitzig, and Weyl 2018</a> for details on the mechanisms of funding. Quadratic funding works the same way regardless of the currency, groups can nothing from splitting or combining projects, and it doesn't matter how frequently the mechanisms is run (daily, monthly, etc.)(<a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3243656">Buterin, Hitzig, and Weyl 2018</a>). This makes coordination very flexible for each community based on how much funding they have available and how frequently projects need the funding allocated. </p> 

For more information, see: <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3243656">Buterin, Hitzig, and Weyl 2018</a><br/>

<h4>zk-SNARKs</h4><p>zk-SNARKs stands for: <em>"zero-knowledge Succinct Non-Interactive Argument of Knowledge"</em>. <br/>
What are they?<br/><br/>Mechanisms that do not reveal knowledge/information possessed by the prover, but proves the knowledge exists and is within the prover's possession such that the verifier can confirm it without receiving the information/knowledge itself.<br/><br/> In order to use a zk-SNARK, there are three parameters that need to be true: <br/><br/>
"<strong>Completeness</strong>: If the statement is true then an honest verifier can be convinced of it by an honest prover.<br/><br/>
<strong>Soundness</strong>: If the prover is dishonest, they can’t convince the verifier of the soundness of the statement by lying.<br/><br/>
<strong>Zero-Knowledge</strong>: If the statement is true, the verifier will have no idea what the statement actually is."<br/>
<a href="https://blockgeeks.com/guides/what-is-zksnarks/">Blockgeeks</a><br/><br/>

zk-SNARKs are used in Clr.fund in MACI (Minimal Anti-Collusion Infrastructure) integration to keep votes a secret in the voting process for projects from everyone except the coordinator, deterring collusion.<br/>

For more information about zk-SNARKs, see also: <br/>
<strong>Rosic, Ameer</strong>. "What are zk-SNARKs?: The Comprehensive Spooky Moon Math Guide". <a href="https://blockgeeks.com/guides/what-is-zksnarks/">https://blockgeeks.com/guides/what-is-zksnarks/</a>. June 22nd, 2017.<br/>
<strong>"What are zk-SNARKs?"</strong>. <a href="https://z.cash/technology/zksnarks/">https://z.cash/technology/zksnarks/</a>.<br/>

We will now put them in the bigger picture of deterring collusion among voters by diving into Minimal Anti-Collusion Infrastructure (or MACI) in the next post. Coming soon!</p>


<strong><h4>Further Reading/References*: </h4></strong> 

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


<p><small>* Not using any particular citation format like MLA, APA, etc. Just doing alphabetical order by title or author name. </small></p>