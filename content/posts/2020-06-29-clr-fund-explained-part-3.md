---
template: post
title: Clr.Fund:Explained (Part 3)
slug: clrfundexplainedpartthree
draft: true
date: 2020-06-29T17:32:48.733Z
description: "This is a post about how MACI fits into Clr.Fund's MVP and how
  Clr.Fund works. "
category: Raid Guild
tags:
  - Solidity
  - Clr.Fund
  - Ethereum
---
Here is a technical diagram of how Clr.Fund works: 

![MermaidLink](https://imgur.com/M5bAC0S.png) 

Here is the code that created that diagram: 
```
sequenceDiagram
    participant Owner
    participant PoolContributor
    participant Recipient
    participant Factory
    participant MACIFactory
    participant Contributor
    participant FundingRound
    participant MACI
    participant Coordinator
    
    Owner ->> MACIFactory: deploy MACI factory
    Owner ->> Factory: deploy factory
    Owner ->> MACIFactory: transfer ownership to Factory ( call transferOwnership() )
    Owner ->> Factory: set MACI parameters if necessary ( call setMaciParameters() )
    Coordinator -->> Owner: provide public key
    Owner ->> Factory: set coordinator ( call setCoordinator() )
    PoolContributor ->> Factory: donate to the matching pool ( call contribute() )

    loop Adding recipients
        Recipient -->> Owner: ask for registration
        Owner ->> Factory: call addRecipient()
    end
    
    loop Every round
        Owner ->> Factory: call deployNewRound()
        Factory ->> FundingRound: deploy FundingRound contract, transfer funds from matching pool
        Owner ->> Factory: call deployMaci()
        Factory ->> MACIFactory: call deployMaci()
        MACIFactory ->> MACI: create new MACI instance
        Factory ->> FundingRound: call setMaci()
        
        loop Funding
            Note over Contributor, FundingRound: contributor registers with their BrightID
            Contributor ->> FundingRound: donate to the pool ( call contribute() )
            FundingRound ->> MACI: call signUp()
        end
        
        Note over FundingRound, MACI: contribution / signup deadline
        
        loop Voting
            Note right of Contributor: contributor creates message
            Contributor ->> MACI: vote ( call publishMessage() )
        end
        
        Note over MACI: voting deadline

        alt Tally votes
            Note left of Coordinator: coordinator generates proofs
            Coordinator ->> MACI: process messages ( call batchProcessMessage() )
            Coordinator ->> MACI: tally votes ( call proveVoteTallyBatch() )
            Coordinator -->> Recipient: provide vote tally
            Owner ->> Factory: call transferMatchingFunds()
            Factory ->> FundingRound: transfer matching funds and finalize the round

            loop Distribution
                Recipient ->> FundingRound: submit vote tally and a proof ( call claimFunds() )
                FundingRound ->> MACI: verify the proof
                FundingRound ->> Recipient: transfer funds
            end
        else Cancel round
            Owner ->> Factory: call cancel()
            Factory ->> FundingRound: call cancel()

            loop Withdrawal
                Contributor ->> FundingRound: call withdraw()
                FundingRound ->> Contributor: transfer funds
            end
        end
    end

```

So, what does this mean? 
Let's break this down sequentially. We have an Owner, PoolContributor, Recipient, Factory, MACIFactory, Contributor, FundingRound, MACI, and a Coordinator. 

## The Owner

The Owner is the instantiator of Clr.Fund. The Owner deploys teh MACI factory, deploys the Factory, transfers ownership to Factory, sets the MACI parameters (if necessary), and is to whom the Coordinator provides their public key. Once the Owner has the public key of the Coordinator, then they set the coordinator. 

## PoolContributor

The PoolContributor is someone who donates to the matching pool of funding for allocation after voting takes place. This happens after the coordinator is set up with their public key.   

## Recipient
The Recipient(s) are the ones who receive the funding once the voting is done. Adding a recipient can only be done by the Owner by calling ```addRecipient()``` on the Factory. 

## Factory
The Factory is the generic vehicle through which a funding round is started. It is used to deploy new rounds of funding, deploy MACI, set MACI, call transfer the matching funds, transfer the matching funds and finalize the round, as well as potentially cancel the round. 

## MACIFactory
MACIFactory is a vehicle for creating replicable instances of MACI to be used for the funding round that is created. It is instructed on Owner's calling of the ```deployMaci()``` function in the Factory to the MACIFactory to create a new MACI instance. After this instance is created, the Factory contract calls ```setMaci()```. The MACIFactory is deployed first by the Owner before any other actions are taken. 

## Contributor
After calling ```setMaci()```, the Contributor donates to the regular pool for the funding round by calling ```contribute()``` function on the FundingRound contract. After the contribution is made, the FundingRound creates the sign up deadline for other contributions to be made. A contributor votes /creates a message by calling ```publishMessage()```. Voting proceeds until the voting deadline. 

## FundingRound
The FundingRound contract is used to set MACI parameters, for contributors to donate to the pool of funding (not the matching pool), to sign up users,transfer matching fund and finalize the round, as well as submit the vote tally and a proof (```claimFunds()```), and tells MACI to verify the proof given by ```claimFunds()```, then transfers the funds to the Recipient. 

## MACI
MACI is in charge of the signing up of users/voters for a funding round, voting, processing messages/votes, tallying votes, and verifying the proof that is created by ```claimFunds()``` in FundingRound. 

## Coordinator
The Coordinator is in charge of providing their public key to the Owner for registration in the funding round as a Coordinator. They provide the vote tally after processing the messages/votes and tally the votes after the voting deadline has passed. They provide the Recipient with the voting tally. 

## How does one setup a funding round? 

## How do people contribute and sign up? 

## How do the votes get processed and tallied? 
