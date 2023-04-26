
# Hole Punching in the wild - Max (mxinden)

<https://youtube.com/watch?v=R-ToBsdlEk4>

![image for Hole Punching in the wild - Max (mxinden)](./Hole Punching in the wild - Max (mxinden)-R-ToBsdlEk4.jpg)

## Content

Welcome to my talk on hole punching in the wild. It's a small disclaimer here. We gave

this talk previously at FOSDEM in 2023, so this year actually. But I thought maybe this

is actually useful for this audience as well to hear this talk or this talk in a similar
way as like a lot of work went into general like hole punching measurements and hole punching

in general in libp2p. And so this is kind of carrying this information to this audience
as well. Cool. So what I want to talk about is libp2p, libp2p's hole punching mechanism.

We'll go into what hole punching actually is and then hole punching deployed onto the
IPFS network or other networks. And then last step being measuring the success rate of this
hole punching mechanism off libp2p. Okay. Yeah. We did this together or actually there's

a third person as well, which is Elena, building the whole project around measuring hole punching.

And then Dennis collected a lot of data, executed the whole measurement campaign and so on.

Janice also helped a lot on the measurement campaign and collected all the data. I'm still listing Dennis here as we prepared the talk together, but unfortunately Dennis can't make it today. Short introduction for myself, Max, software engineer at Protocol Labs, maintaining

libp2p among many others, of course. Okay. Let's look quickly on the agenda. I would

like to give a very brief intro to libp2p, even though I would guess most people are already familiar here. Then talk a little bit about the problems that we're actually facing in libp2p, which is firewalls and NATs. We're facing many other problems as well,
but we will not focus on those here today. The holy solution or the biggest hack of the

internet called hole punching and then how we measured this and what are kind of the takeaways from this larger measurement campaign. Okay. Cool. Again, short show of hands, who

here has ever used libp2p? Okay. Cool. Anyone that used IPFS obviously also used libp2p.

So libp2p, peer-to-peer networking library, one specification then implemented in many different languages. It provides low level features like encryption, authentication,

and hole punching, which we're going to talk about today, and then higher level features like for example, DHT, a distributed hash table, or gossiping protocols that then build on those lower layers that we built. Okay. And then in general, I would say libp2p is all you need or should be all you need to build a peer-to-peer application. Okay. So

we're going to focus on hole punching. And why do we need hole punching in the first
place? Well, we want within the libp2p network, we want full connectivity among all nodes,

or at least we want to strive towards full connectivity. And in today's internet, the
biggest barrier to full connectivity is on the one side browsers, which we'll not talk about today, and then on the other side, NATs and firewalls. Yeah. So we somehow have to

overcome NATs and firewalls to then actually achieve full connectivity within a libp2p network. Cool. Yeah. Not going to dive deep into what are NATs and firewalls. NATs, network
address insulators, they go from a simplified, from a local public IP to a public IP. Right?

They do this mapping. And then firewalls, small disclaimer, I'm not advocating for all of you to turn off your firewalls. Please don't do that. But the firewalls have a very
important role. They protect you from outside traffic. Right? And in most cases, you actually don't want anyone to be able to connect to you. You actually don't want that full connectivity.
But in our case, we do want that. And then the one thing that we're going to work with
today during the talk is the NAT table. It's just a representation. Right? Every model

is wrong, but this is actually useful. You have a table of five tuples, source IP, source

port, destination IP, destination port, and then the transport protocol. And every router,
or in most cases, the NATs and firewalls are actually in the router. Every router keeps track of this table. And we need this later on to do the hole punching. Cool. So what

is the problem? By the way, in case you find these graphs fancy, none of them were created
by me. All of them were created by Dennis. So let's say A wants to connect to B. So A
sends a packet to B. That will add an entry on the very left to its routing table. Right?

To its NAT mapping table. And then the packet will make it through the Internet somehow.
Right? It's routed through the Internet. And then makes it to router B. And router B will
check its table and discard it. Because it has never seen any packet going from B to

A. So it will think that this packet is coming from an attacker. Right? Or from some malicious
source out there. So better to drop it. And that's a good feature of a firewall. Right?
So how can we overcome this in situations where we actually want those to be able to
connect to each other? So let's say we have some mechanism. We'll go into what that mechanism
is. But let's say we have some mechanism that will have A and B start a process at the same
time. Right? So let's say... Let's go. A and B both send a packet at the same time. That

packet traverses their routers. That adds entries to their tables. And those packets

somehow meet in the middle, go the other way, end up at the other person's router. And ta-da!

You have entries in each of these tables. And the routers will let the packets go through. Right? And this process is called hole punching. Where A punches a hole into its own router

so that the packet from B can make it through that hole into node A. Okay. Again, very much

simplified. There's a lot more details to this. But I think this is helpful here. Okay.

So now I talked about this magic process. Right? How can we achieve that A and B start

this process at the same time? How do we achieve that they both send the packet at the same time? In lib2p world, this is called DCUTR. Decutter by some direct connection upgrade

through relay. Don't remember the name. That's fine. In other protocols, like for example

WebRTC, you do this coordination over turn, for example. So how do we synchronize somehow?
Well, in lib2p, we first introduce a relay. So we have both A and B connect to the relay

node. And then we at least have a relayed way to communicate with each other. And what
we want to achieve now is a direct connection between the two nodes. So once we have the
relayed connection, B will send a connect message to A. And it will actually start a

timer. It will start measuring the round trip time. The connect will go through the relayed

connection. Again, we don't have a direct connection yet. It will arrive at A. And A
will send a connect back. So at this point, you can see that B knows
roughly the round trip time between B and A over the relay. What it will do now, it

will send a sync message. And it will wait half a round trip time. So half a round trip
time is exactly the time the sync message will need it to A. And after half a round
trip time, B will start the hole punching. And when A receives the sync, it will start

the hole punching. So ta-da, we have our magical mechanism that actually synchronizes the two
and have them do a hole punching at the same time.
So then the packets are sent out at the same time. They traverse somewhere in the internet.
They make it to the other side. And ta-da, they make it through the routers. And this way, we actually achieved a hole punch. Okay. Cool. So that's how we do hole punching

in Lip2P. And hole punching is not specific to Lip2P. We're definitely not the inventors

of, in my eyes, the biggest hack of the internet. This has probably been done before 2008, I'm
very sure. But the most official reference I've found is in RFC 5128. It's actually quite

amusing to see this fully documented. Implementations in Lip2P started before 2021.

But since 2021, between 2021 and 2022, we have been specifying most of the protocols
and then implementing both protocols in Go and Rust. At Fosdem last year, we actually

announced or not announced, but we talked about most of this and then started rolling this out on the IPFS network in roughly summer 2022. And, well, rolling this out is one step,

but the next step is how can we actually make sure this works properly, right? And for that,

Dennis organized the so-called hole punching month, where we tried to measure how well

is hole punching actually working across the many networks that are using IPFS.

All right. So how do you measure hole punching? Well, what we could do is just have one colleague

run their laptop, have me run my laptop, right? And then we do hole punching. But now there
is so much complexity in here. Like how does my laptop's network stack look like, right? How does my internal network look like? What is my router? What's the NAT behavior of my router? What's the firewall of my router? What is my ISP? How fast does my ISP route
to the other ISP? And then everything mirrored on the other side as well, right? So there's so much complexity that if we simply test it between two endpoints, we'll get one data point. But that data point is pretty much useless as the internet is so diverse out there. So we set out for something different. We had many ideas. This is the simplest solution, I would say. What we first do is we have a honeypot. So you can think of this as in the

concept in the security world. So we have a DHT server in the IPFS network that simply

tries to attract other nodes. And among those nodes are nodes behind firewalls and NATs,

right? Just standard IPFS users out there. Now these users announce themselves to the

network and somehow come across our honeypot, right? Given the cadamular DHT, they will at some point connect to it. And the honeypot will forward those connection information

of those nodes behind NATs and firewalls to a database. And then on the other side of
the database, we have a server which kind of exposes this data to our clients.

Now the clients are special hole-punching month clients. We built this in Go and Rust.

And we asked many people to deploy a small little binary, a small client, in their network and just have it running for long term. And what the clients do, those are not IPFS clients. So those are very spec down clients. What the clients do, they connect to the server,

get addresses of a bunch of other IPFS nodes out there that are behind firewalls and NATs.

They will try to connect to the relay node of those IPFS clients behind firewalls and
NATs. And then they will actually try to do a hole-punch between those. And what they

are then going to do is report the result back to the server. And this way, we pretty

much tested hole-punching across the entire globe.
So this graph shows in which countries we actually had those hole-punching clients.
So not real IPFS clients, but spec down clients of people running in their home networks.

And these are the remote clients, so the standard IPFS clients running behind NATs and firewalls.

And so we basically punch holes in the direction from the puncher clients down to the remote
peers. Yeah, a couple more things. So it roughly went for a month. We have above 6 million

hole-punches. We have roughly 150 clients. So those clients that run into people's network

where we asked, hey, can you please deploy this client in your network for a longer period of time? And then we're roughly talking about 50k peers that we punched, too.

All right, so overall success rate, to summarize in one number, is roughly 70%. So 70% success

rate to establish a direct connection between two peers behind firewalls and NATs on the
IPFS network. But there is a lot more to this, and I want to draw attention to a couple of

the details. So probably the most interesting one is success rate. So what you see here on the left is

the number of networks, and then towards the right, the success rate. And what you can

see is, well, we have some networks on the very left where it's simply a success rate to zero. This could, for example, be symmetric NATs. I can go into detail what that is. We
simply can't hole-punch those at the moment. But what you can also see is that we have
a big chunk of networks where actually success rate is very high. You see the big spike.
So that's very promising to continue doing hole-punching. Then how does this relate to TCP and QUIC? There is a lot more detail to this, but surprisingly

we haven't seen a big distinction between the two. We would have expected QUIC, given as UDP, and given that thus the NAT behavior is very different, to outperform TCP by a
big chunk, but we haven't seen that. And then we seem to have some problems on IPv6, but

we haven't really dug deep into why we have such a low success rate on IPv6 across TCP

and QUIC. All right. So this is a very, very misleading chart, I would say. So you would think that,

well, 80% of the successes, hole-punch successes, were QUIC and 18.9% were TCP. That is correct.

But if two peers try to connect to each other, we will erase different transports. So we'll

connect over TCP and QUIC at the same time. And, well, maybe the TCP connection succeeded,

but we actually don't know, because QUIC is so much faster at the handshake, it only needs
one round trip for the handshake, that we simply... QUIC is just the fastest, and at

some point, once we have one connection established, we just don't bother about the TCP connection
anymore. So yes, QUIC is widely outperforming here at TCP, but this is due to the fact that

QUIC is simply faster at reporting a successful result of a hole-punch. Maybe the TCP connections would have succeeded, but we don't know at this point. Cool. Another complexity is the virtual private networks. The problem here is that, in general,

you would think that you're, as your laptop, you're very close to your router, to your home router, right? You would think of, like, at most one millisecond or something. But in the case of virtual VPNs, your exit point is very far away. So that screws with a lot

of the measurements that we do. You remember earlier this magic mechanism that I said, where, like, yeah, we try to time the two endpoints to hole-punch at the same time,

right? This is very difficult on VPNs, because the part that you want to hole-punch is not
right next to you, but a lot further away. And in case that's very much further away,
the hole-punch will not succeed. So that's why we have significantly less success rate on virtual private networks. Another takeaway is what we do in hole-punching

is we don't try once, but we try multiple times. So in total, we try three times to
hole-punch to a remote peer. And what the measurements pretty much show is, in case it didn't succeed on the first try, like, there's no reason to try again. But I'll add a little caveat in a little bit. So that's definitely a learning, and we can skip the

other two. And then lastly, what I would have expected is that the round-trip time to the relay really matters. So, for example, if two nodes within
Europe want to hole-punch, I would have expected that the round-trip time in case the relay is in the US, that this really matters. But we haven't really found a correlation between the round-trip time to the relay and the round-trip time. Whether the round-trip time to the relay
really matters in terms of success rate on a hole-punching. That was quite a surprise. I would have expected, given that the two European nodes are right next to each other,
that this is quite a problem with the US relay, because any timing problems that can happen

on such a long path really screw up our timing between the two European nodes.
All right. So next steps, a couple of learnings. Well, we need to reconsider the retries. Then

on QUIC, we can actually still do the retries. What we're currently doing is one tries to
establish a connection, and the other one just sends garbage. In case one of the two is a symmetric NAT, we should try doing it the other way around. So one sending the garbage, and one trying to establish the connection. So we'll probably have higher success rate there. And then we can probably do some optimizations around RTT measurements, better RTT measurements,
not to the remote peer, but actually as well to our gateway. So this way, we could, for example, discover that we are in a VPN, where our exit node, well, the VPN exit node, is

very far away. So the thing that we need to hole-punch is further away than the standard router. All right. The entire data analysis, again, done by Dennis. I would assume that this is

available in case people are interested in the data. I don't think it's available publicly,
but I would guess reach out in case you're interested in this large chunk of data. And

yeah, we still have to root cause a lot of things on this.
Cool. We published a paper on decentralized hole-punching. So how does hole-punching work
in LipidP? Main author here is Martin. And then since then, Dennis wrote up a very extensive

report on all of the measurement data. So if you want to check out that part, I think,
over the GitHub repository is the best way to find it, right? And yeah, so just go to protocol slash network measurements, and you find a bunch of really good, in my eyes, requests for measurement reports there. And one of them is the hole-punching
measurement report going into every detail of the data. All right. That's all from my side. Thank you.

Any questions on hole-punching?

So Max, so is the is the timing on UDP packet response traversal so fine-grained that the sync packet is necessary?

You know what I mean? Like you said, you had to send the sync because you're trying to get the endpoints to time their hole-punch.

What is the typical timeout on a NAT firewall where it will allow an inbound packet, you know, as a response rather than reject it?

Like what is that timing? Is the sync packet actually necessary? Yeah. So the sync mechanism we mostly build to hole-punch for TCP. If we have the sync anyways, might as well use it for UDP as well.

But probably the WebRTC way would be the better one. If we just would be doing UDP where we just, yeah, just shoot a bunch of UDP packets out.

And this way punch holes into our own NATs. Right. Because in your measurements, did you, I don't know how you would track it, but did you track the sync packet failing to get to the other side?

And therefore the other side didn't try to hole-punch back? And that is like a source of false negatives?

So the sync packet would never fail in that sense, given that we have retransmits on our transport layer.

But that makes it even worse, right? Because retransmits screw up your entire measurement. We have not gone into that, no.

Sure. And you said you haven't done any root causing on the IPv6 stuff? No, we haven't. Yeah, I would guess it's a silly mistake on our end. It might even just be in our puncher clients. We haven't. I just wanted to disclose it here.

Great talk though, by the way. Thanks. Anyone else? Any nitty gritty details about NATs?

One question regarding the relays. You said you would have expected that relays in the US make a difference.

But really the packet only needs to make it out of my router and to the relay before the other one comes back.

So I guess if the relay is very far apart, that time to the router is so much shorter than to the relay,

that probably by the time you're telling the other party to start, your packet already made it out of the router, even if it already made it to the other side.

It might have already made it out of your router, but it might have also already made it into the other person's router,
even though they don't yet know whether we're doing hole punching or not. Does that make sense?
Right. Yeah. So it's like, yes, like, can I show this somewhere? So we do the relay, right? We do the connect, then we do the sync, right?

We have the sync, and let's say the relay is so far away that actually this sync, the packet from B actually overtakes the sync and makes it to A's router first.

And then A's router is like, I don't have an entry for this. I'll drop the packet.
So, yeah, I would still expect this to be problematic, but we haven't found it in the data.
It doesn't mean, what do you say, like, missing proof of existence doesn't mean it doesn't exist.

You had another question? Yeah, it was in the same direction. I was thinking that it would be beneficial for the VPNs to purposely choose a relay in the US.

Mm-hmm. Ah. Why? To be far away enough. Yeah, to increase the delay after your exit point.

Okay, yeah. So you're saying if your VPN exits in Europe, take a relay in the US to slow it down?
So just to repeat, like, the idea is to pick up a far relay in case you're behind a VPN.

So you make the first attempt, you see it fails. Usually you would say, okay, another attempt wouldn't help, but then purposely pick a relay with higher latency further away to make up for it.

Yeah. Or wait a little bit after your sync. Yeah, probably easier. I don't know. At this point, I think we should do, like, try mostly, like, really buckle down on UDP and just send a constant stream of UDP packets both directions and hope that one of them makes it.

Yeah. Thanks. Cool.

I have a quick question. So after this great study that we did and all of the stuff that you've built together with Dennis and everyone, like, what is the, how did the results change what you would do with not hole punching in libb to be?

What are, did you change course of action? Do you have any extra items that you now want to fix for the next release?

Like, yeah. Okay. So one thing that was super helpful. So in general, go lipid B was very much ahead on hole punching. And what we could actually do on the rest side is, well, we basically had free hole punching partners through this architecture, right?

Because we could test against all of the IPFS nodes that are behind firewalls and that's due to this honeypot architecture.

And this led to finding many things on the rest lipid B side where we could actually do optimizations like we were not choosing our alternate servers correctly.

We were not. Yeah, I have to look them up, but we did a bunch of things. So that was definitely very fruitful. Then I would guess that there's retry mechanism here on UDP hole punching will be fruitful, but we haven't actually investigated this one.

And then I think just discovering the fact that we have difficulties on IPV six will be fruitful, but again, we haven't, do we, do we know yet?

I don't know. I also don't want to give it too much attention. Maybe it's just a silly bug in our implementation. Yeah. Cool. Okay. Excellent. Yeah. So, um, what protocols are keeping lib P2P firmly in the supporting TCP?

Ah, some ISPs drop UDP just by default. Yeah. There's nothing good coming in through UDP. That's interesting. In the past, right? Obviously. Yeah, I get that. I'm just saying, but right now, like, is it really just TLS HTTP? Like, I mean, are these. So like literally still ISPs drop UDP packets, so we can't run quick in those networks.

How big is that? So anyway, sorry, I don't want to derail this. I think it's somewhere within the 5% ballpark, but this could be completely off. Like, it's definitely something that is still relevant, for example, at the ITF and still considered.
No, I'm not saying it's not relevant. Just wondering how hard we can push the web three part of this. Yeah. Okay. Anyway, cool. Thank you. Thanks for having me.

