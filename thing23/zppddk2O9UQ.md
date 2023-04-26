
# Effectiveness of Bitswap Discovery Process - Gui

<https://youtube.com/watch?v=zppddk2O9UQ>

![image for Effectiveness of Bitswap Discovery Process - Gui](/thing23/zppddk2O9UQ.jpg)

## Content

Alright, so I'm going to talk about a measurement that we run at ProbeLabs.

So I'm Guy, I'm working with Protocol Labs on the ProbeLabs team with Yanis, among others,

and we run mostly measurement and protocol optimization all around libp2p, ipfs, and

also filecoin. And that's a measurement that we've run this year in December, and we wanted to get some

more data around bitswap. So for those who aren't familiar with bitswap, bitswap is mostly a data transfer protocol

that is used by ipfs to just transfer data from one computer to another, but it's also

doing some content routing. Not sure if it was meant to be from the start, but that's what we're going to evaluate today,

not the data transfer, but rather the content discovery, content routing.

So the motivation that led us to do this is the way bitswap works is there is a static

provider search delay, which is a magic parameter that has been set to one second, which means
that bitswap will wait for one second before making a DHT query in order to find the content,

and we wanted to know whether this value of one second was right, or if we should maybe increase it or decrease it or get rid of it at all, in order to optimize content routing

efficiency. So just a few words about how content discovery works in bitswap.

So first, let's take the Kubo example.
When you want to look up for a CID in Kubo, Kubo is going to call go bitswap and it's

going to say, hey, I want to have this CID. And what go bitswap is going to do is for one second, it's going to broadcast, hey,

I want this CID to all of its directly connected peers. And if within one second, none of them say, hey, I got the file, here you go, it will

say, hey, DHT, I need some help. Can you look this CID up for me and tell me who is storing it?
But the DHT walk doesn't happen if the bitswap broadcast is successful in the first place.

So what we did in order to measure it and to get data around this is we got a huge list

of CIDs that we got by just sniffing the bitswap traffic. So just listening to all of the requests saying, hey, I want this file, I want this file, I want this file. And we gave bitswap not just one second, but 15 seconds to find and retrieve a block.

And we made sure that bitswap didn't call the DHT, which means that Kubo is going to tell bitswap, hey, can you find this CID for me? And now bitswap is on its own, has 15 seconds, and it's either you get it or you don't get
it. And in the cases where bitswap wasn't able to find the content, we still wanted to make sure that the content exists and is reachable somewhere.
So we just make a DHT query. And if the DHT query succeeds, it means that bitswap failed to retrieve the content.
And if the DHT query doesn't find anything, it means that the content isn't unreachable.
And so it's not a bitswap problem. I will also prevent recursive content resolution, which means that for CIDs, if you request

a root CID in Kubo, it's going to retrieve all of the content that is included inside

this block. So we didn't want that. We wanted to retrieve a single block only. And this experiment has been run in December last year from Google Cloud VM in Central

Europe, which is relevant for the latencies. I mean, it's relevant to know this information when we get to latencies.
Now some numbers, some statistics. So we discovered that more than 98% of discoveries were successful, which means that bitswap

is great at discovering content. However, for each request, bitswap has to solicit more than 800 peers.

That's really a lot. So it means that when you're looking for content, you literally just broadcast, hey, I want this. I'm not sure it will work, but at what price? And so, yeah, that's a lot of messages to be sent. And also we got some statistics about the content providers.
And what we've seen is, for instance, the top 20 providers are serving 75% of the CIDs

that are requested a lot. So it's not you list all of the CIDs that exist in the IPFS network and just see who's
providing them. Now it's the CIDs that are actually accessed. So during the measurement period. So it means that only a few providers are providing a lot of content.
So on the huge list of CID we got, we only had a bit more than 700 content providers,

which is not a lot, given that, for instance, the public DHT has around 20,000 participants.

And what we also observed is that NFT.storage, so that's on the right hand side, the top

10 peer IDs that were serving blocks. And most of the requests were towards probably NFTs provided by NFT.storage.

But we couldn't find all of the operators of the top 10 peer IDs.

So now to latencies. So that's only for successful bit swap discoveries. And we can see that the majority of the requests will succeed within one second, if it is successful
at all. However, we can see that some of the requests will succeed up to 15 seconds, because there

was the given time. So it means that sometimes you're requesting for some content and bit swap will eventually
get it after some seconds. But in most of the case, you get it under a second. And now if we zoom it a little, we can see that even most of the content, like almost

80%, is obtained within 200 milliseconds, which is very fast.

So to get the content within 200 milliseconds, it's two RTTs, which means that a node that

is going to be located close to you is going to be directly connected to you first and
provide you this file. So we ran the measurement from Europe. It means that content was available somewhere in Europe probably, so that we could get it
this fast. Now since the measurement, there have been some recent developments around this bit swap

in general. And there's been an upgrade on the connection manager, limiting the inbound connection to

a bit less than 100, which means that now bit swap isn't connected anymore to like 800,

900 peers. And so the broadcast is less efficient. So it's good because it's less spammy. You're going to spam less peers in order to find your content. But it's also going to be less successful probably.
So we don't have numbers since this happened. We've also tried to just get rid of the provider search delay.

So set it to zero second, which means that at the moment, Kubo wants to look up for a
TAD. It's going to in parallel query the THC and do a bit swap broadcast.

But unfortunately, probably due to some side effects or bugs in the way session are handled

in bit swap, this didn't turn out to give a better TFB.

So the performance to get the time to first byte was actually worse when we reduced the

delay, which from a protocol perspective doesn't make sense. But yeah, it happens. And yeah. So now the takeaway that we have is that bit swap is a fast and accurate way to discover

content. It's true. But it's just very inefficient. You're going to need to pay a lot and generate a lot of traffic in the network in order to

find the content, which is probably not what we want.
And also bit swap as a content discovery mechanism doesn't scale.
Because in order to keep the same, let's say, success rate when you discover content, you

need to be connected to the same ratio of the peers. Which means that if the network scales, but we got 10x peers in the networks, which is

like 10x providers as well, then you will need to be connected to 10x more peers in
order to get the same success rate. And it's linear. It just doesn't scale. And also what we observed is that, which was quite surprising for us, is that the top 20

peers, the top 20 providers were actually serving like 75% of the requests on bit swap,

which is surprising. But it also means that if you are to broadcast requests, you could just broadcast your request

to these 20 top peers and you will have a 75% success rate, which isn't that bad and

it's not so noisy. And so then it brings us to the question, do we really want to bundle data transfer

and content routing together in the same, let's say, component bit swap?
Or should we have like a distinct data transfer so bit swap could still do bit swap stuff,

but just not broadcast and we could use more efficient content router such as the DHT or

IPNI or any other content routers that are a bit smart and don't just broadcast, hey,

can you give me this to all of the peers? So yeah, that was mostly it. So there's a full report on the GitHub repository network measurement that you can scan the

QR code to get to. And it contains the additional data, more plots, and also some improvement recommendation.

And yep, that was it for me. I'm happy to take any questions you may have.

You might have answered this before and I might have missed it. Where were those metrics of availability of CIDs?

Where did those come from? How did you collect them? The availability of CID? Yeah. Like that 75% number that you're using? Were you making those requests yourself or are these based on gateway traffic? No, it's traffic that we sniff from bit swap. So it's... Oh, I see. So you just have a passive node and it's just getting... Okay. And we made sure that each CID... If we were requested twice the same CID, we didn't replicate twice the traffic.

So we made each CID unique. Yeah. Okay. Thanks. Thanks, Lachie. I'm just curious what... It's probably in that link at the end, but what are your suggested next steps?

I'd say if we can get a bit swap, like fix bit swap to make sure that we can have provider

search delay at zero so that we can start the DHT walk at the same time, then we would

see the performance of the DHT compared with bit swap. And then gradually we can walk away from this general broadcast and make a bit swap and

Kubo much less spammy. So I think the DHT is a much more... or IP and I are much more efficient content routing mechanism and we should go towards this and gradually turn off the bit swap broadcast.

Thank you.
I have a question while we wait for others, perhaps. So it's interesting that 200 milliseconds graph that we've seen in the past, it just

occurred to me that maybe what we should have done is run some of these experiments from a remote location to see if that would go to 400 milliseconds or something.

Because yeah, as you said, we were running this experiment from within the EU and then

if content is also within the EU, then we have that 200 there, but maybe it's not the case for all of the world. So yeah, it would have been interesting maybe something to think about.

Yeah. But as a lot of content is NFTs coming from NFT storage, I guess NFT storage has multiple

replicas of all the data all around the world. So I expect maybe the performance in North America to be similar. But maybe accessing from a remote place would just shift the graph to the right, I think.

Yeah. Yeah. Okay. Is NFT.storage that 75% of the nodes that are quick to respond?

So we didn't look into all details. And so maybe, so that's, so the NFT storage node, we're sure that it's NFT storage node

and the ones that don't have a label could be NFT storage or other providers.
So that's a lower bound of the NFT storage traffic. And yeah, so for instance, for the 75% is the top 20 providers.
Here you can see the top 10 and we have six, at least six NFT storage. So that's an estimate. Okay, then let's thank Guillaume again.

