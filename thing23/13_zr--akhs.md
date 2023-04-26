
# Data Transfer Track Intro - Hannah Howard

<https://youtube.com/watch?v=13_zr--akhs>

![image for Data Transfer Track Intro - Hannah Howard](/thing23/13_zr--akhs.jpg)

## Content

So, welcome to the Data Transfer Track. My name is Hannah. I'm going to be sort of coordinating

this track. We have some amazing talks today. I'm going to start off just sort of like trying to lay some groundwork for the stuff that's about to come. Just give us some opening thoughts

as we enter the day. I kind of have three core goals with this track. I hope that you

will get out of the day. The first is that I want everyone to learn, level up our collective
understanding of the key concepts in data transfer. The second is I want to give some
folks who've done some amazing work a chance to showcase and demonstrate the state of the
art and get you excited about the world of data transfer on IPFS. And finally, I want

us all to do a little bit of reflection, trying to identify common goals, pain points, and

just improve our process as a development community working to make IPFS awesome, and
particularly in the area of data transfer. So those are our three core goals. But I want

to hear your goals. So, I would love it if you... I think for this size room we better

do volunteers instead of go around in a circle type thing. So I'm going to ask for five people.

And I'm going to need the coordinating folks to walk around with the microphone for this part. Yes. So five people who want to tell us all your name, what city you are coming

from, how you currently do data transfer on IPFS in your company, organization, or other

entity, how you would like to be moving data transfer, moving data across the wire, what

would make it easier, better, faster for you, and finally, what you want to get out of today.

What are the goals that brought you here to this data transfer track? And you're going to have a full minute for this, so organize your thoughts first. Cool. Can I get anybody?

Who wants to be the first volunteer? Thank you very much. I'm Tao Shen. I'm from
China, Chengdu. I came here yesterday. And we are building a Pandwa projects. It's an

authorized data layer based on IPRD and IPFS related technologies. And you know, these

days there is a Web3 festival in Hong Kong. And I mean, with the new year, the China government

gave many, many support for Web3 and data economy. And the China government has set

up a dedicated separate entity in the government to support the data economy for Web3 for future.

We want to leverage the data transfer and IPFS related technologies to transfer the

data, for example, in the Web3 data economy scenario. For example, IPFS and the Pandwa

and the Web3 CDN, something like that. Is that okay?
That's perfect. Yes. Thank you. Cool. Who else wants to tell us why you are here?

Hello, my name is Matt Hamilton. I come from Barbados. The way in which I mainly transfer

stuff on IPFS at the moment is sticking it on Web3 storage, generally via their Web API
or their little CLI tool. I'm getting stuff back that way. I'm involved in the Filecoin virtual machine. And the first thing when we talk to people about FEM and smart contracts and that is, oh, great, so I can access all my IPFS data from Filecoin, from the blockchain
now. And it's like, no, actually you can't. And they're like, okay, so how do we get it? It's like, well, first you've got to get it somewhere and then get it onto a storage deal and then it ends up on Filecoin. Well, where do we put it first? Well, probably the best thing is IPFS. So really I'm here to kind of find out about all of the other different ways in which people are getting data onto IPFS first, almost as like a staging area
and then archiving it, so to speak, onto Filecoin. So I'm interested in the myriad of different

ways in which people are doing that. Cool. Love it. My name is Walker. I work for Protocol Labs on the Launchpad team. We're the training
and networking program. So I'm here to better understand data transfer so we can help teach

it to everyone else. I love it. A little bit of learning. I'm Alex. I came from New York. I work with the Banyan team. We're figuring out, we're

scoping out a product for enterprise scale onboarding onto Filecoin. So it comes with a couple of challenges of like, what's the most performant protocol for me to get like a 32 gigabyte car file into some sort of staging area? How should I think about that staging area? Like, what types of protocol should I participate or like how should I index these car files? So I'm just trying to think, what are good tools to help me solve for this use
case and what are good protocols for me to make something that's performant and very robust? So looking forward to learning about that. Love it. Hi, I'm Ash from Mumbai, India. And I am super excited to hear about how Ido is so fast.

So that's one of the motivations is the protocol to understand what they have built.
Cool. Okay. Thank you to our volunteers. Excited to hear that we have folks coming from lots

of different places and with lots of different goals. I really like that a lot of folks are here to learn not just about the latest greatest, but also like how to actually use this. That

seems exciting and hopefully we'll be able to answer your questions during the day.
I'm going to give you a very brief sort of overview of the stuff we're going to be talking about today. And the sort of sections of the day. They're vaguely organized conceptually.

In the morning we're going to be kind of looking at some state of the art stuff. We've got to talk about the awesome Iroh BOW protocol. I don't know if BOW is the name of the protocol.

But I'm calling it that. Yes. Okay. Cool. And then we have another talk about Carpool. Then we'll have lunch.

And then in the first half of the afternoon we have a couple more like exciting new projects.
I'm going to talk to you all a little bit about LASSI. And then Jeropo is going to be

talking to us about the RAPID program. These are both clients that work on retrieving your

data quickly. And then we're going to have a panel to talk about a project that's been
going on for the last few months called the Move the Bytes Working Group. It's been a group of folks who've been trying to move the state of data transfer in IPFS forward. And we're going to be doing a little bit of a retrospective on that. So a bit of reflecting. And then in the afternoon we're going to look at actual applications. How are people building

on top of the data transfer protocols available in our networks? And how are they putting it all together? So that's the basic overview of the day. I do want to do just before we start a little
bit of table setting. Basically I kind of do this at the beginning of a couple data
tracks for IPFS events. And I find that the best way for us all to look at different protocols

and things that are available is to kind of have a bit of a shared vocabulary of terms
that we all recognize as important to data transfer no matter how we're doing it.

First thing I just want to tell you about, make sure everyone knows, is just a couple of existing protocols. Bitswap is sort of the original IPFS data transfer protocol.

It's now I think almost 10 years old. I don't know if it's quite that old. But it's approaching a decade old. It moves blocks over libp2p and almost everyone hates it. So that's exciting

things about bitswap. I also want to tell you about another protocol. Sort of the second protocol in IPFS. It is

called graphsync. It was written after bitswap. Unlike bitswap, graphsync moves entire DAGs

at once. You express your request in terms of whole essentially DAG. DAG means directed

async graph. Whole groups of blocks that you want to move over the wire. And you send it
to servers and they send it back to you. It is pretty widely used in Filecoin retrieval.

And everyone hates it. So that's our second protocol. They're actually both really awesome
protocols but they get a lot of, not a lot of love, a lot of other feelings.
So a concept that I want everyone to have in their heads is the concept of multi-party
data transfer. So most, in most of the world of the web, you download data from one source.

Probably the place you are most likely to have downloaded data from multiple sources outside of IPFS is BitTorrent. And our content addressing sort of like core of IPFS enables

us to download content from many sources at once. We should always, we always have the
ability, the potential of doing that. And how data transfer protocols do or don't tackle

the question of multi-party is an interesting one to have in mind.
Another concept, next concept is a thing called incremental verifiability. I just think of
this as as you download things, you should figure out if you're getting what you expected.
Again, content addressing allows us to verify the data we get regardless of the source we
get it from. And we should be doing it ideally as we transfer incrementally. So we're not
putting, I don't know, 10 gigs on a user's hard drive before we ask if it's the right data. So that's incremental verifiability. A concept that's going to come up a lot, especially

on the client side, is how you plan your queries. How do you decide, how do you plan how to
request data? Because often you are moving a lot of data. You may be moving a 32 gigabyte

Filecoin car file, or you may be moving a DAG that's much bigger than 32 gigabytes.
And how are you going to plan how to get that in one or multiple requests? How are you going to find it? How are you going to potentially split it up among multiple parties? How do you avoid duplicate data if you're using multiple parties? And how do you effectively utilize local caches? So this is the sort of query, how do you plan a query for data?

Another concept that is important when you start to think about moving data at the level above the block is the shared data model. This essentially means what is your protocol
assume about what both sides know about the data being transferred? So essentially, especially

like, so as an example, with like GraphSync, you're moving DAGs of data. That means that
both sides of the data transfer have to be able to understand and interpret the data
at the DAG level. And so that actually can introduce a lot of complexity, especially
as you have protocols that move data with a higher level of conceptual understanding about that data. One other thing to just think about as you

hear about protocols is how are they handling all the standard concerns for the server side?

Server side data transfer protocols tend to have some common tasks. One, how do you keep data moving across the wire when you're serving lots of requests? How do you minimize resource consumption? And how do you respond to malicious clients? So that's going to kind of matter

on the side of the person sending data, no matter what the protocol is.
Finally, almost every one of these protocols should have a plan for error recovery. We

found in networks, you drop connections, servers have missing or partial data, there can be

race conditions, consistency problems, and it's interesting to think about how are these

protocols dealing with that. So I just throw out all these terms as a way to think as you

hear about protocols. Think about how is it addressing this particular concern.

And then lastly, before we get into the talk talks, I want to just talk briefly about some
trends that I think I'm seeing common to multiple protocols in 2023. I spent a decent amount

of time thinking about data transfer and hearing about what folks are working on in the Move
the Bytes working group, so I've noticed some things that are cropping up more than once.

The first thing is that I'm seeing a lot more folks really interested in HTTP. When we started

building IPFS, there was an assumption that we had to build from the ground up with a
new networking stack. And as time goes on, you're starting to see more folks think maybe

we don't need to deal with data transfer at the network protocol level, and more we need

to think about at the API design level, essentially how do you work with existing protocols like

HTTP to move data quickly over the wire in a content address context.

Another thing that I've seen from a couple folks, this isn't a universal trend, but it is something that I've seen from a lot of folks, is a desire to just deal with the lowest

level concepts in our ecosystem, blocks and SIDs. I've noticed that a lot of folks who are thinking about moving large amounts of data fast want to sort of do away with needing

to understand at the protocol level about anything other than the block and blobs of

bytes. Essentially, it's like we're all going back to BitSwap, as maybe it wasn't the wrong
idea to begin with. I want to be clear that this is not universal,

and I see other folks who are actually getting greater value out of higher level protocols,

but this is something I've seen from a few different places, where people just want to deal with blocks, because blocks are simple. Cool. One final thing, because you're going

to hear a couple talks about this today, is the concept of multi-protocol clients. At

various points in the development of our ecosystem, we've had this idea of, well, if we just build

the one ERR protocol that was super awesome and handled every case in just the perfect

way and we got every single person in the ecosystem to adopt it, then we could have
fast data transfer. You can see that actually, when you look at that IPFS principles, they're

actually pretty clear that the transport shouldn't matter that much. That opens up the question

of how do you build smart clients, clients that can speak more than one data transfer protocol to get data. That's something that I'm seeing more people start to think about,
where maybe we need to not look for the perfect protocol and we need to start thinking about how do we work with the many protocols with different strengths that we have to get our
data quickly. Those are some stuff I've seen, I think. This is the last slide that I have

and then I'm going to hand it over to our first speaker. Just to cover some things that have happened in the last year. If you haven't been involved in the IPFS ecosystem for a

long time, the last year has been a really interesting period for data transfer and for

a lot of things in IPFS, starting with IPFS Thing. It was a moment a year ago when we

met where a lot of folks were acknowledging as a group for the first time that the things

we had built so far needed some work and we needed to think about how we can do things
better. We met about a year ago in Iceland and we talked about all the protocols and

why people hate them and how we could do a better job. In November, there was IPFS Camp

and that was another larger group discussion about data transfer and out of that came the
Move the Bytes working group. Move the Bytes began with the goal of replacing BitSwap in
four months and we started meeting biweekly in November of 2022. We shared a bunch of

knowledge, we saw some awesome protocols develop. Many of these you'll see today. We have not

replaced BitSwap in four months. I always thought that was a little ambitious, but we are making some awesome headway. I thought it was another pretty interesting event for

data transfer and IPFS was Number Zero starting over and attempting a complete rewrite of

IPFS to try to attack data transfer at a more fundamental level with a ground up rewrite.

That actually is a great segue to our first speaker, Rudiger, who is going to talk to you about the awesome data transfer that they have built in IRO.

