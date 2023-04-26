
# Decentralized Off-chain Backends: How Autonolas utilizes IPFS across its stack - David Minarsch

<https://youtube.com/watch?v=bzSjPTF-6G4>

![image for Decentralized Off-chain Backends: How Autonolas utilizes IPFS across its stack - David Minarsch](/thing23/bzSjPTF-6G4.jpg)

## Content

So as introduced I'll be talking about decentralized off-chain backends and in particular I'll first

introduce sort of what Autonomous is and how it gives rise to these kind of technologies

Then I'll cover how we use Autonomous today before I'll explain in detail how we basically

use IPFS across pretty much all layers of our stack
And finally I'll drive home how you and if you're a member of a DAO can benefit and give

a bit of a sneak peek ahead
So just one short slide on myself I'm one of the kind of original contributors to Autonomous

which itself is a DAO, an open source stack at the same time I co-founded an AI and crypto labs called VALORI and prior to that I worked at the same

intersection of AI and crypto and prior to that did a PhD in economics

So let's have a first look at what Autonomous is in particular what the software stack is

its protocol and its purpose and how it can basically be seen as a foundational network

for a term we call co-owned AI and in a context I'll also explain what I mean by that

So first I'd like to kind of explain in a bit more detail this idea of the decentralized

off-chain backend and in the context of Autonomous we give it a specific name it's called an

Autonomous Service And what you can do with this Autonomous Service Stack is build basically any kind of off-chain

application that takes arbitrary amounts of data from arbitrary sources be it web2 APIs

web2 like wrappers of web-free data or actual nodes participating in any sort of web-free

network That data then gets consumed by the service which can run continuously because it's running
off-chain and can do arbitrarily complex things and ultimately it can then take actions on-chain

as well as of course off-chain so if it does take action on-chain it can do so at the moment
on any EVM compatible chain as well as to some degree also Cosmos and Solana chains

but the way it's architected is that it's totally kind of blockchain agnostic as long
as there's a smart contracting layer it should in principle be possible to deploy it on such

a chain or targeting such a chain Now why do we call it decentralized because ultimately the software stack allows you to

run whatever application you're implementing here as a decentralized node system off-chain

So the way to think about it a bit is like a ephemeral blockchain for lack of a better

word Now having introduced this concept I'm going to basically change gear and focus on the

protocol first so the Autonomous Protocol is what lives on the blockchain and it ultimately

facilitates three things it provides a set of on-chain registries to represent different

components of these applications it allows for the tokenomics mechanism the incentive

and it allows for governance And so specifically today it's worth pointing out sort of two aspects of the protocol one

is kind of depicted here on the left which is how these autonomous services which live off-chain are secured on-chain so they're represented on-chain as NFTs and the service

itself takes action via its service specific multi-sec so ultimately the threat model looks

very much like you would something you would be known from a proof of stake system with

a two-third plus one honest majority
This system then can take action against any other on-chain contract you know be it a DeFi

protocol a governance contract you name it

So this is how we security services and then in terms of the developer experience of developers
building services on this protocol they don't necessarily have to build the whole service

they can build sub-components of it so let's say a developer develops such a component

they can then register this in the protocol and that of course adds additional functionality

to these kind of off-chain services which can be built and if someone in particular
a DAO uses these functionalities in their own autonomous service and wants to reward
the developers who originally contributed to the service then they can do so natively
in the autonomous protocol So the autonomous protocol ultimately has this incentive mechanism for developers to
bring these components which make up autonomous services and reward them for that

Now ultimately sort of before I kind of wrap on this autonomous piece and get into how

we use IPFS and the applications I just want to kind of give you a sort of sense of the

North Star which drives us or which we work towards it's this idea of having co-owned

AI programs i.e. AI programs or systems that are both co-owned where multiple parties own

the system as well as jointly operated so that there isn't a single party controlling

any aspect of the system and we refer to this as co-owned AI and ultimately we see sort

of autonomous as one of multiple ways in which you could build such a co-owned AI system

Now just to go from the theory into the practice I want to give a couple of examples of how

the stack is used today We have the same software architecture driving quite different applications across different
verticals in crypto and so on the left we have built with Balancer underground product

which is called Balancer Smart Managed Pools it is effectively an autonomous asset management
product where this off-chain system the autonomous service ingests data does some minimal compute

and decision making on it and continuously then reweights a pool which is on-chain a

pool of assets and so what this allows us to basically create an index product based

off-chain data On the other end of the spectrum we've got an automation product an autonomous keeper
service which is deployed in the keeper network protocol on Ethereum where it effectively

just works jobs so anyone who has an automation need can register there and the keeper system

will pick it up why is it interesting because basically it creates a sort of configurable

fault tolerance as to the job execution likelihood before we brought the autonomous keeper service

to the keeper network basically you had to hope that your job incentivized actors in

the network to work your job with the autonomous keeper service you can sort of configure that
because you can define how many keeper bots or agents you have in this network which are

continuously available and working jobs We have a couple of more examples here but I'll leave them for you to look up if you're curious in the interest of time One particular spotlight I would like to put on the governor though and that's both a lighthearted

experiment as well as one with very serious implications and so effectively what we've

done is we've created an off-chain system to which you can delegate your governance

tokens and you can endow the system with one of two very extreme voting preferences that's

the lighthearted part good or evil and the system will then take those into account to

effectively vote on upcoming proposals fully autonomously If you're curious about this like our twitter feed contains more information on this and I'm happy to share afterwards Now we obviously had an IPFS conference and I really want to kind of introduce how we're

using IPFS across all layers of the Autonomous Stack and that's where I want to spend the
rest of the time on So there's four key areas where we do that one is to reference and retrieve code components

The second application is to provide a production-grade package registry

The third application is that the way we reference code components also contributes to the crypto

economic integrity of the system and then the fourth part is that we use effectively

libp2p for peer discovery in the network which is established by these agents so that they

can find and communicate with each other So I introduce those in turn Starting with the packages in the Autonomous Stack and as mentioned before they're referenced

using CIDs so specifically what happens is a developer creates and publishes a software

package which is specific to our framework it's a Python framework at the moment although

for the protocol that's irrelevant
So the developer would create this package publish it in the remote registry and in this

framework we reference all code files all packages via CIDs and so this gives us this

nice dependency tree where we can say okay a given code package is content addressed

by this hash we can take this up to the level of the agent as well as to the level of the

service and so that then allows us to represent these elements on-chain to which we'll come

in a second in more detail via NFTs
This the important thing here to keep in mind is that this mapping is between the on-chain

part and the off-chain code is entirely optimistic of course so there's no way for us to verify

cryptographically on-chain at the moment that these packages are in fact what the registrant

purports them to be instead the protocol incentives the economic aspects of the protocol try to

align the incentives for the developers to represent them correctly

So coming to the second part because we have this on-chain representations as NFTs of off-chain

packages we can do a number of interesting things so firstly we can use this as a system

to track contributions to a given autonomous service so if let's say a developer contributed

various of those components into an agent or even the agent and the service is made
up of those then if there's any rewards effectively associated with the service NFT then they

can be allocated to the contributing parts which make it up

There's another very practical element of using this mechanism to reference packages

which is that we can basically point the CLI tool to the on-chain NFT and from that spin

up the entire service including all its configuration etc.
Which means that in an autonomous service where let's say we have four different operators
one running an agent instance each when they want to launch their agent all they have to

do is point the CLI to the NFT ID of this service in which they're operating in and

then the CLI tool will be able to fetch the metadata from the metadata fetch the code
build the source code and then run it this also helps us to a degree to prevent sort

of default malicious behavior in the sense by default the application is compatible and

malicious actors basically have to a fork the stack and bypass the use of the hashing

to ensure for the integrity as well as are then subjected to a slashing protocol which

comes on top to address the outstanding issues which can't be addressed any other way.

So this was the second part where we use IPFS the third part is in order to store all this

code remotely off-chain we have implemented our own effectively IPFS enabled registry

where developers can pin autonomous packages. Very practically the way this is implemented is as a cluster currently with three nodes although it's scalable based on demand where each node represents a pod containing both

an IPFS node and an IPFS cluster node which allows us then by the IPFS cluster functionality

to synchronize the IPFS data between the underlying IPFS nodes and so what we have as a result

is this very resilient and fault tolerant IPFS infrastructure for fetching these packages

and storing them. In fact we have also built a content verification layer on top which basically checks whether

the provided code is of the framework sorts and otherwise rejects these uploads.

There are some key learnings here. They're from one of our developers who worked on it and he's in our discord if you want

to reach out he was very excited working on this product and made a lot of optimizations
around the cluster configurations which overall really helped us reduce our networking ingress

and egress costs which were initially quite high with the default configuration so if

this is something you're curious about please reach out.
And then the final and fourth part where we use parts of the IPFS stack is in the agent

communication network. So first what does the agent communication network do? It effectively allows point-to-point communication between agents where these agents initially

the only thing they have is their wallet addresses. So agents all have a wallet so they have an account on let's say Ethereum and then there's

another one and they know that they want to talk to each other but that's all they've got. So they have no idea where in the network the public internet the other agent is located
and the role of the ACN is to establish that mapping. And so we've built this kind of custom application there which under the hood uses the libpeer2peer

library and effectively establishes a distributed hash table to allow for this mapping between

wallet addresses and agents location in the internet.

So that wraps the section on how we use IPFS and we are as you can see using it quite widely

across our stack and are big fans of the ecosystem. Please do reach out if you have any recommendations or questions.
And just to round off I want to re-emphasize that DAOs can benefit and entities wanting

to build DAOs can benefit from our stack today. Our stack allows you to decentralize and make actually autonomous your off-chain processes.
A lot of DAOs have quite centrally operated fault intolerant processes off-chain so that

might be the stack for you. It is a fully open source stack so you can fork it or contribute to it and it allows
for high composability and code reuse. So all the applications which we saw before share a tremendous amount of the same even

application logic which is one of the big benefits of the stack.

These days we are mostly working on slashing which is coming live still this quarter and

we are exploring how DAOs can use their own governance tokens to effectively secure such

autonomous services to operate their own off-chain infrastructure.

And that wraps my presentation. If you have any questions feel free to ask.

Q. Could you please tell me, you mentioned DAOs operators as users of autonomous. Do you focus on any other users?

Yes, it's a good question. We have basically an autonomous service has sort of different user group attaches. So there's the entity which could be like a single person or a DAO, a smart contract who owns the service.

So they effectively have like the rights to turn it on and off again effectively and configure. There's the operators, the entities who are running the different nodes in the service. And then there is the beneficiaries of the service.

All three could be the same or could be different entities. So in a DAO, for instance, in an autonomous DAO we have some of those services which serve the DAO itself. The beneficiaries might be community members in this case.

And the operators are DAO members and the actual service owner is the DAO governance contract itself. Does that answer your question?

Thank you. Q. A question about the workings. Is it like that you deploy a service or a running autonomously running script on the P2P network, which listens to a blockchain and then does an action? Or is like the triggering coming from the contract on the blockchain and that calls like that?

Yeah, it's a great question. Thank you. Let me go back to one of the slides just for ease of reference.

So the autonomous service is its own node system and it is running off chain. So in the default sense of the ecosystem, if you come to it today, you would run your own autonomous service.

And so then you would decide how many operators there are. Probably initially you would want to be like the single operator and the single owner. And so then specifically, where are the events created in a sense?

So you have two parts here. So one, the service is continuously running. So it's not a smart contract on a chain, which only waits for transaction and then gets executed.

It is a continuously running process off chain. In fact, a number of them coordinated.

And so there's sort of two ways it could make progress, so to speak. One is it could listen for events either on some Web2 API or on a Web3 node, like an on-chain event.

And then that could trigger something in the logic of the service, which maybe leads to a new transaction or something internal.

And the other thing, and that's the key thing, is you could have proactive behavior of the service itself. So that's where really the autonomy comes from.

By proactively, basically, maybe on time or on internal states of a model it has of the world, it can take action.

And that's very different, obviously, from a smart contract on chain, because it can simply not do that. It needs to wait for transaction. Does that answer your question?

So, and when you say off chain, that could be my laptop or it could be multiple peers on the network.

Yes, a great question. So the deployment mode is up to the service owner. So you can run often the same application, like just locally as one agent instance on your laptop, let's say, or on a cloud server you provision.

And then once you want this kind of benefit of default tolerance of maybe decentralization by adding more operators, you would scale up the number of nodes.

And then they can run in different configuration as long as they have access to the public Internet and a static IP allocation and so on.

Does that satisfy your question or does this answer your question?
Yes. Okay. Thank you, David. It was a great presentation. Thank you very much. Thanks for having me and I hope you have a nice rest of the conference.

