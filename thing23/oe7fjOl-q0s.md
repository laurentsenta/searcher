
# Welcome and Introduction to the Content Routing Track - Masih Derkani

<https://youtube.com/watch?v=oe7fjOl-q0s>

![image for Welcome and Introduction to the Content Routing Track - Masih Derkani](/thing23/oe7fjOl-q0s.jpg)

## Content

Hello everyone, thank you so much for joining me in the content routing track. For those

of you who don't know what I look like, refer to that picture above. You will recognize
me around. My name is Massi, I'm from the IP&I team, part of Bedrock, and here to talk

about content routing. First of all, welcome. Emojis don't show up properly, but that should

be a Belgian flag right next to it. We are in a city famous for its beer and famous for
its chocolate. I mean, apart from faster lookup, what else would you really want in life, right?
So thank you so much for coming, really appreciate it. So what is content routing? Now, bear

with me, because I'm aware of the amounts of intellect in the room. We just want to
walk through this question together. What really is content routing? If you look at

the previous work, we can see that content routing is typically referred to as finding

who has a CID. So by content, we really mean just a CID or a content identifier, and by

who we mean the peer ID and some addresses. So far so good, right? This all makes sense.

So this simple operation is the very essence of content routing in motion. It's the first

stepping stone, pretty exciting. But wait, hold on. What about provide? For contents

to be found, I need to provide it somewhere too, right? So sort of providing content to this whole network, telling people that I have a CID is also part of content routing, right? So let me backtrack a little bit. These two simple operations are the essence of content

routing in motion. After we get through turning our famous cat picture into a content addressable

data, then the very next step is to tell people about it and have people find where to download

that content address data. So far so good, great. Hold on, but find sort of doesn't always

result in just peer ID and address. Sometimes it has some extra information in it. Is that
content routing? Sure, that is content routing too. So I'm going to backtrack again. These

three simple operations, I'm going to stop you right there.
So what about find itself? Find might not just want extra information, might want even

less information, just a peer ID, just for speed's sake, right? What about providing

content with a you can that enables others to provide on your behalf? As we can see,

this is progressing, it's going more and more complicated and it's kind of a fuzzy area
we call content routing in general. It gets worse. What about IPNS? IPNS sort of points

to a content. It's like an alias to a CID. The characteristic of publishing an IPNS record

is very similar to the characteristic of publishing a CID. So is IPNS part of content routing?

It is all a little bit confusing and overwhelming, right? So we have this fuzzy notion of content

routing, which includes many things. It's different depending on the system that is

interacting with the network. It is a very not exactly well defined, but it is extremely

evolving, right? It's changing all the time. So let's take a look back and just look at

what changes happened in content routing. If we look at 2015, that was when the initial

IPFS was released. The content routing mechanism in the original IPFS was basically made up

of two mechanisms. I call it a structured lookup, which uses a structured peer to peer
networks, DHTs and so on. And somewhat an unstructured lookup, really hybrid of structure

and unstructured if you want to be very pedantic about it, but it was a there's a concept of

just gossip and just broadcasting to the network and see what we get back. These were the two
fundamental beginning, the initial mechanisms for content routing.

Then BITSOP 1.1 and 1.2 came about. And these two operations, these two advancements focused

on improving this unstructured way of lookup, this gossip mechanism across the network.

Then KUBO 0.5 happened. This is way before my time, so I've done a bit of archaeological

work, talked to folks that have been here for much longer than I have. And this KUBO
0.5.0 introduced significant improvements into the structured way of looking at data.

DHT became much more sophisticated, much more healthy. And then after that, we see a series

of work that pushes on that front. We have Hydra boosters, which introduce highly available

DHT nodes with shared knowledge that provide high likelihood of you hitting a node that

has significant information about the DHT, such that it would then improve your lookup
success significantly. With the great work of Stewards, specifically
ADIN, we have full RT, full routing table implementation. This is an extension of the

vanilla cademlia routing table, where rather than having a logarithmic way of finding peers

by number of hops being logarithmic to the size of the network, you basically try and
store the full state of the network in the hope that you will only need a single hop

to find out who has the information. That significantly improves the speed of both
lookup and providing. And remember, if we refer back to the previous slides, we have
this mixture of all these operations that are all included under this umbrella of content
routing. So then something really interesting happened.

The very beginning of swappable content routing, this is what I call it. And this swappable content routing enabled just collaboration and enabled connecting multiple content routing

systems together, beginning with just one. So we basically took the DHT and took the

previous structured and unstructured things that we had, and we wrapped it around APIs
that then allows us to seamlessly exchange, replace them with another mechanism. And the
point was to introduce tuning knobs. This is one of the first instances of introducing

these tuning knobs that allows implementers, allows people that are interested in sharing information. And remember, we do all of this to have content address data in motion to
really optimize for their use case. Reframe evolved into HTTP delegated routing,

a much simpler API with much more focus on human readability. Specifications came out.

This is now shipped into, shipped in Kubo and allows, and has reasonable defaults, which

allows you to integrate your own routing system into it. The routing system that makes sense to your system, to your network, without having to pay the cost of the routing system that

should work across a wide area network. This is pretty exciting.

After that, IPNI happened. This is the beginning of bridging gaps between the network. So before

IPNI, remember, we have multiple content routing networks. We've got IPFS, which has data,

content address data. We have Filecoin, which has significantly more content address data with much, much different characteristics associated to it, like for example, longevity

of the storage. But these two networks are completely separate. And the reason for them being separate is that the data in Filecoin and other content address networks is just

massive in comparison. There is a huge amount of it. And the existing sort of content routing

systems are not really designed for that type of scale and publication. That's where IPNI
comes in, to bridge this gap by making explicit design decisions that we talk later on today

about in order to bridge this gap between the two networks. With the birth of IPNI, you now can look up content address data full stop. Let's pause

there and think about that a little bit. This is the thing we've been striving for. Certainly

not the destination. It's just the beginning. We are just scratching the surface. But we are delivering every day more and more on the promise that, hey, if I have the CID,
I just want the content back. And I want to be able to retrieve it, and so on. For the
retrieval, we touched on that a little bit yesterday on data transfer track. But this
development is pretty exciting. We have bridged the gap. We have faster lookup across the
board with some explicit design decisions. And now we are starting to focus on concerns

that to me are cross-cutting. Like, for example, privacy. And this is a pretty exciting advancement

in this whole evolutionary timeline of content routing. Because we're not so much worried

about just making it work such that we can find the data no matter what the latency.
We are finding the data. We are finding the data fast. And now we are focusing on the sort of solutions that are cross-cutting across any content routing system no matter what.
Like privacy. In preserving the reader's privacy, they would be preserving the writer's privacy.

This is just the very beginning, but it is an extremely exciting time. There's one extra
data point here to point out. The lines in the graph, they're not linear. More than half

of this stuff happened in the last three years. Think about that for a second. This is an
acceleration of work and advancement in the area of content routing just in the last three
years. That is something notable. So what are the evolutionary trends here? Because

really that's what we care about when we are looking at a timeline. What are the trends
here? Lookup is getting faster. Both lookup and providing is getting faster. We are becoming

significantly more scalable when it comes to making content address data available full stop. We have systems that accept content routing, content address data by the billion
and in return provide you with sub 10 millisecond lookup latency. We have increasingly more

tuning knobs that allow you to share content routing mechanisms and swap them with another
system such that we stay true to that promise of giving us it, find me information, giving

us it, tell people about it without having to force the users to learn about really complicated
stuff. We are increasingly going towards a world that has interconnected networks right

now. Finding data from IPFS and Filecoin together is old news now and to me that is an exciting

success. It really should be relevant to the user where the data is and we really should

focus on how the user can push data and get the data back. And like I mentioned we are

entering an era of cross cutting solutions that are agnostic of the content routing algorithm

or mechanism itself and more focused on properties that a sound content routing system should

possess for the good of the users like privacy. So if I want to sum it up in one sentence

this is what I came up with which is we are moving towards a fluid content routing. What
does fluid content routing mean? Imagine a glass of water, you pour it in different glasses

and it takes the shape of the glass. So this content routing becomes more flexible and
becomes more sort of ubiquitous. A cross cutting layer that has distinct boundaries but forces

users less about knowing how it actually works and more about its true functionality. And

to me that is something really something to celebrate. It's credit to the work of all
the communities that's been happening over the last 10 years or so. I think it is certainly
something to celebrate. So what does it look like today? This content routing network that
we're talking about, what does it look like today? This is the diagram that I put together to kind of show you the landscape of content routing today. We have users on the left hand

side. These users talk through the network via IPFS implementations like Kubo. They may

be using the gateway to find information. They also may be contacting IPNI directly

to find information. We have this mist of information that still exists undeniably which

is BitSwap. Unstructured, you just have to be in the party to see what's happening. Unstructured,

resilient in specific use cases, not so much when it comes to bulk publishing and bulk

lookup. But certainly there still. We have Hydra nodes still around. These are those

red dots that you see. We go through the changes that's happened in Hydra's a little bit in
the IPNI talk. But Hydra's existed and now they're reduced a little bit but they're still

around. We have direct connection between Kubo nodes now into other delegated routing

systems such as IPNI which is also exciting. This was shipped since Kubo 018. So the role

of Hydra's in terms of propagating search through multiple routing systems is not as important anymore. I also find that really interesting because anything that makes any

of these nodes or collection of any single node more important in a collective network
to me is a really welcoming development. On the IPNI side, we have cultivated a platform

that enables a whole new array of connected content routing data. This is something that

would have not been possible with the sort of design decisions that we've explicitly made in IPNI. This is a bulk content that is provided by providers like NXT storage,

Penyatta and so on. We also have IPNI deeply integrated into storage providers, Filecoin

storage providers. It's been integrated into Boost from day one and it was integrated into
Filecoin in its early creation. We are starting to see a whole new way of interaction with

this whole system which to me kind of points back to that idea of fluid content routing
that I touched on earlier and that is the use case of RIA which is using the decentralized
Saturn network to then look up information using LASI. I think that is also pretty exciting.

So what are we going to cover today? Today we are going to take a closer look at latest
developments in this big piece in the middle. Then we are going to look at what is IPNI,

how it is advanced, where is it going, just understanding the direction there. We are
going to have a break and after that we are going to come back and talk about those cross cutting concerns that I talked about like the privacy and how this cross cutting concerns
that are putting the user at the center of this interaction are affecting other routing
systems like DHT and IPNI. And we are going to wrap it up with a talk on how we are moving

towards a more decentralized IPNI network by removing barriers in terms of exchanging

information about what is available and also enabling other nodes in the network to get
that information and become up to date. So this is sort of the overview of the track.

I hope you are as excited as I am and with that I am going to pass it over to Guy to

tell us a little bit more about what DHT has been up to in terms of scaling that ability to provide information. Thank you.

