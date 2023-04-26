
# Opening the DHT to large content providers - Gui

<https://youtube.com/watch?v=bXaL64fp55c>

![image for Opening the DHT to large content providers - Gui](/thing23/bXaL64fp55c.jpg)

## Content

Hi everyone, so as Massie said I'm going to talk about mostly the DHT and how we can open

it to larger content providers. Maybe you know it's been a pain for large content providers to provide a lot of content

to the DHT because the implementation hasn't been that efficient and one motivation on

why we want large content providers to advertise the data on the DHT is to get them away from

BitSwap because some of the large content providers are only providing on BitSwap and
BitSwap is very chatty, it's a lot of spam across the network, the only way to get some
content is to go through BitSwap and so that's to get them to the DHT so we can get all IPFS

less chatty and more resource efficient. So how does the DHT provide process work? Because that's what we want to improve. So when nodes want to advertise that it has some content to the DHT, what we do is we

need to find the 20 closest peers to this CID and then ask them to store a provider

record which is basically a pointer mapping the CID to the peer ID of the provider such

that anyone that is looking for this piece of content, this CID, can look up the 20 closest

peers and at least one of them will be there online and will say okay this peer ID is providing

this content. So now in terms of performance, in order to find the 20 closest peers, usually in the

IPFS network we need around like 3 or 4 hops, the way it works is we have concurrency parameters

so we can have at least, so we can have at most 10 in-flight messages at once which is

I won't just ask, so when I'm looking in my routing table for the closest peer to a CID,

I will not send just one message, I will send a message to 10 of the closest peers I know
and get back an answer and iteratively get closer and closer.
So it means that the overall number of messages that I send to discover the 20 closest peers

will be around 30 at the moment. Note that when I say 20 it's the replication factor which sometimes is referred to as K

but K has multiple meanings in academia so I'm going to refer to this parameter as rep

for the rest of the presentation. And then once we've found these 20 closest peers we want to allocate the provider record
to these peers so that's additional messages. So now looking at it in terms of number of connections to open and number of messages

to send, we get that we need to open approximately 35 connections, so new connections when we

allocate one provider record, I mean when we advertise one CID which means allocating

20 provider records. So most likely for the first hop of the DHT resolution we will already be connected to
the peers so that's good, we don't need to open new connections. And then when iteratively getting closer we need to open some new connections, so approximately
15, so that's just back of the envelope calculation. And then to find the 20 closest peers we need to send 20 messages, yeah we need to open

the connection to these 20 peers. And the number of messages, so it's approximately 70, so 50 for the lookup and we need to send

the provider records, so 20 times which is 20 messages.
Now how does it look for large content providers? So let's say a large content provider wants to provide 1 billion CID to the DHT.

Now every single CID must be reprovided periodically, so after 48 hours all the provider records

will be garbage collected, so we get rid of them and just providers need to republish.

And the reprovide interval has been set to 22 hours based on measurements because what

we want, the first priority is that the content is still available. So even though the provider record will be garbage collected after 48 hours, if all of
the peers that are storing my provider record go away because of the churn, because you open your laptop, you get some content allocated, then you close the laptop, we need to make
sure that the content is reprovided every 22 hours. And so it means that in terms of number of open connection, so now if we want to provide

1 billion CID, if it's 35 connections we need to open for each CID, that's 35 billion connections

we need to open in 22 hours, so that's like 450 connections per second, so that's a lot.

And number of messages sent, it's approximately double this number.
So that's why the DHT is currently very friendly for large content providers and this is a

major problem. So there are ways around this, such as using the accelerated DHT client, and I'll come

at this solution at the end of the presentation. So what can we do to optimize now this reprovide operation because we need to reprovide every

22 hours. So now you're going to tell me, okay, we need to reprovide like 1 billion CID, but we only

have 20,000 peers in the DHT, which means that I could just open a connection to these

20,000 peers and send them all of the CIDs instead of looking for the same peers over

and over again, closing the connection, opening again. And so that's, so we can just use the pigeonhole principle because we have much more CIDs than
DHT servers. And it means that we're going to allocate multiple CIDs on the very same DHT servers.

So what we can do is just group the CIDs we want to provide by XOR distance, which means

that yeah, all of the CIDs that are close together will be allocated on the very same

DHT server and just sequentially. So we open the connection to this DHT server and send it all of the provider records that

we allocated them at once. And so in order to optimize this, we can just sweep the key space. So from the key 0, 0, 0, 0 to the key 1, 1, 1, 1, 1. And when we get to a peer, we just say, okay, here are all the CIDs I want you to advertise

for me, and then go to the next peer and continue to just sweep this way.

And this way we can minimize the number of connections to open and of messages to send.

And so we should get approximately, the number of connection to open should be approximately

the number of DHT server peers.
So now let's get to the details of how it works. So I have on the right hand side, a binary trie, which is a prefix tree, which is commonly
used to represent distance in the DHT. So in the rightmost column, we have the peer IDs. So those are the peers, the nodes, and all of the other, let's say, cells in this graph

are just, let's say, intermediary cells to represent distance.

So just to draw the binary trie. And so here in this example, we have keys or peer IDs that are represented by a bit

string of length eight, and in Kademia we have a bit string of length 256, just because

it wouldn't be readable to have such a deep tree.
So yeah, so this representation is optimized when we are looking for a key.

So in a database, when we are doing XOR distance arithmetic, this binary trie is the optimized

data structure to look up and add and so on. And that's the visual representation that will help us understand what's happening.
And so that's also what we're using to group the CIDs that are close together.

So now we define, so in this binary trie, a key space region. So we define a key space region as a region where all, so let's say the intermediary nodes

are prefixes of the keys. And if a prefix has at least, so the replication number children, which we'll go through an

example then, it is considered as a region.

And we are interested in finding the smallest number of possible regions that are fully covering the key space. So now if we go through an example, we get that the replication factor will be three

for this example. And so if we take the first, so the topmost yellow rectangle, we have that in the rightmost

column, we have four peers. And so they all start with the prefix 00. And so we can see that the prefix 00 has four children or four nodes within it.

And so it's defined as a proper region. Now going to the second region, we can see that the prefix 011 has exactly three nodes

within it. So it could be a region. However, the prefix 010, which is just above, has only two nodes in it, which is not enough

to make a region. And so we need to combine them together to make sure that, so here the prefix 01 has

more than three nodes within it. And so you can apply the same logic. So going below, so it means that the region will always approximately have the same size,

which is going to be slightly more than the replication factor, but it's not necessarily prefixes with the same length. So we can see then there's the prefix 1000. So it's a prefix of length four, whereas before we had prefixes of length two.

And so this region will be useful in order to understand where we need to store our CIDs.

So a CID will only be stored. So all of the replica of the CIDs will always be stored in the very same region.

Now how do we explore a region and make sure that it is a region?

So what we need to do is to look up a random key within the target region.
If some of the return peers do not match the region's prefix, it means that the region

has been fully explored. And otherwise, if all of the peers are within the region, we must explore the neighboring

region, which means that we take the prefix, flip the last bit and generate, so a new key
there and explore the new sub-region until the region is fully explored.
We have enough peers in this sub-region. And now once we have this region, what we want to do is sweep all of the region from

the key space, let's say from left to right or top to bottom, doesn't really matter, but from the 000 to the 111.
And once we are in a region, we need to re-provide all of the CIDs that are matching this region.

And so it means that once the sweep is over, I have re-provided all of my CIDs.

So it means that the sweep needs to take approximately re-provide interval, so 24 hours to complete

or can take up to 24 hours to complete.
In order to do this, we need to store the peers in a binary try so that it's easier
to keep track of the regions and the seeds will be stored in the binary try as well in

order to understand in which region they belong. And so it's also optimized for faster insert or delete of the CIDs.

Now how does re-providing work within a region?
So within a region, we need to define a temporary key value store that will map peer IDs to

keys. So we basically want to list all of the keys that must be allocated for every peer ID in

this region. So we just iterate on the seeds that belong to this region and map them to, so here, as

we want to replicate them 20 times, we map each key to 20 peer IDs.
And then once we have this map, we just iterate over the peer IDs of the region. We open a new connection and then we allocate whatever number of CIDs we have to allocate

them. So it means that we open the connection once and then send multiple messages, one for each
CID we want to re-provide. And so the number of workers can easily be limited in the implementation.

Now how do we schedule this? So ideally we do not really want to re-provide everything at once, which is what the accelerated

DHT client is doing, just because it's causing a rush hour and you do everything at once,

then you can shut up your node. I mean, in some application this would be needed, but usually it's more resource efficient to spread the use of resource over time.
So each region, so if you take the very same region with the very same prefix, should be
republished within the re-provide interval. Otherwise maybe the data will provide the record, wouldn't be available anymore.

And so as the regions may grow or shrink, the delays within the region may need to be

adjusted. And we need to have a scheduler to keep track on when to re-provide each region.

And also it's important, like one important role of the scheduler is what if, so I was

providing in a sweep and over and over again, and then my nodes goes offline.
And then when it goes online again, I need to catch up for the things that I didn't re-provide.

And so I will try to re-provide as fast as possible until I get back on track.

So now concretely, how does it work with, so yeah, let's take how it works from the

start. So now I have no CIDs that are tracked and the IPFS implementation is asking me, so DHT

implementation to provide a first CID.

So the first provide of the CID isn't timely. No, no, sorry. It is timely because when I want to publish some file, so I advertise it on the DHT and

then maybe I immediately want to send it to a friend and I want this friend to be able to get it from the DHT. So I need to provide the CID immediately. But then the re-provide isn't timely because I have 22 hours to re-provide. So it doesn't matter if I re-provide it after 15 hours or 20 hours or 22 hours.
So that doesn't matter if it takes more, even if it takes seconds or minutes to re-provide.

And so it means that what I'm going to do is when I receive a provide, I will just do
the lookup and allocate to the 20 closest peers. But then I will not wait necessarily 22 hours to re-provide because the CID will belong

to a region that will be scheduled to be re-provided at a given time. And so I will just put this new CID with the region and republish it at the time that was
planned for this region. So now what happens when a region is shrinking, which means that if a region has, so in our

example, we got like a replication factor of three. So we needed each region to have at least three peers. Now if peers go offline and a region only has two peers left, what happens?
So in this case, if we have a region with less than the replication factor of peers,

it must be merged with its neighboring region. So the region, so if you take the prefix identifying the region, you flip the last bit, you get
the neighbor, and you just migrate the regions together to a larger region.

So it means that the time the content will be re-provided will be slightly changed because

both of the neighboring region each had their time at which the content would have been

re-provided. So what you can do is just take the soonest time to re-provide all of the content.

And same for region expansion. So if a region grows and at the point it can be split into two distinct regions, so two

regions that have at least a rep peer inside, then we can just re-provide both regions for

the first time at the time that was planned for the region to be re-provided.

And then the scheduler will make sure to just space the re-provide time so that not everything

happens at once. So now I'm just going to give an intuition of why this is correct, or the re-provide

scheme works, is when we have... the first provide... we just need to find a region where this CID belongs, we add the region to the scheduler

and it will be scheduled to be re-provided every 22 hours. So that's for the very first CID.

And then when new CIDs are added, they are added either to a new region where there was no CID,

so a new region is defined and added to the scheduler, or they are added to an existing region.
And then it's just basically induction, so if the region grows, so...

I mean, you're going to add the CIDs to the already existing region, and now the regions are going to shrink or expand

depending on the peer turn in the network, and so that's how we define that the re-provide happens.

So now, let's come to the performance evaluation and try to measure the performance.

So if, again, taking a large content provider, we're providing 1 billion CIDs every 22 hours using IPFS realistic settings,

so we measured approximately 20,000 DHT server nodes, and we have a replication factor of 20.

So it makes us... So to know the number of regions, it's approximately the number of peers over twice the replication factor,

because, yeah, on average, I mean, an upper bound of the region size is twice the replication factor.

So we got approximately 500 regions. In order to explore a region, we need approximately 55... We need to open approximately 55 connections and to send 70 messages.

So the number of connections to be opened in order to re-provide the 1 billion CID is expected to be around 28,000,

whereas it was 35 billion for the Vanilla provide operation.
So we got an improvement of the order of 1 million, which is huge in the number of connections.
And then in the number of messages to be sent, so we'll need to send approximately...

So 500 times 70, which is the number of regions, times the number of messages to explore a region,
plus, as we need to provide 1 billion CIDs and each one of them to 20 peers,

we need to send 20 million messages to provide CIDs. And so that's kind of the limit. So we can approximate the number of messages to be sent to just the number of replica provider records.

And so when we compare it with the Vanilla provide operation, we got an improvement of 3.5x,

which is also good, but not as impressive as the number of connections to be opened.

So now we can compare it with the provide many from the accelerated DHT client, or full RT, which is the same thing.

So what the accelerated DHT client does well is that it will group the CIDs by XOR distance when we're providing,

which means that, okay, it's re-providing everything at once, which is good and bad.

It's good because you can group the CIDs and minimize the number of connections to be open,
but it's bad because it will cause a rush hour, so you just re-provide everything at once and generate a lot of network traffic.

Also, another problem of the accelerated DHT client is that it may contain stale entries in the routing table,

because it is made... So you do not make a lookup before allocating the CID to the 20 closest peers.

What you do is you run a crawler, I think, every hour. And so the crawler is going to give you an image of the network, but maybe you're going to re-provide one, like, 15 minutes after,

and the network has totally changed, and you don't have a clue about this, so you're going to allocate to the wrong peers,

or peers that aren't online anymore. And running this crawler in order to refresh the routing table is also expensive, because you need to basically connect to every single peers every refresh interval, which I think is one hour.

And also, it's not very optimized, because the CIDs are grouped...

The CIDs group have a constant size, which means that it isn't optimized because of the try, because...

Yeah, if you order them in a linear way and group them with a constant size,

you will not respect the try and the prefixes of them, and so you will end up allocating them on the wrong peers,

and opening more connections.
So in order to get there and be able to implement this, we need to tackle a few other problems first, as always.

So now, the way the DHT works is when you want to provide something, there is just a provide interface to the DHT,

or to content routers in general, which I think isn't good,
because it means that in the current situation, the IPFS implementation, so Kubo or any other implementation,

need to handle the reprovide themselves. Which means that if Kubo wants to track some content, it must have the timer and have knowledge about the DHT

or any other content routing system, such as IPNI, know which one it is using,
and know how to reprovide using this content routing system. And what we should do instead is having each content routing system have its own reprovide mechanism,

because they all work differently, IPNI is totally different than the DHT for reproviding.

But we should just have a simple interface, such as, okay, start providing content, stop providing content,

such that the IPFS implementation doesn't need to have any knowledge about how reproviding works

in a specific content routing system, and can just say, okay, please start providing for me,
and I don't need it anymore, you can stop providing it. And so it means that we need to change interfaces in how it works, but yeah, I think this will be done in the coming month.

So now, so yeah, that was the explanation of how we can minimize the reprovide cost for anyone.

I mean, it works especially good for the large content providers, but if you are advertising some content on your local Kugoo node, it's also an optimization for you.

And it enables large content providers to use the DHT and slowly get away from the BitSwap broadcast, which is good.

And we want to ship this along with the DoubleHash DHT later this year,

so that, yeah, if you switch to the DoubleHash DHT, you have some extra benefits that you can reprovide in a much more efficient way.

And if you're curious about it, there's the Notion page that you can check out, in which the updates will be happening.

So yeah, that was it for me. Thank you.
Yep, Thomas? Isn't the current solution where you have to reprovide sort of like a stopgap mechanism, where you make sure that you spend some cost or you have some cost with wanting to provide content to the network?

Yeah, but this solution will not change this. You will still need to reprovide every 22 hours. It's just you reprovide in a more efficient way.
So is the main performance bottleneck we're addressing here is the connection establishment?

Yeah, I think so. I mean, I haven't tried to advertise that much content myself, but just looking at the back of the envelope calculation, opening 450,000 connections per second seems a lot to me.

Yeah, I mean, you're going to have to send the messages either way at that rate, right? Yeah, I mean, yeah, you need to send a lot of messages anyway, but if you minimize the number of lookups you do, you reduce the number of messages you send by 3.5x already.

Okay, so it's not necessarily the number of... It's not just the connection establishment, then, it's the numbers, the lookups that are expensive, right?
Yeah, because... I'm just curious, because I know that, for example, we've been talking a lot about QUIC, and that actually has a zero round-trip time connection establishment.

Yeah, so... But if it's actually the cost of doing the lookup, that makes more sense. You're going to have to be sending a lot more messages to do the lookup. Yeah, exactly. So if we look just in the number of messages, we still get the 3.5x improvement.

Right, right. And just because you do much less lookups. And basically, the only messages you get to send is the provider record.

Interesting talk. Thank you, Guy. So I had a question. In the last slide, you mentioned this is going to be rolled out with the double hashing.

Why the two are coupled? I see three distinct things that you pointed out. One is the interface changes. The other one is the core work here, which is just making provide more efficient. So I'm curious why they're bundled up with the double hashing as well. I'd say mostly for practical reasons, because both need some refactor. And so, once we can refactor...
and include both features at once, then it's just easier. What's next? So when can I see this in Kubo, for example? What's the plans for things like load testing? Because I'm curious to see how it actually behaves in the wild. So, yeah, next is the implementation, which will happen once the double hash implementation is done.

And then, yeah, probably we'll be integrating to Kubo at the same time as the double hash.

In terms of testing, this can be done on the provider side only, right?
Yeah, it's just... So we can test it ourselves before the release? Yeah, it's just a client change. So it's a client optimization. We don't need to push it everywhere. It's just if we ship it in a new Kubo version, people running this Kubo version can start benefiting from it. Yeah, but in the testing phase, we can test it ourselves without having everyone to upgrade, right? Yeah, exactly. Okay, that's great. Thank you. Thank you. Thank you. Thank you. Thank you. Thank you. 