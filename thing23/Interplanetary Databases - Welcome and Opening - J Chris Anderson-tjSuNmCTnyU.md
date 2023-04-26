
# Interplanetary Databases: Welcome and Opening - J Chris Anderson

<https://youtube.com/watch?v=tjSuNmCTnyU>

![image for Interplanetary Databases: Welcome and Opening - J Chris Anderson](./Interplanetary Databases - Welcome and Opening - J Chris Anderson-tjSuNmCTnyU.jpg)

## Content

Today we've got talks from I guess like five or six different database technologies and

what's cool about them of course is that they all use IPFS, IPLD and what we're hoping we

can do today and this may be more like open discussion after lunch is talk about opportunities

for interop and like the flexes we can do to show the other parts of the industry how

much power we get from having these IPFS-based data structures.
So my theory is that almost any one of the technologies we're going to hear from today,

if the engineers sat down for an afternoon they could figure out a way to cross-index
or reuse data structures, share it across all the databases. So it's not that you get it like for free no matter what, but it's like if you want to be able to do that kind of interop, I don't think you're going to be able to do it outside of this community. So pretty excited about that. So let's see, who in the room here is speaking today?

I think me, the other speakers are probably drinking their coffee still.

So yeah, I guess while we've got some time before the first talk from Aaron Goldman,

maybe it makes sense to open up that discussion a little bit and see who here is like building
toy databases or real databases. Anyone else besides me make databases for fun? All right. Cool. Do you all want to like holler out what your deal is? Should we like start from over here? Sure. So my name is Enrique.

I'm with Protocol Labs, with Consensus Lab more specifically. I mean I have some background in distributed databases from my past, from my career.

Right now, what I'm thinking of is, I mean it's still something in the very early design
phase, but it's like a distributed universal database for data analytics, where everybody

can share data into it and anyone can run nodes of that database and then the queries

can be distributed all across this computation universe you have.

So that's it. Cool. Yeah, so for people who just walked in, we've got like a half hour before the first talk starts, so we're going around and anybody who does database stuff, just kind of like mentioning what you do. Do you want to hand it behind you? You can also take the mic if you don't do database stuff. Yeah, so I'm Lucas, or Magic6k on the Falcon Slack and everywhere.

So I work with Protocol Labs also, and I'm working, like I'm building this distributed

block store, which is like better data layout that's very easy to put on Falcoin, and at

the same time it also actually scales, so it's not some basic KB store that we abuse
as a block store, it's just like block store. So I was thinking of building a block store that goes fast and very big. Yeah, that is actually a real helpful thing to do, because we all like to just be like,
the block store, it's got it. I'm Daniel, I used to work on OrbitDB, and now I work on my own database, and I'm trying

to make it so the database can keep moving forward after all the peers, all the clients

have been unreliable and gone offline.

Anybody else got database stories to share? Background in databases? Hi, Daniel, at Ceramic now, recently, well, 3box, and prior to that I was at InfluxDB,

helping them build their query engine over there. So very interested in databases and excited to see how they map to the Web3 space.
Cool. Yeah, and I think your colleague Aaron's up first.

So all right. I'm his colleague Aaron, who's also at Ceramic, building a database, and I'm up first.
Nice, that's great. Well, we're going to learn more about that database for sure. And so trying to, you know, I don't want to run early by starting the talk now, so maybe

there's a topic that Daniel brought up on the kind of online discussions before this

that might be an interesting one, and maybe we don't get juiced up to really get into it for a little bit, but it's about like how, I don't know, do you want to say what your
thoughts were? When you have reliable peers, it's nice, because you can do like kind of very specific replication

of data, and then you have kind of these, when you're syncing with services, it's a

little bit different, but yeah. I guess, okay, so when we were talking about this before, my thought was as a transaction

engine, like you can do something that's like at the data layer, I mean at the like, your

application blocks, right? Like you're writing some tree updates out, and now you've got IPFS CIDs that you want

to sync around. And so maybe let's call that like replication strategy A, and replication strategy B is

going to be more like, now let's wrap those up into a car file and send that around.

And then there's probably like replication strategy C and D, right? That would be more about like, I guess, leveraging the immutable identifiers and stability like

that. But yeah, I don't know, are there, can we classify the possible databases in the IPFS

immutable IPLD realm into kind of like you would outside of it, oh, this is the transaction,

oh, Apple LTP, et cetera. And does that map to the verbs that the IPFS infrastructure makes available to us?

So to me, that's an interesting open question, because I'm having to ask myself those questions every day about like, how am I going to implement this part of the transaction layer or whatever?
So I don't know if anybody has thoughts on that.

Database geeks. We'll write on.

Let's see, does anybody else have any like opening topics you think we should talk about,
like use cases for databases? The thing that you really wish was solved, but it's not, anything like that? I think we need B-trees in the advanced data layouts, because right now you can index stuff

that's in IPFS, but we are not very good at putting indexes themselves into IPFS.
So let's say I want to do Wikipedia over IPFS. I want an entire inverted index, so I can do full text search of that, and those blocks

themselves should be in IPFS in a way that I can run that query efficiently in the browser. Right, and then there's just a bunch of really cool things that you get for free when you

build it that way. Like Franz's talk yesterday about the media archiving stuff that he's doing with like
a European government organization was enlightening, because he was as an implementer not trying

to be theoretical about the database stuff, but he was still hitting on the same patterns that we're seeing when we build. So like you've got a collection of content, and you've got it archived in IPFS, and you

can put, this is what, I don't think he did this, but he was like, this would work. You can put it in Elasticsearch, and then have Elasticsearch, when it returns the results,
that also return the CIDs, and then use those as proofs to replicate the underlying data,
and then rerun the query on the client. So it's that kind of stuff that when the index is represented in a IPLD graph, then you don't

even have to jump through hoops to do that, right? It's almost like thinking in terms of the proof in the related block set is the easiest
way to do it. So yeah, there's a bunch of benefits to getting indexes into the data structures. I think about things like Google searched the web for us, and they built an index of
the web, and we are welcome to use their search engine, but we are not welcome to use their index. To some extent, we can build much more powerful search algorithms if the thing that we are collaboratively building isn't, I build a search engine, and you build a search engine, and someone else builds a search engine, but we collectively share the effort of building an index, and now we can all run whatever algorithms we want, because there is a large shared index. Right, yeah, and for the folks at home who aren't fully Merkle-brained yet...
What? Surely no such person exists. It all sounds super dangerous. Let's collaborate on indexing together, right? No, these data structures are immutable, so you can't mess it up. You can literally do all the indexing you want, and I can totally ignore you.
And so that opens up the space for data collaboration in a way that is also, I think, really powerful.

If literally every operation has to be a fork, then letting people operate on your data is

lower risk.

