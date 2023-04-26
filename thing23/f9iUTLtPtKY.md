
# Web3 CDN Saturn accelerates IPFS & Filecoin retrievals - Alex Kinstler

<https://youtube.com/watch?v=f9iUTLtPtKY>

![image for Web3 CDN Saturn accelerates IPFS & Filecoin retrievals - Alex Kinstler](/thing23/f9iUTLtPtKY.jpg)

## Content

Hey everyone, my name is Alex, I'm product manager on Saturn and today I want to talk

to you about how Saturn accelerates retrievals from IPFS and Filecoin. Before I get started,

there's one thing to celebrate today, I was actually waiting for the countdown, we reached over 2000 nodes on Saturn today. So it's a big achievement. A bit of a caveat here, this

number might go down because there's still a little bit of multi-noding going on and we'll be a little bit stricter on our requirements for performance and also availability to actually
improve the resilience of the network. Today I'm going to talk a little bit about the background

on Saturn, why we build it, which problems we solve. Then introduce Saturn itself and

talk a little bit how it fits into the context of the project Rhea, connecting basically
dots where we left off before. And lastly I would like to talk about a service worker

example how to actually get to work and directly retrieve files from Saturn. And an outlook

in the end. Okay, let's start with the background context.

So what's the core problem that Saturn is trying to solve? So number one, most of the traffic and retrievals from IPFS today is going through public centralised gateways.

So these don't really match the speeds that modern DApps and large-scale applications
in Web3 require today. And secondly, there is currently no cache and acceleration layer

for Filecoin, so Filecoin retrievals are not ideal. So there's lots of work being done

there. Saturn should help actually accelerate retrievals from Filecoin, apart from the incentivisation
layer of course. So why are we building Saturn? So from a commercial perspective, if we look

at the market at large, the global CDN market is expected to be twice as large as the cloud
object storage market. Of course only trust the statistics that you falsified, so take
this with a pinch of salt. But yeah, so there is definitely a commercial reason to build
a CDN and that also goes for Web3. So let's move on to Saturn as a decentralised

CDN. So for anyone who has not heard yet, but I assume everyone has, Saturn is a decentralised
CDN that supports retrievals from Filecoin and IPFS. A few stats apart from the 2000

nodes that we have achieved since November in 2022, we have now a capacity of around
11 terabits per second, which is about 5% of the network capacity of Cloudflare, just
to give you a comparison. So it's a huge growth that we had in the last 5-6 months and we're
currently serving about 158 million requests daily.

And just before that presentation here, we had a chat with Diego and we pulled up the stats that were presented in November when the network was launched. And so looking at actually some of the performance numbers for the time at the first byte. Back then we had

about 135 milliseconds on median response time, which was like 2.9 times the speed of

the IPFS gateway. And today we measured a median of 10x faster than the IPFS gateway,

averaging at around 64 milliseconds, which is a really great achievement. The same goes actually across the board for the average for P95 and also for the P5, where
Saturn is already beating the IPFS gateway at large. We don't yet have the traffic fully

running through Saturn, so these numbers will definitely change. But I think that's a really good indication that shows that the network is becoming more performant the more nodes are actually added to the network. So talking about commercial viability of Saturn. So who is actually Saturn for and what can it do for you? So first of all, we're targeting a couple of verticals. Not all of them are
our first priority right now. But just to give you an indication, Saturn could be used,
for example, by storage providers who want to accelerate and have instant retrievability of a file that has been uploaded and improve the user experience for the end users. It

could be used for NFT projects, like marketplaces, etc. to retrieve NFT assets really quickly

and also improve basically the overall performance. It could be used by blockchains to, for example,

sync blockchain snapshots for node providers to actually get up and running faster, for

example. It could be used by video projects to do browser-side HLS streaming using ServiceWorker,

an example that I'm going to show a little bit later today. It could be used by game studios, for example, to load any textures, skins, like this large amount of data, like

gigs of data, for example, when you play one of those modern games, which you typically
load either up front when you actually start the game or during the gameplay. So that's what Saturn could be used for, especially in a client-side implementation. It will enable you to use the full power of the network and also in general for the app developers.

So talking about Saturn, the first client of Saturn is IPFS.io, as we have seen before.

So just to put it in perspective, as a summary to Will's presentation before on a high level,

typically a customer would request a SID from IPFS.io. IPFS.io will proxy that request to

Saturn. Saturn, using LASI, will either serve it from the Nginx cache or it will cache miss

if it doesn't have actually a file to IPFS or Filecoin using bits of Graph Sync. You

have the Saturn orchestrator here on the bottom. There has been previously a presentation on how the orchestrator works. So the orchestrator is currently still a centralized service that manages the network, allows who can join the network, who leads the network, is responsible

for logging and also for recalculating the weights of each node on the network. And so
Saturn can currently serve car files or blocks back and the IPFS.io gateway will do the verification

for you. So moving over to the use case example for

today, so there are currently different integration types how you could use the Saturn network.
So one is the browser retrievals via the service worker where you actually use the full power

of the network because you actually interact with the untrusted nodes and the verification is happening on the client side incrementally. Secondly, it could be server retrievals where

you could potentially use something like Caboose as server client libraries for use cases like

blockchain nodes who sync state for example and have to run it directly on the node. And then lastly gateway retrievals using IPFS.io or the service worker implementation that Adeen just presented. So let me dive into the browser retrievals
example and let's hope that the service worker works. All right. So it's pretty simple. So

we have to add actually just a script tag to our file and then also download the service

worker itself, add it to the root of our directory and hope that we can actually play that. So

let's give it a go. So I'll make this smaller. So I have prepared an empty file here that

just embeds video.js so that you can do HLS streaming. So there's nothing else configured

here. There's no service worker, nothing. So let's give it a go.
So first we have to add the tag. Let's do that here. Okay. So it's there. All right.

And then let's try to fetch this. So I've prepared this up front. So let's have a look

at the IPFS desktop. So I have a bunny video here. So all the different

like HLS chunks are uploaded. We have the manifest files here in the bottom. It's the HD version of that. So let me add the source here. Okay. Cool. Okay. So what else is missing?

So now we need to actually add the service worker to the root directory of that file.

Let's fetch that one. Nice. Okay. That's fun. Space between? Oh, yeah. Great. Perfect. Harder

to see on the stage. Thank you so much. So, yeah, we see we've got the service worker
here responding to the correct domain. Okay. Let's save this. Let's start this. Okay. Wish

me luck. Okay. So let me make this a little bit smaller.

And it's loading directly. So we see there's a cache hit. It's playing directly. Let's

jump somewhere else. It's working pretty well. Okay. So you see here's the remote address

of the node that we are connected to. So I also want to show you one more product that
we're about to release. So that's the Saturn Explorer. It has been built by an external
company for us. So let's search for the nodes that are available in Brussels. And there

you go. So we have two nodes. One of these nodes is the one that we're connected to.

Perfect. Cool. And so as an outlook for what's coming next,

it's a lot of moving pieces at the moment. So first of all, we actually have well, we

have achieved the sub second time to first byte for P95 already last month and currently

actually working on stabilizing that, especially when real traffic will come up to keep that performance. So end of this month, we want to launch a
better test program for Saturn customers, because we're really commercially driven to
also start onboarding like the first customers. Next month, we will release the decentralized

payouts with the smart contracts on FVM, where currently there's a beta running with a Cassini test group. And by hopefully end of this quarter, we've got 100% of all the traffic flowing

from RIA through Saturn, and we achieve a way better performance correctness and latency for that. That's it. Thank you very much.

