
# InterPlanetary Consensus (IPC): Adding a consensus layer wherever is needed - Alfonso De la Rocha

<https://youtube.com/watch?v=qPLhqPGDZyk>

![image for InterPlanetary Consensus (IPC): Adding a consensus layer wherever is needed - Alfonso De la Rocha](./InterPlanetary Consensus (IPC) - Adding a consensus layer wherever is needed - Alfonso De la Rocha-qPLhqPGDZyk.jpg)

## Content

and so on. So, actually, these should be a fast, I mean, I was planning to attend there

in person, then some personal matters happened. I was supposed to do a workshop and I was hoping to show you IPC in, like, working, so that everyone had a chance to see what is this

IPC. But what I'm going to do now is try and pitch the system, get this in the back of your mind, and hopefully in the future we can get in touch. Because, like, right now,
what we're trying to do with IPC is getting into production, now that FEM is out, and the idea behind IPC is a way of scaling, so adding consensus to existing distributed systems

and having a way of scaling Falcon. And in this talk, my idea is to give you a glimpse

of what is the system, and hopefully we can have follow-ups and see if we are a match to your projects or whatever you are doing in the scope of computation or any other system.

Really briefly, like, where does this IPC idea come from? Well, with Web3, what we're

trying to do is decentralize a lot of the critical systems that we have out there, like, for instance, the internet, computation as many of the projects in this track, and one

of the core modules of Web3 are blockchains, and unfortunately, there's a key part in blockchains
which is the consensus layer, that it has some limitations. If we look at today's technology,

we go to the, like, OG systems like Bitcoin and Ethereum, what we see is that we will
never be able to scale them, at least in their current state, to the kind of loads that we have in Web2 and the kind of loads that the projects around computation, like the ones you've been pitching in this track, need, because we have around, like, 7,000 and a few thousand transactions per second. But even if we go to these, like, next-generation blockchain systems, where supposedly they are able to reach up to the thousands and

millions of transactions, there's also a limitation in the consensus layer, because in the end, we are limited as they sequentialize every operation and every transaction, they are limited by the specifications and by the performance of the validators running that consensus layer.

So the idea behind IPC was to try and achieve these kinds of goals. So in the end, we want
to start accommodating the Web2 loads into Web3 as we advance the technology. So we want

around the giga and the tera transactions per second of throughput in our systems and have consistency and, like, security over these systems with this amount of load. Ideally,

I think that this is something that not every blockchain approaches, which is fast, optimistic, local finality. So right now, we're seeing that in order to run a transaction, generally, I mean, there are caveats, because some projects are building workarounds around this, but
generally, it's hard to achieve below one second transactions, even when the validators are in the same data center. The idea is that some use cases may need, like, fast finality
or at least fast optimistic finality, and we'll be able to settle the more, like, secure

requirements further up, but we cannot fine-tune the... There's no consensus that this one

size fits all to every application. So the idea behind IPC is to not only accommodate this kind of Web2 scale loads, but also be able to fine-tune the underlying consensus

algorithm and the underlying blockchain to fit the needs of our applications. And along with this, we want to achieve, like, censorship resistance, partition tolerance, secure global finality, the kind of things that other blockchains are trying to tackle. And the way that IPC achieves this is through horizontal scaling. So the same way that we horizontally scale
our AWS instances whenever we have more load, the idea is that let's... Our idea was let's build a framework where instead of, like, explicitly partitioning the state or explicitly
partitioning how transactions are validated, like in many other sharding proposals, let's
try and build a framework that allow users to deploy horizontally the kind of blockchains
and the kind of consensus layers that they need. And this is the idea behind IPC. So

it's an on-demand horizontal scalability for Filecoin, where what we're trying to do is to build a hierarchy where users are able to deploy their new, let's call it blockchain
substrates or consensus substrates, that anchor their security to the upper layer networks.

Like for instance, Filecoin. So it would be kind of like building a layer two or layer

three or a recursive secondary layers of blockchains where instead of having an out-of-the-box

blockchain, you would be able to fine tune the blockchain to what you need. So in the
end, you will be able to choose your consensus algorithm, choose your security trade-offs, because as I mentioned, like, there's no easy way to find the best, like, trade-off for
every kind of application. Like some computation systems may not require that much security,
but they want to go fast. And then if you're doing a DeFi application, you may want a lot of security and you don't mind if finality is a bit lower. So that's the idea behind IPC, to give a framework that allows you to do this kind of stuff. And to give you a high
level overview of how is this being deployed, let's consider the Filecoin network, Filecoin

mainnet, which is currently our, what we call our rootnet. So it's our anchor of trust from
which all of the subnets are going to be spawned and from which we are going to start building

our new layer twos and layer threes recursively. So Filecoin has a 30 seconds block time and
like many more blocks of finality, which makes it not great for certain applications, especially
on unit like fast finality and fast block times. So the idea is that with IPC, you will

be able to deploy new networks that are able to interact seamlessly with the states in

the Filecoin network, but go a bit faster and like do local operations faster. So this
is the case of Spacenet. In Spacenet is one of our, so it's our test network where we're
deploying IPC and all of the innovations that we're doing at ConsensusLab. And this is a test net that is exactly like Filecoin, but with a new consensus algorithm that is called Mir and that allows us to have one second block times. So in this way, someone that
is trying to have like high performance and have a blockchain that goes really fast and
with fast finality, they will go to Spacenet and with Spacenet, we will be able to still interact with states in Filecoin. But the idea is that we wouldn't like one second finality with a few dozens of validators may not be enough for a certain application. So they
will be able to deploy, for instance, their subnet, like spawn a new subnet from Spacenet

that anchors their security to Spacenet and recursively to Filecoin and build their application
like that. So the idea is in this way to be able to build a hierarchy of different kinds
of blockchains that anchor their security and are able to interact seamlessly between each other and deploy the consensus layer and the blockchain substrate to the needs
of their application. And this is, sorry, and just to give you a bit of technical details,

how is this powered is that there are two user defined FVM actors that are the ones that handle all of the interactions between all of these blockchain substrates and handle
all of the interoperability, anchoring the security, exchanging the messages and so on.
So how would this work? If we have a rootnet and we want to deploy a new subnet, the first

thing that we do is we request the deployment of a new subnet in the rootnet. We see here

that we will deploy an actor that governs the policy of the subnet and this will trigger in its own the different subnets and this is something that we can do recursively. As I said, this is run by two main IPC, sorry, by two main FVM actors and so there's a lot

of on-chain logic between all of these actors and then there's an off-chain, let's call it
peer implementation or process, that is the APC agent, which is the one that orchestrates
the exchange of messages between the different blockchains. So for instance, if you have an application that wants to interact with the subnet and with the mainnet and wants to interact with several of these networks to implement their operation, they will interact with an

IPC agent that is the key off-chain component that knows how to speak the IPC protocol and

knows how to speak to the different subnets and the different rootnets that are part of your application. So you define in your config what are the subnets in the system that you want to interact with and it will abstract for you all of the operations and when you want to, I don't
know, put more collateral, if you want to send a message, if you want to send a crossnet message, it handles for you all of this back and forth with the underlying blockchain substrates.

And these subnets that I'm mentioning in the end is just a fork of Lotus. So when I'm

saying that we deploy a subnet, the peer implementation of each of these subnets is
just a fork of Lotus with a modified consensus algorithm and some other modifications. But in the end, it's the same kind of good old stack that we're used to. So we use IPLD for the

data store, we use FEM for all the runtime of the blockchain, and all of the types, the
types and the semantics of the blockchain that the subnet substrates are the same as we see in Filecoin. And this is really interesting because it means that we have subnets and content address

blockchains as we would have currently in the Filecoin mainnet. Yeah, and as I mentioned, if you want to start giving a try to IPC, I will tell that, I mean,

the entry point is this APC agent, which is the off-chain process that handles all of the

interaction with the different subnets. So if, for instance, we want to have an application that requires these three levels of subnets, you would have a full node replica for each of these

peers that is syncing with the network, and then your IPC agent will abstract all of this complexity of interacting with the different blockchains and will give you all of the operations that you need to handle all of this complex hierarchy.
And all the state of these networks. And to leave you this as a food for thought,

when and how can IPC be useful? So if you need consistency in your data or in some operations

that have happened in a distributed system and you don't have a way of handling this, because if you go to Ethereum or some other slower network, it makes no sense. So this can be
a good first use case for IPC. Or if you have this problem, the use case, like IPC may help you

solve it or at least tackle it. Then we have, for instance, use cases that require faster finality

and higher throughput. So I guess some of the builders are having some trouble with this 30 second types of Filecoin mainnet. So the idea is that when we release IPC, you will be able to deploy your subnet and have a more consistent and more efficient way of handling the different
types of files. You will be able to deploy your subnet and have faster finality and faster block times if you need so. Also, like Spot, in the end, you saw that I didn't go into the details,

but deploying a subnet is just deploying an actor, an FVM actor over a network, which means that it's

kind of cheap to deploy your own subnet and try a few things. And you can make subnets ephemeral. And it's really low barrier and low overhead to deploy a subnet. And this is going to be like, we want to have ephemeral subnets with a small number of members that want to run some
kind of job or agree on something and then report the result into the Filecoin mainnet or whatever

rootnet that they're using. And also, it's really interesting when you need verifiable computation
or incentive mechanism. Because in the end, with this consensus layer, you can define all these
semantics, and you can define how a set of members in a distributed systems need to agree on

something, all leveraging the consensus layer that we're giving and the FVM as a runtime.
And there's a bunch of resources that I share here where you can start learning about IPC, because I just had 15 minutes. I didn't know how to put all of the knowledge forward. But we already have Spacenet, which is the IPC testnet where you can start tinkering with deploying your own subnets. You have a bunch of design docs and analysis around all of the research that we did

to deploy the system. And if you have any kind of question, we have these Filecoin Slack channels,
like Consensus, IPC, and Spacenet, where you can ask us questions, and we can guide you through starting to use IPC. So IPC is currently without crossnet messages in Spacenet. And by the end of

this week, hopefully, we will have also crossnet messages. So between the different subnets, you will be able to interact seamlessly by sending messages to some smart contract that may leave in

some other subnet. And if you want to run, as I said, the IPC agent is your entry point to IPC.

In the readme, you can see a lot of getting started tutorials and some notes on how to run

your first IPC subnet. Both the docs and the tech is still a bit rough because it's quite new. So

let's say this is a bleeding edge. There may be a lot of potential bugs or rough edges that need to
be polished. So feel free to open issues, to directly ping me if you have any problem, and
let's discuss how we can improve the tech. This is the roadmap to give you a high-level overview.
So we're here in the M1 – sorry, now we call it M2 – but here in M2, where IPC is going to be

deployed on Spacenet, this is happening. So we had the first phase of this in the end of March,
and this is happening this week. So hopefully, from this week, you will be able to test in the Spacenet testnet, deploying new subnets. And we are aiming for Q2 or beginning of Q3 for the

deployment of Spacenet in Fibonacci mainnet so that you can start testing subnets not only in testnet but also over the actual file code. And that's all from my side. Sorry about the

equations. I was hoping to have a bit more time to introduce IPC, but this is just like – take
this just as a pitch for you to at least know what IPC is, and feel free to just ping me if you want

any follow-up or you want to discuss this thing. Thank you very much. And yeah, if there are questions, we can probably take them, I think. So I had one question. Could you please tell me,
you mentioned that verification is faster than 30 seconds. Do you have any numbers?
So the reason is because we are – I mean, in the end, we're running a BFT-like consensus,

right? So you can go as fast as the rounds of BFT that you need to do. So it doesn't depend

on the amount of computation that you need to do. It depends on the latency and the delay,
the RTTs and the number of messages that you exchanged with the rest of validators. So right now in Spacenet, we have a dozen validators, and we have less than a second
block times because in the end, it's just the time of executing the – so it's like any other

consensus algorithm in a blockchain. You run the block, you trigger the state changes, see if they are consistent with the rest of the network, and if they are, you can go.
So it's more of a distributed system. Thanks.

