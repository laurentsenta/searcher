
# Data Transfer batching Techniques featuring Blake3, CAR Mirror, and more - Philipp Krüger

<https://youtube.com/watch?v=VjZrOg1O-ac>

![image for Data Transfer batching Techniques featuring Blake3, CAR Mirror, and more - Philipp Krüger](/thing23/VjZrOg1O-ac.jpg)

## Content

Hey everyone, I'm Filip, I work at Fission, and I want to talk a little bit about, well,

not a certain data transfer protocol in particular, but more like about a bunch of data protocols

and how they compare. I think Hanna gave a great overview this morning about different
kinds of ways or design goals that data transfer protocols can have, and I'm going to do somewhat

something similar, so maybe some repetitions, I'm sorry about that. Anyhow, well, let's

not get ahead of ourselves. Basically, when we began at Fission and we were thinking,
well, data transfer and IPFS, well, what do you do? Well, you use IPFS pin. So we would

have a server, we would have some clients, and when we want to transfer data, we would pin from the server and fetch stuff from the clients. This turned out to be not amazing

sometimes, I mean, we've heard this story a bunch of times, so bitswap is behind IPFS pin, and that can be difficult sometimes because of various issues, DHT issues, connection

management, and lots of WANs, like, I think, yeah, that was mentioned in a previous talk
also, but this talk is not about DHT issues, I mean, this is the data transfer talk, or

track, not the content writing track, so let me talk about the transfer issue, which is
basically what we found is, well, you fetch the root block in bitswap, then you find more CADs at the client, then you fetch more blocks, and it continues, so you have a bunch of round
trips. How do you get rid of this problem? Well, you do batching. And so most of the

data transfer protocols that we see today, that people are building, are basically trying to solve this by batching, but it's not obvious how, like, do you use bigger blocks? Some

protocols try that. Do you send car files? Well, they're not bigger blocks, they're just a bunch of blocks together. So, yeah, if it's not that obvious, let's talk about some of

the design goals you could go for, that you could aim for, when you build a new data transfer
protocol. So, we've already established we want to have few round trips in our protocol,

but what is also very important, and again, Hannah mentioned this, I think I heard of this term first time last year, the IPFS thing, and I think it's a great term, it's just incremental
verifiability. So this is what sets us apart, or IPFS in general, I think, apart from, like,

just normal HTTP transfer, FTP, or whatever. Basically, is the data that I got, that I

was transferred, part of what I actually care about, or is it something unrelated? Or I also like to think of it as, like, the untrusted data buffer size. So you have some buffer that you allocated at the, whatever it's trying to fetch, and it has some certain size, and
you don't want to, like, have too much untrusted data that you fetch, because it might be a
DOS attempt, it might just be something going wrong, and obviously you don't want to spend
resources on some data that you don't care about. It's also part of minimizing trust. When you, like, talk to some other peer in the network, you have a different kind of
relationship you need with them. Like, I mean, we don't need to know who we're talking to

if all we care about is just a SID. So that's a nice advantage that we want to keep. So that's incremental verifiability. Another thing that some protocols are aiming for is

query support. It's some protocols, again, not every protocol is trying to do that. So

for example, for GraphSync, that would be, well, you have a root SID and you have an IPLD set, or you have a root IPNS, I guess. Or I think a different form of querying would
be range queries and files. Another thing that some protocols are trying to solve is

deduplication. So when your problem is not actually transferring some data for the first time, but you actually want to sync something, mirroring, right? That's what Carmira is trying to solve. Or it also crops up when you have data transfer, cold data transfer, kind of,

but it stops and you need to resume it. Although the resumption, I guess, and we've seen this with the Blake 3 talk this morning, you can also solve this via just querying the parts

that you, that failed to transfer. Another big thing with deduplication is there are

some protocols and some Merkle trees that are really built around structural sharing. So for example, if you have a proletary or if you have a hand, you, and you update this
very frequently, you may want to take advantage of structural sharing. And if you want to
sync the next version of this hand, and you want to only sync the diffs between basically

their new version and the last one. And finally, something, and I think this is not something
that I thought of in advance, but more like that is a result of the protocols that were built. Some protocols have like a lot of flexibility in what kinds of DAGs they support. For example,

do you only support Blake 3, the hashing function, or do you support like arbitrary IPLD data,
or do you need some kind of ADL pre-installed? So ADL being like the advanced data layouts

in IPLD. So there's quite some like flexibility in the design goals of a data transfer protocol.

And let me just basically run through all of the protocols that we have and try to like

figure out where they lie. For that, I have this table and basically I'm going to compare

like IPFS pin, which is basically BitSwap, just sending a car file, then CarMirror, GraphSync,

and the Blake 3 and BIOS stuff. The reason I'm doing this is not only like trying to categorize this and put things into like neat little boxes, it's also like trying to explain

why we have these many protocols, because some protocols focused in different design goals firsthand rather than others. I think that every protocol in theory could like try

to solve every design goal, but it's just that they weren't tackled in the same order
for every protocol. And yeah, like everyone needs to simplify in some ways in order to

get to something that is working. So IPFS pin slash BitSwap, I just gave it like tick.

This works for all design goals except for a few round trips, because in practice you get incremental verifiability since BitSwap actually just forces you to, I think the threshold

of like the block size is configurable, but like a conservative estimate of what you can expect is like blocks shouldn't be bigger than 256 kilobytes. And if you build your
DAGs this way, and like in practice, most DAGs and IPFS are built that way, then you'll end up only having to allocate like a buffer of 256 kilobytes before you can hash it and

see, oh yeah, okay, this is the data that I care about. You also kind of have query support, but you need to do it on the client where I'm really stretching like the design goal here. But if you think about like few round trips, not being the design goal, then

yeah, you kind of have query support. If you have like a hand, you can fetch one block at a time and you get through and do a certain query. You also get data duplication and DAG
layout flexibility again, all without few round trips, which is a big kind of bummer.

With car file transfer, so there's something like, I think Web3 storage is basically doing this and lots of other places are doing this today, which is like the basic way of doing
a batching of a block transfer. You get very few round trips. Yeah, you're just one, one
round trip. You ask for something, you get a car file and it's packed with whatever you want. You don't get incremental verifiability out of the box, I guess. But I put in like

technically, you could put the orange kind of middle in between thing here everywhere,
because just depending on how you pack your car file, of course you can do that. But this
is really just as simple, like imagine an IPFS DAG export and then serving that over
HTTP is basically what I'm talking about here. And so you kind of get incremental verifiability
if you do like a topological sort of all of your blocks inside the car file. But it's

not technically guaranteed. Someone may send you something that is not that. And yeah,

I don't know. Basically it's not in the car file spec, but whatever. It's very easily
achievable. Query support and deduplication need to be built on top, but it's very flexible
in terms of the deck layout. You can put any kinds of blocks in there.
CarMirror is basically trying to just improve car file transfer. It's literally that plus
doing it in rounds. And it is not focusing on query because that wasn't what vision traditionally

had problems with. We just wanted to sync data faster. If you have a bunch of updates and you've been offline on your phone for a while and you want to sync that, that should be fast. But our DAG structure at the end of the day was unfavorable for that with BitSwap.
And so it took a bunch of time sometimes to get through all of the levels in our DAGs.

And so that's why this is built. It's basically just batching on a different level and a little
bit of deduplication on top of just sending car files around. GraphSync I think actually achieves most of these things. I think there is an extension for deduplication. So essentially the idea being you can do a query and you can add,
like, don't send me any of these bloom filter blocks. I think that works if I'm not wrong.

You can send a raw list of sys that you want to exclude, but you can't actually send a bloom filter yet. But I think in principle you could build an extension. And I guess,

again, that's probably the case for most of these protocols, that you can evolve them
into getting at every design goal here. And lastly, I think very interestingly, the Black3 and BAU stuff, it doesn't take a lot
of these boxes, but it's extremely fast. So that is great. And it also has amazing chromatography

and a lot of verifiability, even in a way that is, I think, very interesting. And what
I want to do now next is basically let's look at this and try to see how we can take some
of these ideas and just transfer them over, no pun intended, to other protocols. Right?

For that, let me repeat a little bit of what Rudi has talked about this morning. Basically
Black3 and BAU is the idea that you have huge byte arrays that you want to transfer, and
you create these, you chunk these byte arrays, and you create this Merkle structure, this
binary tree. On top of that, we're basically here, every line is this hash is two upwards.

And then you send over, starting, when you want to fetch a sys, you send over starting
with the tree, with a, what was it, preorder, left first, depth first, traversal. And so

it ends up being something like this. Basically, once you have these hashes, you can verify, oh yeah, these hash to the state at the top. And so you know, oh yeah, this is the data
I care about. And it continues. And again, you can, at some point, you can hash stuff again and realize, oh yeah, this is the data I care about. Yeah, that continues. But obviously,

there's some overhead to this. And so the idea that they had and that he presented was, hey, let's just take out like one layer of this internal Merkle tree. And what we get

is we've reduced some overhead here in what we need to send, because we need to send some

hashes fewer. That's really cool, I think. So essentially, imagine, for example, you
have two peers who've never seen each other. They want to talk to each other. They may
not trust that they actually, or the other side actually sends them the valid data. But
if they've been talking for a while, if there's some kind of relationship between them, you may trust for like a lot more data at the same time to be valid. So essentially, you
can have, at the time that you transfer, you can choose the level of incremental verifiability

that you want to have. Because traditionally, in IPLD in general, and with BitSwap, we've

had to choose the level of incremental verifiability in advance when we wrote these blocks, right?

Because we choose the chunk size in IPFS and UnixFS in advance. But what if we could do
this every time and choose it depending on what the connection is we talk to? So can we do it? I think, yeah, we can. And the idea is basically this. So let's take some IPLD
block. This is actually like one node and a hand. It's in practice, it used to be like

DAG C-BOR, but I've rendered it as DAG JSON, because that's actually human readable. And
so we have a bunch of nested arrays, blah, blah, blah. And we have some data. And then we have a link to some other block. And so this block isn't actually that big. And that

means that basically the ratio of how many hashes appear in it, so how much of the block
is actually a hash and how much of it is actual useful data, so to say, is not that good.

And so the block that is actually linked to there is just this small thing there. It has a bunch of bytes, just a byte array. So what can we do? Well, we can just inline that.

And I've just done it like that. So if you look at this, it's basically just taking the thing at the bottom and putting it there where the hash was. But we keep the original hashing

function that was used. We keep the code that was used. And we have some sort of special
marker that, yeah, this was inlined now. Think of it like taking the SID and just chopping
off the last 32 bytes from the hash in the SID and instead putting the block there. And
again, before someone comes up to me and says, like, identity hashes and SIDs, no, it's not
the same thing. It is not the same thing because when you actually compute the hash of the
whole block on the receiving side, you deconstruct this thing and split it into individual blocks again. So where identity SIDs you can't replace with the SID that represents the SHA-2 hash

of what's inside the identity hash SID, this does. And I'm sorry for everyone who's not
familiar with identity hashes. Not that important. Basically, it's something different. But what

I think is pretty cool is you can also approach the limit of unverified data in the unverified

data in the buffer from the bottom instead of from the top. Like, for example, so that
is something that Blake 3 kind of did. And Blake 3 and BAU. You can choose the size all

the time or at the time that you do the transfer. But also, you could technically do the same

thing, like, if we go back to this, you could also choose to not, like, inline all of the

bottom parts, but inline the let's say the second layer. And instead, you have to, like,
send a bunch of hashes that you did not inline yet from the blocks below. So imagine, like,

not sending H6 and H7 and not sending H5, but instead sending the hashes in the layer below. And you can do kind of a similar thing in with this idea and with IPLE. And what

you end up with is basically when you send something, you agree upon in the beginning
on, like, a certain size of whatever the incremental verifiability should be compared to. And then

you can take a block and inline hashes so long as the block is still just slightly below

the incremental verifiability buffer size. So that's how you maximize, like, the ratio
of how much hashes you send in terms of overhead to how much actual data you transfer. Which

I think is pretty interesting. For example, if you have, like, let's say very high frequency

write data structures, you may end up having lots and lots of small blocks so you can have

maximum structural sharing between revisions, but you can also transfer them on cold calls
very efficiently without incurring all of the overhead of all of the intermediate hashes. And I think doing lots of hashing, Blake 3 seems to be a case in point that that's not
a bad thing to have small blocks that are hashed together and put into a structure. It can be fast. Is this an idea for Carve V3? I don't know. Maybe. Maybe this is something

for that. I don't know. Take it, do something with it. Yeah. That's basically it.

Another idea is, well, queries are kind of like code. And I know that IPVM as a project
is starting up. We have FVM nowadays. So perhaps we can take and simplify all of the querying

code, which has a certain limit today on how, like, complex these queries can be, and replace

it with code. I mean, this is not the first time that I'm proposing this idea, but I'm hopefully taking it a little bit further. Basically, there's a problem also, I think,
with IPVD selectors and querying, since you need to have these ADLs. So you need to have,

let's say, every node needs to know about, yeah, you have a ham data structure. These are the support ham data structures. And these are the supported byte array abstractions and the chunking methods. And then you need to have some code that knows how to, like, transform path queries or range queries into these multi-block data structures. And it's
a certain set that you need to agree on with all of the peers that are talking to each other. And I think that is one of the problems why adoption has possibly been harder previously.

So if you think about it, there's people who are coming up with new ways of structuring Merkle data, for example, or different types of hams and different encodings, and they

can't really participate in all of this IPVD with ADLs world and IPVD selectors, because

they would have to convince IPFS implementations to adopt these IDLs. And even like on the

web, on the front end, we don't even have ADL support in browsers yet or in JS IPFS.

And so I think maybe there's a way of using, let's say, WASM to agree on something in advance

and use that to overcome all of the agreement that we would have to have on ADLs, right?

Basically the idea would be, well, you send some WASM that goes through your block store and collects all the blocks you care about. There's lots of problems with this. So for

example, you need to transfer the WASM file in advance. It can be pretty big. In practice, I guess you would be restricted to a bunch of programming languages, which have good tooling to generate small WASM files. That's a very small set of programming languages.

There's some concerns about, yeah, what if your query runs forever? You suddenly have something much, much, much more complex.

Although I think that queries on their own may have had issues with how making them more flexible at the same time makes you more susceptible to DOSing or resource hogging, basically.

Maybe all of that gets figured out, and it's easier once we have IPVM, because they need to tackle these problems anyhow. So just throwing that out there.

And then if we have queries as code, deduplication is kind of like a query. So basically what you say is, well, I already have a bunch of blocks, and I want to find the diff.

So you just write some code that is preloaded with all of the blocks you have. You send it over, and this code basically runs through the remote state and figures out what the diff is and sends only over the diff.

And this is, I think, maybe interesting or nice, because you can conceptually split data transfer into some preprocessing phase, which may be supported by WASM or something like that, and then a transfer phase.

I hope, I think, I made up some time. I basically just want to throw out these ideas. I'm excited for what's happening, and if you have any questions, please shoot them at me.

So on the BAU Blake 3 stuff, do you still get the same level of advantage if you have a really deep data structure, as opposed to, because what number zero, for example, looks at is sets, so it's really evenly done?

What about with something that's narrow and deep instead? Yeah, I think that you would need to, I think the N0 stuff for now doesn't support that, but the more general idea of just inlining the contents of SIDS could theoretically work that.

So if you have just a very, very long, let's say, linked list encoded as a Merkle tree, or a Merkle list in that case, or call it a blockchain, whatever, what you end up doing is you just inline all of that until you have something that is a big size, and you can do it just going down.

So it doesn't matter what the topology of that DAG is. I mean, there's some questions around, like, if you don't have a linear kind of tree, but you actually have a tree with multiple children, where do you go down first?

Do you go breadth first, do you go depth first? So there are some open questions to that, but yeah.

