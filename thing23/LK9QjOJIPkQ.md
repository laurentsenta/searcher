
# FVM: The Filecoin Virtual Machine - Matt Hamilton

<https://youtube.com/watch?v=LK9QjOJIPkQ>

![image for FVM: The Filecoin Virtual Machine - Matt Hamilton](/thing23/LK9QjOJIPkQ.jpg)

## Content

Hello, good morning everybody.

And yes, we start this track a little bit earlier than the other ones, so hopefully people are awake here a bit. Dieter is already asleep. My name is Matt Hamilton. I'm a developer advocate with Protocol Labs, working specifically on FVM, the Filecoin
virtual machine. So in this talk today, I'm going to talk a bit about what is FVM and how FVM works

and the architecture a little bit on that. And then I'm going to move on to some demos. And I'm going to demo some stuff on the public testnet, Hyperspace, and then also I've got
some other stuff that I'm lining up to demo for you. So let's start then. So FVM delivers on-chain programmability to the Filecoin network.

So how many people here have done anything to do with, say, smart contracts on any other

blockchain? Put your hands up if you've done anything to do with, like, deployed a smart contract or whatever. Two or three people, so that's about, yeah, two, three, four people.
Okay. So how many people here think they know kind of like how Filecoin works and the deal, storage

deal flow? Thinks they could give an explanation. Luckily, the guys from Boost put their hands up. That's good. That's great. But okay, again, only a few people. So hopefully there's stuff to learn here from this talk.
So the Filecoin master plan is three stages. And really this whole track today is about this third step, which is bringing compute
to the data. So we've created the world's largest decentralized network, onboarding and safeguarding humanities data, and we're now working on the aspect of bringing compute to that data.

FVM was launched a month ago on the 14th of March.
For the Americans, that's 314, aka Pi, Pi Day, and the chain ID of Filecoin network

is actually Pi as well. It's 314, and all the testnets have a successive digit of Pi added to it.
So it was a month ago. There's already about 400,000 filled for the Filecoin token on the FVM, and lots of contracts

deployed, and we've got an initial ecosystem, batch of partners building on the network

as well, as well as what we call the mainnet cohort. So we had an early builders program of people learning about FVM before it launched and
building on it, and now we have this mainnet cohort of people building on it as well.
As we launched, the very first contract that was deployed was a commemorative NFT of the

FVM team. So we have some other people, not all of them, but some of them that have been involved in actually building and delivering FVM over the last about two years.

So FVM solidifies Filecoin as a layer one network for powering the open data economy.

So one of the things that... So Filecoin will also... Obviously, we're here at an IPFS conference. IPFS, Filecoin, both decentralized networks. The one element that Filecoin adds to the picture is an incentivization model for storage.

So storage providers are actually rewarded for storing data, and there's actually a crypto economic model built into the network to not only reward people for storing data, but also

there's an integrity part there as well, that you can actually prove that the data is actually
stored as well. So with IPFS, you have the assurance of the integrity of the data from the CID to the

data. Once you get the data down, you can check that that is actually what you intended the data to be. But with Filecoin, it then goes a step further and there's actually further assurances on the actual availability and storage of that data. But one way to think about the two of them is if you think about Filecoin as being more
aimed at the moment, at least, towards archival and cold storage of data.
Whereas IPFS is more towards instant retrieval, sort of hot storage of data. Those are where they are kind of at the moment. Over time, that'll probably merge, and there's tools that are making these differences a lot less visible. So tools like LASI allow you to retrieve data from either IPFS or Filecoin, and you might

not necessarily notice the difference between them. So FEM allows you to orchestrate kind of how, where data gets placed, allows you to do things
like incentivizations around it. So you can build things like data DAOs. A DAO is a decentralized autonomous organization. You can have a data DAO in which maybe you have a number of participants that are sort of members of this smart contract that collectively fund the storage of data and maybe vote on

the storage of that data, for example. I'll give some more examples going forward. I'm already at the stage of this conference where I'm starting to lose my voice. So please excuse the slight raspiness here. But yeah, there's a big market out there in the open data economy. And FEM is one of the critical upgrades to Filecoin at the moment because it opens it

up to a much wider audience. So we're bringing in everybody that's able to develop on top of the likes of Ethereum
and Polygon and other EVM based smart contract platforms onto Filecoin as well.

And again, I'll explain that in a bit more detail as we go forward. So the FEM architecture. So FEM is a polyglot virtual machine. It is written, well, it executes WASM web assembly. And the idea being is that you can then build additional runtimes on top of that. So the first runtime that we've launched is F-EVM, the Filecoin Ethereum virtual machine.

And that's what I'm focusing on in this talk. But there's also a bunch of user defined actors that control the actual operations of the

Filecoin network itself. Sorry, built in actors that control the operation of the network itself. And then there'll be the ability soon to deploy your own actors at the WASM level as well
in the near future. And so, like I said, for the first time, it allows developers to actually interact with

the crypto economic model and the marketplace of Filecoin and actually write logic around
how and where and who is storing data on the network. So they can write this in Solidity, which is the language used for Ethereum development.
And you can then take exactly those smart contracts that have been developed for say Ethereum or other EVM compatible blockchains, and then they get run on F-EVM, interpreted

and run on the WASM runtime. Now the way we do this, we have an Ethereum RPC or Ethereum compatible RPC.
So Lotus, which is the reference implementation of a Filecoin node, actually implements the
Ethereum JSON RPC API. So what this means is that to all the developer tools, Ethereum based developer tools, the
Filecoin network now just looks like Ethereum to them. So you can use the existing tools, hard hat, remix, and again, I'm going to give some demos of these in a bit, to connect to Filecoin and interact with the Filecoin network.

There's a number of built in actors, like I mentioned, and these control the functions of Filecoin. So things to do with rewards of tokens. So like I mentioned, the Filecoin network has a crypto economic model. If I want to be a storage provider and store data on the network, then I have to put some

collateral up with respect to the storage capacity I have. I'm then paid to store data kind of over time, and that payment is actually released over

the duration of the storage deal. And if I lose the data, I lose that stake that I put up as well for that storage deal.

And there's other actors there that deal with things like the market itself and what is

stored where, how much power a miner has, which is based on how much storage capacity

they're bringing to the network. So the Filecoin network, it's about 15, 16, I think it is, exabytes of data.

So it represents about 1% of the total data center storage capacity. It really is a truly massive network.
So any developer can deploy smart contracts to Filecoin. We've got some examples here, things like perpetual storage of NFTs.
So you might have a NFT collection, and you could have a smart contract that is paid via

like an endowment. Maybe the smart contract itself has some sort of investment capabilities, and it then renews

the storage deals as they come up to expire. So storage deals on the Filecoin network last for, I think it's about between six months and two years, roughly. And when a storage deal comes to an end, a smart contract could then automatically renew that. Data DAOs, I've mentioned, you might have people participating who then voting on what gets stored, how it gets stored. An example, I was at ETH Denver, and somebody came up to me and said they're working with a film festival. And I was thinking that could be an example thing. You could have something like a film festival. The films are stored on Filecoin. You could actually have voting represents the competition element of the festival as

well in terms of winners of the best film or whatever it might be. And that also determines things like storage. And then you've got economic things like staking. So I mentioned at the start that with Filecoin as a storage provider, you've got to put up
collateral. Now that's quite a big barrier to entry to put that collateral up. Well, now bringing kind of DeFi into the network, you could actually have storage providers
have the ability to borrow funds in order to do that. So as individuals say, holders of the fill token, we could stake that token on the network

and earn a interest for doing so. And that can then be borrowed by storage providers to set up their business, basically.

And what's interesting is that because of the nature of Filecoin and smart contracts
and that, the smart contract can actually verify that the storage provider has the capacity
they say they have and potentially look and see what future revenue that storage provider
has as well. So a bit like say invoice factoring in traditional business, you could actually have a capability where a storage provider could borrow against future revenue because that revenue can actually
be validated on chain on the network. And you can actually see the storage deals and that storage provider has a certain number of deals over the next year or two years that they will be paid for.

So what sort of things are people building on FEM so far? So we've got various things. We've got, I mentioned perpetual NFTs. So NFT storage, for example, are working on a project called NFT Forever, I believe it is. So they're using that. Glyph, these are the people that are behind one of the main Filecoin wallets. They've got a staking pool that they're working on, Glyph pools. And again, that allows you to deposit a fill and that'll then, there'll be a lending market for storage providers. Saturn, they're using FEM. They've got a project called Retrieve. They're using FEM4 and trying to move some of the more centralized elements into sort

of slightly more decentralized trustless elements by using smart contracts on the network.

DeFi staking and loans. So fillet and collective DAO, both examples again of ability to kind of stake Filecoin

token and earn rewards. Lighthouse, they're also a perpetual storage system as well. They've been using other blockchains for the perpetualness and they're now able to implement that on Filecoin and have more access to the data about the storage deals, which means

that gives them a bit more control over that. Again, DataDAO, Lagrange and SpendDAO, they are both to do with decentralized science

and storing like public datasets. So you can imagine a research organization having some kind of way in which they could store large research datasets and have the funding for that in place to fund for that
continual storage of data. And another example that you'll hear more about later on. So Bakkiao have a project, Bakkiao's big decentralized storage platform. You'll find out more from Wes and Irina, I think later on. And they've got a project called Lilypad, which is a bridge between FEM and Bakkiao.

And they'll be giving a demo of, I believe Waterlily is it, which is an application on top that allows you to run stable diffusion jobs on Bakkiao and have the resulting images

stored as NFTs on FEM. So how do people go about this? Like I said, what we do is FEM looks like an Ethereum network to most tools.
So things like Metamask, which is one of the most popular wallets that people use for interacting
with Ethereum. You can use that, you can configure Metamask and I'll show you that in a bit to connect to Filecoin. Remix, again, you'll see this in the demo. There's an IDE for developing Ethereum solidity smart contracts.
Wide hat is another tool, and again, I'll show that for deploying and compiling smart

contracts. So a couple of things before we get on. One other thing to note, and you'll see this a bit later on, that Filecoin has a number of different what are called address classes. So each account, each wallet has an address, generally starts with an F, or if you're on a test network, starts with a T. And most wallet accounts you'll see start with like F1 or F3. If you've got an account, a wallet, and you receive a fill, for example, your account probably starts F1 or F3. FEM brings in the ability to have kind of foreign addresses from other runtimes and

other blockchains. And so we've developed this additional address class F4, that is a delegated address class.
And so there's what's called the Ethereum, in this example, the Ethereum address manager that sits at address 10. So anything that starts F410, the rest of the address is actually translatable into
an Ethereum address. So Ethereum addresses that start 0x can be directly translated to a fill address that
starts F410 and back again. So this means that we can translate an Ethereum address, sorry, a Filecoin address to an address that looks like an Ethereum address, so all the Ethereum tooling is happy with it, right?
But you'll see why that's necessary in a bit. And we have a series of libraries, Solidity libraries, that give you access to that underlying

Filecoin functionality. So those inbuilt actors that I mentioned before, so there's some libraries that kind of give

you a slightly higher level access to those actors, things like types that are used as
well within the network. So good, how are we doing? Great. On to the first two demos. So this first demo is going to be on Hyperspace, which is the testnet, the public testnet for

FEM. And I'm going to test and deploy NFT. So I'm going to upload an NFT and its metadata to NFT storage. So it's then accessible via IPFS. I'm going to show you configuring Metamask to connect to the Hyperspace network.
We're going to use OpenZeppelin, which is a repository of existing Solidity smart contracts,
and they've got a wizard that allows you to easily create a smart contract that represents an NFT. There's a standard called ERC-721 that's used for that. We're going to deploy that to Hyperspace using the Remix IDE, and we're going to mint an NFT to an account. And then, because Dietrich's here, I'm going to import it into Brave Wallet and show it in Brave. So I made lots of sacrifices to the demo gods last night, so hopefully we're going to go

well. So let's start off here. So first of all, configuring the network here.

So there's a site called Chainlist, chainlist.org, and you can go on there and you can search for Filecoin. And if you include Testnet, you'll get a list of networks. And there's the Filecoin Hyperspace Testnet. And you can click Add to Metamask. If you install Metamask, which is a little browser-based wallet, then it will configure
itself automatically there for you. We can actually go and see in here the networks, and we can actually see our networks here.

So Filecoin Hyperspace, and you can see we've configured it here. There's this RPC URL, so that's where we're connecting to actually submit the commands.

The chain ID there of the Hyperspace Testnet, the currency name, and a URL for Block Explorer.

So I've done that already. So we've got Metamask account set up here, and I have some funds in it, 250 T-FIL.

I got those by going to the Hyperspace faucet, hyperspace.yoga for some reason.

If you click on Faucet, you can put in your address there and click Send, and it will
send you some funds to the account. So if I copy my address, put it there, prove that I'm a human.

These seriously get harder and harder every day. Yeah, yeah. Chicken or croissant. Wow. I don't know what version. I think they're using just like a particularly hard version on this site for that. Anyway, that's being sent. So I mean, I've already done it, so I've already got some T-FIL here in the account, but we'll see next time I bring up the wallet, you'll probably see that's jumped up some more. It seems to be delivering 250 T-FIL per go. It was originally about five, so they've bumped it up quite a bit now. So we'll probably see that jump up to sort of 500 or so. So we've got a wallet. Our wallet is connected to the Hyperspace Testnet.
So now we need our smart contract for this.
Actually, no, first of all, I'm going to upload the data to NFT Storage.

So I've actually got here a quick script that was taken from, I think, NFT Storage's website.

It's in JavaScript that basically takes some metadata information and the name of an image

and that will then store that.

on NFT.Storage so it will be stored on IPFS and what's nice about it is that it will store the image as one IPFS item and then it will store the metadata as another IPFS item and link the two together and come back and tell me where it is.

So that's where it is now and I've loaded there's an image there IPFS thing and you're going to have to guess what that is. You'll see what it is in a bit. So that's the address of where it is there. So we'll need that in a second.

So first of all, I'm going to open Zeppelin's contract wizard. It shows an ERC721. I want it to be mintable, have auto-incremental IDs and use a thing called URI storage, which means I can tell it where the asset is. I'm going to call this token IPFS thing.

And there we go. So I'm going to then click open in Remix and that's going to open it in Remix, which is a web-based IDE. This is something that's very familiar to people that develop on Ethereum. Like I said, it's the same kind of technology and we've got our contract there ready to deploy.

I can click compile. It will compile that. And now to deploy it, I need to just tell Remix where I want to deploy it to. I choose injected provider Metamask. Now pick the network that's in Metamask, which is our testnet.

You can see here it says 500, although it's Ether, it's actually T-Fill, but that's because we've had that extra 250 delivered by the faucet. So we've jumped up to 500. Woohoo, I'm rich.

And I can click deploy. It will bring up Metamask and ask me to sign the transaction. The reason we need some funds is every transaction on to a blockchain requires some funds on there to pay for that transaction. So that's what we've done. And that will then deploy that to the hyperspace testnet.

So while that's deploying, that'll take a minute or so. Are there any questions so far? I'm probably going to stop and ask if there's any questions as we go along, because there's a few points in which we kind of stop and have to wait for blockchains to do their thing.

So are there any questions so far? Everybody clear? Good. So yeah, this is, like I said, this is just minting NFT, well, creating the contract that represents an NFT. So non-fungible tokens on most of the Ethereum-based networks, EVM networks, are represented as smart contracts.

Some of the blockchains have NFTs as actual native functionality. But here they're represented as smart contracts using this standard called ERC-721, which is one standard. There's another standard called ERC-1155. And that's deploying on there.

Most of this contract is kind of boilerplate on here. Like I said, we've included the ability to be able to set when we mint the token, we can set where the URI is on there.

So that's now deployed, that little green check down the bottom there. And down the left here, I can interact with the contract. It now shows it here and shows me that a contract has been deployed at that address there.

And I can now interact with that. So now what I want to do is I want to mint a smart contract. So I need an address to send it to and I need a URI. So the address is going to be the address of my wallet. So if I click on there, I can copy that and put that there.

And the address of the URI is from back here. And it's an IPFS URI. Yay. I hit transact. And again, I need to sign the transaction.

And then we just wait again a little bit while that submits to the testnet. So the nice thing with using IPFS with things like NFTs is because IPFS, because you have the immutability of IPFS, or at least you can detect whether something has changed.

It means if you create an NFT, so an NFT is just a representation, it's just like a title deed to something. And so an NFT generally is basically just, I own this thing that is over at that URI. And so, yeah, having it being an IPFS URI means you can't just go and swap out the thing at the end of the URI.

So there have been examples where there's been NFTs and they've used HTTP URIs, and somebody has literally gone in and replaced all the images with pictures of rugs to indicate what's known as a rug pull in Web3, which is when the owner runs off with all your money and pulls the rug out from underneath you.

So, right, so that is now minting, and we will have that soon. Now, what I'm also going to mention, ah, that's done, good. So we have MetaMask, and you can see our balance has gone down slightly because we've been doing transactions, and those transactions cost a little bit of funds.

But we also have Brave Wallet, part of the Brave browser. And I've got that connected to Hyperspace SNET, and it's the same account that I've got configured in there, so you can see it's got the same balance there.

Now, what I want to do is, if I click View Account Assets, and I can click Add Asset, and I can add a custom asset here, add a custom NFT, and I need the address of the NFT, that's the contract address, and we can get that again back from Remix.

We can see here we have the contract address, so I'm going to copy that and put that in there, and select, I've got here, Filecoin FEM Hyperspace.

So similarly to within MetaMask, you can configure custom networks. One thing, unfortunately, it doesn't do is it doesn't automatically look that up, so I have to put that in. So, and then Token ID is, I believe, 0, there for that.

I click Add, and Done, and that should have added that. So if I click on the NFT tab, ta-da! We've got, so that was the image that I uploaded. So there we go.

So that is an NFT stored on Filecoin on FEM. So the actual asset there, that image, was taken last night with my FEM teammates, and we have stored that on an IPFS URI via NFT storage.

And so that, actually, the image itself, because of NFT storage, will be making its way towards Filecoin at some point as well to be stored on Filecoin for archival purposes.

So, that was the first demo. On to the second demo. Can anybody tell me the significance, what might be of this picture, what the link might be? Anybody recognize where this picture is from?

Men in Black. Who said that? Somebody said that before Dietrich. Got it before Men in Black, exactly. What's your name, sorry? Fabio. Okay.

Yes. And what was in the? A universe. So he has, the cat has the entire universe there around its neck.

So that will probably give you maybe a little tenuous link to the next demo, which is I'm going to run an entire Filecoin setup, an entire Filecoin universe, I might say, or at least part of the universe, on my laptop here.

So, one of the things with Filecoin is that it's a very big system, and it's quite hard to demo or test the functions and learn how to develop on top of Filecoin because there's a number of different parties involved.

So, with storage on Filecoin, and I'll go into the, like the storage deal flow in a bit, but in short, as a client, you say, I want to store some data, and then you have a number of storage providers, one of which will say, yes, I will take that data and I will store it.

So, it's two parties that are involved. So, it's very difficult to actually demo or develop yourself locally when you're reliant upon another party to do the other half of the conversation, right?

You try and test something and you're waiting for something on the other end to respond, and in the case of storage providers, there is a human element there as well.

So, what I've done is I've developed a series of Docker images that you can run locally on your laptop, and that will give you the ability to play both halves of that conversation.

So, in this demo, I'm going to clone the FEM local net repo, bring up a completely brand new, fresh local network.

We're going to use Hard Hat, which is one of the other tools I mentioned earlier, to deploy what's called this deal contract, which is an example smart contract that can create a storage deal to the local net.

I'm going to invoke it with some data to store and say, go get this data and store on Filecoin, our local copy of Filecoin.

There's some software called Boost that the storage providers run, and we have a local instance of Boost running, so we can now play a storage provider.

We can see that data, that request come in, and we can approve that and then publish the storage deal, and then complete the circle.

We can actually fetch that data from Filecoin with Lassie, and big thanks to the Boost guys. Thank you, who have helped out with this, because 24 hours ago, this was not working, and we got to the bottom of it to get it all polished off, so it is actually working now.

So, that's a pre-recording. Pre-recording is for WUSs. Let's get on with the live one.

So, there is a project here, Filecoin project, Filecoin FEM local net. All I need to do is clone that.

So, if I copy that address and clone that, and then go into that repository, and type docker-compose up.

And that is now starting up a complete Filecoin network locally. So, there's, I think, five parts to that. There is the Lotus node, that is the node that you interact with.

There's the Lotus miner, that is one that is producing the blocks. There is Boost, which is the software that I mentioned that the storage providers use to manage their end of the process.

And then there's two other additional parts to Boost. There's Boost BitSwap and Boost HTTP, that speak respectively HTTP and BitSwap out to the wider world.

So, this is starting up, like I said, a completely brand new network. When you run this initially from scratch, it has to download the docker image, and it has to download some initial proof data.

In total, that's about three gigabytes of data. So, be careful if you try and do that on conference Wi-Fi. But once that's down, then it's cached. So, I've got the cached version here.

But this is actually starting up a brand new chain and creating a new genesis block doing this. This takes about three minutes to complete that side there.

So, while that is doing that, and let's try and move on here. So, the deal making flow at the protocol level on Filecoin, you might not necessarily be able to read all of that.

But the point being is it is fairly convoluted, right? And it moves around through various parties for the deal to be created, some of which is off-chain, which is the stuff on the left-hand side as you're looking here.

And some of it is on-chain, which is the right-hand side. So, Filecoin is designed to allow you to do a lot of stuff completely off-chain.

The reason being is you're dealing with potentially gigabytes, terabytes, petabytes of data, and that would be too much to be dealing with actually on a blockchain itself.

So, the point with Filecoin is a lot of stuff can happen off-chain, and then the proof of that stuff happening is what is stored on the chain there for the deal flow.

So, in terms of smart contracts, there's two main ways we have at the moment for interacting with the deal process and resulting in data being stored on the network.

The first one we had is called the bounty hunter sort of flow, and that follows a fairly typical Web3 model in which the smart contract doesn't actually do anything per se, but it incentivizes humans to go and do something.

So, what happens with the bounty smart contract is I say, I would like to store some data that's at this CID.

I send that to the smart contract, and I'm willing to pay this amount of funds for that to happen.

And then any other third party can see that and go, okay, I can make that happen, fetch the data, store it on Filecoin.

And once it's stored on Filecoin, they'll get back a deal ID, and they can then present that deal ID and what's called the piece CID, which is the CID that represents that data, back to the smart contract.

The smart contract can then use the inbuilt market actor to actually look and see, has that actually been stored?

And if it has, pay out the bounty, right? So, that's basically incentivizing a human to do the difficult bit. And then we have a newer approach, which is what I'm going to demo today, which involves what's called a client deal contract.

And that is the smart contract emits an event, and that event comes out of the Lotus Node software, boost the storage provider software, observes that, sees it, and then will go and fetch the data and do that process.

That's a slightly more automated approach there. So, how are we doing here? Our network is currently setting up, and we'll be able to tell that it's up and running when boost is running.

And boost, if we've got local boost instance, local host 8080, yes, it's running.
So, there we go. This is boost running locally. Now, the first thing I'm just going to do is go down to settings and just change the price that we're demanding to zero so that we don't have to worry about funding a wallet for that transaction.

I'm just going to set both of those to zero. There we go. Right. We have zero deals on the network. So, next stage, we need to deploy this deal smart contract to our local network here.

And let's see what I've got here. Okay. So, which one is it here?

One of these terminals I've got set up.
So, there is a starter kit, the FEVM hard hat starter kit. Again, it's in the Filecoin project, and that gives you a number of example contracts that you can use to get started with a full network.

So, I'm going to deploy that. So, type yarn hard hat deploy.

And this is the terminal that I don't have my private key set in. Maybe this one is the one that I do.
Let's try it in here.

No. Okay. I need my secret key. I can export that. Again, this is a test network. Do not do this on live networks.

I can export my private key.

Take that and...

Right.
Okay. Wow. That's quite a failure there. Ah, I know why it is, because I've just created a brand new network and I don't have any funds in that wallet.

So, because that wallet is not the wallet that I had on the hyperspace network, this is a wallet on this local network that's completely fresh, I need some funds in that wallet.

So, what I actually need to do is I need to get the wallet address. So, I need to get that from here. Now, it says 499 because I'm still looking at hyperspace. If I switch that now to Filecoin local net, that says zero.

That's why that failed, because we didn't have any funds to actually deploy that contract. So, I'm going to copy that address from there, and I need to tell the Lotus instance that I want to...

That I need some funds, so I need to transfer some funds to it. So, I need to go into where we had our network, and I can call docker compose exec lotus evm stat.

And I need to convert that Ethereum address to an F4 address, and Lotus has a command called lotus evm stat, and that should run that, and then we get back a Filecoin address that starts T4, because it's a test network, be it F4 if it's a production network.

And now I need to send some funds to that. So, lotus send 100.
There we go. So, that is now sending some funds to that account.
So, we'll just wait for that to happen. So, this local network that I've set up has a 15 second block time.

The main production network mainnet has a 30 second block time, so it runs about twice as fast as the production network.

The actual original devnet that developers use had a three second time, but that seems to be a little bit too fast for laptops to keep up with, so I extended that to 15 seconds.

So, it's still a bit faster than working with the production network. So, when you submit a transaction, it gets into one block, then you have another block of effectively confirmation, and then you get the result back in the third block.

So, 15 second block times, about 45 seconds. So, well, there we go. 100 T-FIL. So, we have some funds now. So, that means, let's try again with our deployment.

Opening, opening, that sounds like opening the champagne.
It's deploying. Yes. Good. Right. So, that is deploying that smart contract to the local instance of Filecoin that is running in the production network.

A ball around a cat's neck.

it to go and store some data on the network somewhere. So we need some data to store.

Now on this network we're running two kilobyte sector sizes, which means we can only store up to two kilobytes. There is a config entry you can change and get it up to eight megabytes. You don't want to go any bigger than that because again running locally on laptops it takes a long time to seal each sector on there. So on the production network it's 32 gigabytes and the
sector takes about four hours to seal. Obviously that's no good for demos so we're going to do it here locally. So I'm going to call this hello ipfs.txt.

Hello ipfs. So what do you say your name was at the back that guessed the cat? Is it Marco? Fabio.

Fabio is cool. There we go. Right just to show that there's nothing up my sleeves here. You know

this is a live demo here. So hello ipfs.txt. Fabio is cool. So we need to put that somewhere

for the contract to, sorry for boost to go and fetch that. Now there is a nice little site

data.lighthouse.storage and this just is a convenience site for onboarding data into Filecoin.

You can actually do this manually yourself locally but you just need to put the data somewhere that
boost can get a hold of it. Somewhere initially so whether that's http or ipfs it doesn't particularly
matter but just somewhere it could get to it. So I'm going to upload a new file and I'm going to
choose here hello ipfs. That's uploaded and so we can see here hello ipfs and this gives us a number

of pieces of information that we need to pass to our smart contract. So we can plug those bits and pieces into the invocation we're going to do on our smart contract. So I've got that actually
written out here. So the thing that we need we need the PCID which I think is that there yeah PCID.

We need the payload CID. Let me just make sure I've got the right yeah hello ipfs. The payload

payload CID which goes in here. Piece size is 256 car size is 220. So piece size 256

car size 220. I need the URL to fetch it from which is down here so I can copy that

and change that. So I think that's everything there car size location. We set the price we want
to pay zero. If you remember at the start I set boost to say it demands zero so we don't have to actually pay for this storage. We set the start and the end epoch so that's when we want our
storage deal to start when we want it to end. We set the piece size the CID up the contract. We
need the contract address which we have here. So place that there. So right let's now execute that

and copy that and I'll run that here. I'm in the wrong directory. I'm going to run that here.

Right so that is now submitting a transaction to that smart contract on our local network saying

hey here's some data I want you to store and go and fetch it. And we can actually see this is the

the the log output of everything that's going on on the on the network that we set up and at some

point we'll see that come past. We'll see boost hopefully spot that that contract has omitted an

event and yet boost storage deal boost has seen that. So that should mean now if I go back to boost

deal one here we go and it says transfer queued. So it has queued that transfer for retrieval and

it's now going to try and retrieve it from that id. It's actually started it's very quick because it's only 256 bytes. It has fetched that so the transfer has finished successfully. Now the boost

network is going to go on and do some other bits. It waits for a little bit and then it will start

sealing the sector. So what happens with Filecoin is when data is taken on by a storage client
it then you then see what's called seal the data which converts it to a format you can then run

these regular proofs on. So a storage provider has to do a proof of that data every 24 hours.
So it has to prove to the network yes I still have your data and I haven't lost it. So there we go we're slightly running out of time but hopefully this will finish very shortly and we

can then kind of go on to the next step. So that is now grabbing that and doing the sealing.

Boost people do you know will do you know if this if Lassie can fetch at this point or not?

Right that's the step that I've missed we're ready to publish. So after publishing you can fetch it. So I hit publish, publish now and it's going to go ahead

and publish that deal and we can actually see here awaiting publish confirmation. So that is now publishing that storage deal. So that is the miner telling the network yes I am

dealing with this piece of data. I fetched it and I'm now doing the sealing process. So that's going

to do that. So while that's going on what I'm going to try and do is I'm going to try and fetch the data since we're running out of time. I'm going to try and fetch the data now and see if we can get the data back from this from Filecoin as well. So we've gone full circle round. So what

I need is the root CID I believe this one. Yep good thank you and I'm going to run Lassie.

So Lassie is a retrieval client. Actually I'm just going to go there. So I can say Lassie fetch now.

If I give it that CID and run that Lassie is going to try and fetch it. Now Lassie is very clever and it's going to contact thing called the IP and IV interplanetary network indexer and try and find
it. Now it can't find it because we're not in the real world we're just running this on my laptop remember. So it can't find it. So what we need to do is we actually need to tell it where to find it. We need to give Lassie a little bit of a hint. So fetch dash dash providers and we need to give it

tell it where we can actually fetch our data from and we can get that from boost. I can go to settings and down here we see

the addresses. Now I think I need that bit and then it is port 8888 I believe

and we need the peer ID which I think is that one slash peer to peer.

I think if I get it wrong it'll tell me yes the peer ID didn't match that's because I've got I've actually got the wrong that's not the right peer ID there you need the actual one from
lib p2p which shows up in the log somewhere but it actually helpfully just tells me here that it didn't find the right one. So peer ID so that's one that's ended 17 still I need this one

there we go so let's run that so that is Lassie fetching that hopefully

it's taking its time is bitswap 8888 or is it 777 I have got it right have I okay
ah okay what have I got wrong there anybody see anything boost people see anything obviously

wrong there let's just try it again oh there we go well it worked a second time um so we've got that so we've
we've we've now got a car file so I can say ibfs car dash dash unpack pass that I get that I now

have a directory I go into there and in there is a file ipfs fabio is cool

so there we go so that is a full demo there of spinning up a complete file coin instance and
going through like the full workflow there so there you go you can try it out yourself there's some resources if you scan that qr code you'll get to a link tree you'll get to that url
down there that gives you a whole bunch of links jump off points to a whole bunch of other stuff find me on twitter hamato we've also got the fm dev twitter account for the fm developers and

you'll find us on twitch generally on a thursday doing a live stream I think it's 1600 hours utc

on twitch twitch.tv slash fill builders so there's normally myself and my colleague sarah doing live

stuff like this and building things so thank you very much I ran over slightly so I'm not sure if we got any time for questions but if anybody wants any then wants to know anything then come find me yeah
what advice would you give to organizations who are looking to integrate fbm into their
stack what what advice would I give to organizations want to integrate fbm into their stack um
what kind of benefits they can see well benefits I mean like I said the the main thing that ip uh the fbm allows is you to interact with the storage markets right so if you are trying to do something that involves
you know instructing the filecoin network to do some data storage and that's really kind of where they need to be for it to kind of make sense for them to do it and then advice on how to do it
you know come find us on on slack on fill builders we're normally in there or like I said you can

find us on twitter or twitch thank you

