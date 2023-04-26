
# Live CDN Incentives and its Future - laudiacay

<https://youtube.com/watch?v=yrrAjR03TsU>

![image for Live CDN Incentives and its Future - laudiacay](./Live CDN Incentives and its Future - laudiacay-yrrAjR03TsU.jpg)

## Content

Hi everyone, I'm Claudia, I'm the founder at Banyan, we bring data onto Filecoin and

on the side I do research for fun, this is a project from August 22 that I did on the
side after work, I think it should probably be shared with the community because I've not really seen any correctly done incentives for Saturn yet, we know incentivized retrievals

are problematic on both Filecoin and IPFS, everyone in this room knows about that, this
talk is going to be a state of how things are right now, and my vision for how we could
build decentralized retrievals of service, so to be clear this is not you pay a miner and retrieve your data, this is how do you do it at CDN speed completely trustlessly,
so this is going to be kind of vague, there's going to be a lot of now draw the rest of the owl, call block science, this needs to be modeled, or we need to implement this,
but yeah, here we go. Problem description. What is a CDN? People want their data quickly, I talk to potential Filecoin users a lot at my company, people really want speed, we all know this, data is all over the planet, we need to get it to people quickly, and durability under attempted censorship by dishonest nodes is
very important, also getting the data and keeping the data closer to the person who actually wants it is important, incentivizing this is hard. Okay, there's context. I'm going to be using Alice for the users at the edge, both uploaders and downloaders,
Charlie is the message passers in the middle, incentivizing them trustlessly, even though they may want to censor you is also important, figuring out how the market is going to incentivize
them to warehouse data adaptively to serve it repeatedly, so you know like a person in New York should probably keep a copy of the really popular YouTube video that came from Europe, and then Bobs are holding the data and they may be decentralized like IPFS.

So the places where I have little angels are people who have real incentives to get the right data to each other, the devils are people who may be censoring, and everyone is a rational greedy actor here, so they'll save money if they can, and they will try to get as much
money as they can out of the counterparty as well, but yeah, that's basically like who's censoring and who we need to protect against censorship. Okay, cool. So this is a problem different than anything we've said before, so Magic's response to my talk earlier was that's provably impossible, and I'm like, yeah. So most consensus systems require that, like they just assume that message passing is done,

and you know, some people are going to lie, but some people are honest. Well here you have the problem of you've got like a point-to-point interaction where it's just like two guys interacting and we need to reflect somehow in the global consensus
that this interaction happens. So you've got the Byzantine generals, you know nothing globally except what's broadcasted,
and you know there's no incentives at all. Bitcoin, we add the incentives, there's a last step incentivization, which is the gas
fee, so you have an incentive if you can get it to the miner for the miner to put it on chain and then from there we just assume that everyone's going to gossip and you know with
some light correctness assumptions you'll eventually get the right state. Cloudflare's and ISPs, it's a legal contract and then also some reputational stuff, you
know if Cloudflare starts censoring you and you're worried about that you might switch ISPs or you might switch CDN or ISP. Censorship, you know, for the majority case you're not out of luck, but for the very small
minority case there's really not much you can do about this. IPFS is volunteer based with a couple of like, you know, Cloudflare style SLAs, look at Pinata,

and then right now we've got Filecoin, SIAs, and RWEVs right now. SIA does some in-band incentives that I'm going to talk about on here. RWEV does this thing called PIIA, which is like the miners are just going to enforce each other, we promise, and they put an equation on that and yeah. And then Filecoin is like, you know, we call them up, we pay them, maybe in Filecoin, maybe
out of band, and then they send us the data. So none of these are really like incentivizing speed except for Cloudflare where it's properly
in the SLA. But yeah, that's trusted and can be censored. So yeah, like I said, there's tension. There's, you know, you need this like reflection in the public state, which is the money transfer
of I want to pay you for giving me the data. But also we have this private thing where if we have the tension of forwarding the packets to another party, that kind of destroys the value of the good in the first place.
So like, you know, sending it to a third party who's going to referee the transmission, that just doesn't work at CDN speed. So that's an on starter. So how do we prove this very like he said, she said situation of like, well, I say that

you didn't send me the data and now I don't have to pay. So that's problematic, because everyone needs to agree on how much money everyone got at the end of the process. Yeah, everyone's incentivized to lie. Seems like a mess. Yeah, so let's look at some prior work that we're going to ground things on.
This is not exactly, you know, totally relevant to what we're doing, but it's an important
prior step. So that's going to be bow and wire guard. So bow verified streaming. We have this long, big file and each packet that you give me, I can verify in band that that particular packet is correct. Very cool. Six to 7% overhead in terms of like the transmission. If you do the thing that Rüdiger did, where you kind of prune the lower layers of the Merkle tree, you can get much below 6 to 7%. So that's really, really nice. That means for things like video streaming, we can actually trust that the server is sending us the right packet as they send us the packet, even for quite big files. So this is good for big files and for video. And it's quite quick. And then wire guard and BTC lightning transport, which also uses the noise protocol strapped
on top of a state channel. So we have this PKI identified peer. We ensure that nobody tampers with the content along the way. So the Charlies can drop everything that we send, but they can't mess with it.
And the timer system of wire guard, where you renegotiate the handshake for your encryption
keys is a great place to insert the code for a state channel, which is going to be a big part of my proposed solution. So you can just kind of strap payments in band on the transport layer, which we're going to get to it. So those are two things that I'm going to talk about as we keep going.
So more prior work. Retrieval pinning is something that came out last year from CryptoNetLab. Let me just get into the description. Sorry for all the memes. They make it more fun for me to make my slides. So yeah. Let me look at... Okay. So retrieval pinning is this kind of complex protocol where you have this pool of referees who are more or less policing whether Bob is sending the file. So this is absolutely not CDN speed. It just makes sure that the file is available at all within a reasonable period of time. So what basically happens is we have Bob collateralizes, similar to a Filecoin deal.

So that's the start. Bob and Alice agree on a set of moderators for their interaction, which is more or less the SLA of the file will be retrievable. If Bob is not sending the file, Alice petitions the referees. They get the file from Bob or slash him. The way that you trust the referees is complicated and lots of overhead. They validate it and then they forward it to Alice. So this is out of band. This is not CDN speed. This will never work, you know, to get your TikTok videos at the speed of dopamine.
But it will make sure that you do get them. So you don't have a data hostage situation anymore, the way that you kind of do sometimes with Filecoin right now. Very cool protocol. I'm gonna use it as a primitive in the solution that I'm gonna come up with at the end. Okay. Yeah. So it's slow. I think it's a good thing that there's a middleman that's at minimum to RTT without all of the
validation that they have to do between the middlemen. So they actually take the file, get it from Bob, and then send it to each other. And then one of them sends it out to Alice. I think that they could do proofs. The referees could probably do some proofs that I'm not gonna get into to agree upon whether the file was retrieved and it doesn't need to be replicated so many times. But anyway, the fact that you have to send it twice is really not good at scale or at speed. Bob needs to collateralize. Filecoin miners are not onboarding data right now because they have such high collateral needs it makes it really, really hard to onboard. And the collateral that you need for this protocol, according to Kriptonite Lab simulations, are very, very high. There's some other things that weren't simulated. Alice has to pay middlemen. They have to run servers. There's a whole thing. Are these people gonna make enough money to make it make sense to run this protocol? There's other problems too about just practicality of this. But it's a great start. It is decentralized and it makes sure that retrievals happen without having an army of
bots that are just pinging the Filecoin miners. Is it up? Is it up? Is it up? Which is what a lot of the solutions that I'm seeing right now are looking like. So yeah, I like it. But it's not a CDN. So this gets a little better. This is where you have payment drips. This is like a class of solutions where I do a little work, you do a little payment. We do that over and over again. And I have a picture of Friedrich Hayek there. Because it's local knowledge about what just happened to me. It's I just got a packet. Okay, I'm gonna pay. And it's applied into the market. There's no outside shared authority. There's no friction. There's not a lot of communication needs. So let's get into that. Because that's been implemented in Skynet on SIA. So I independently came up with this. And then one of my angels for my company is actually the Skynet founder. And we called about it and went, OMG, we have the same thing. So it was cool. So yeah, it's inband incentives over a payment channel that is right next to the transport

layer. So I send you a packet. You send me a payment. We iterate this and iterate this and iterate this. You get paid. I forget how I was using you and me. But the server gets paid. The client gets their file. So there's not a lot of trust at any point. The most trust that I'm giving you is a couple cents. The most trust that you're giving me is a few packets. So it's not like we're doing a lot of work for each other without trust. There's no outside authority. Yeah, we pay the host directly. There's no need for extra RTTs to wait. Because you can just fire off that packet while you're still receiving other data. It can be in another thread. It's quite fast. So why isn't this great? The middlemen are not being paid for this. Unless you are doing a situation where there's a paid connection from the origin to a Charlie and then from a Charlie to the Alice. And that could work. But there are a lot more router hops usually that are not caching or keeping the file and you don't really want to establish that many peer-to-peer channels. So I don't know. That needs more work. Yeah, there's a lot of elliptic curve signatures because you're sending a message that is a
spend. Elliptic curve signatures, in case you don't know, are slow. Not network speed. Not good. It's not programmable. You can't give your money to someone else to have them download on your behalf. And it's not protocol layer. It has to happen in this like SIA binary. So it's not like it really only works for SIA. So it's a really good start, though. And I linked to two little spots in the massive Skynet code base where you can check out where
they're doing this. There's a lot more code surrounding this. But you can kind of get oriented. It's pretty self-explanatory. So now, done with prior work. These are things that I have done. This is like my little kind of fuzzy research from August. So you can improve Skynet by making the state channel better.
You set up the state channel. You post a commitment to, hey, there are some values that I can send. And when you present one of them, you get, I don't know, like 1% of the prize pool that's the full value of the state channel. So I post that commitment. So maybe it's like a keyed RNG output. Don't worry about what that is. Let's just say I post some math. If you solve the puzzle, each of the 1,000 solutions to the puzzle will give you one
penny. And we can validate that on chain. If the client acts, so when I, you know, act a transaction in TCP, I will add one of these

little bits of information that you can prove is a correct solution to the puzzle and get
back out your, you know, get a few pennies. So yeah, the server will just prove that to close out the channel. And then they can claim however many packets I sent them little receipts for.
Tagmo is already building state channels on FVVM, which is cool. And Bitcoin Lightning payment channels already integrate with noise, which is, you know, WireGuard, that's the transport layer that I'm suggesting that we should use later on.
Yeah, so you can also, yeah, you can ramp up over, you can build trust over time with
this, which is cool. We can start by having you pay me one packet, I pay you one penny. And we can, you know, as we keep on with the connection, we can have you give me five packets. I give you five cents. That will reduce overhead for UDP based protocols where you don't already have the ACK. For the TCP ACK, you can just kind of append this, you know, little checks on monopoly money to the end. And it comes for not free, but like a couple of bytes, which is not so bad.

That's just a thought. That's something that someone would have to model, not me. Weird new topologies of Skynet incentives. You could do an onion routing thing where you append, you know, a whole bunch of hashes that are claimable by intermediaries. I don't know how you would prove that each person only took one. But this would mean that you can pay people who are message passing, but not necessarily caching and originating the entire file without them having to establish a payment channel. I think this is something that requires someone else thinking about it, not me. But another interesting one is maybe when, like, Netflix initiates a connection with you, it comes from their server, and they also give you, like, a little ticket to say you're allowed to download this from someone else, and we'll pay for your bandwidth. The payment channel kind of allows that delegation. Obviously, like, checking to see who or what they're paying for, you know, who they're
paying is easy to enforce on chain. What they're paying for gets you back to that same, like, we can't validate what just happened point to point. So I could ask for bunnies.jpg, and you want to subsidize me getting bunnies.jpg, but actually
I get cats.jpg from the same person, and there's nothing you can do about that. So if you didn't want that to happen, that's, you know, a you problem. You could also do this with the permissioning to download content from a third party. So like the JWT for getting bunnies or cats.jpg could come with this, like, download coupons
for bandwidth. I don't know. Could be a nice workflow. This is all very, like, vague. The past two slides have been. Yeah. Obviously, that's also kind of gameable. So yeah. I don't know. Requires more work. Anyway. In-band incentives look like a generalized multi-armed bandit problem.
If you're not an econ person, this isn't targeted at you. And it's also iterated prisoner's dilemma, and the reason that that is is, like, the shape of this problem is Bob has a lot of clients that it could serve data to, fixed bandwidth, it wants to make as much money as possible. Switching clients is a bit of a warmup cost for many networking reasons and also for establishing
trust reasons, like, is this person just not going to pay me? And it can stop serving and betray the trust at any point. Alice kind of has the same problem, except she has, you know, on her end an incentive to actually get the data. So she actually cares. So that's kind of the thing that's, like, backstopping all the movement on this market. But what's really cool about this is it naturally incentivizes replication for data that is in high demand. Alice wants her data as quickly as possible. If I'm a server in New York and right now I'm seeing her go through my node trying to pull a video from Europe, I can say, okay, I'm going to download a copy of that and just
serve it to her for me and then she'll pay me more than she would pay Europe or pay me the same amount because she can have lower latencies. Yeah. So that's cool. There's a problem with this. Like Alice's are unlikely to start a cartel because as I mentioned on the last slide,
Alice's have this real demand for the data. Like I actually want my data. I want it for as cheap as possible, but I really do want to see, you know, like my friend's vacation pictures. And I don't really have any leverage over Alice because, you know, if Bob has a lot of data, he has a lot of opportunities to profit. So if I'm being silly on my end, he's just going to cut the connection and go work with someone who will actually pay him. If Bob is not decentralized enough, though, if there are only one or two copies of this
file on the entire network, there is no competition for prices and Bob will just kind of raise
the rent to infinity on Alice's data, refuse to serve to Alice, refuse to serve to anyone trying to make a replica. If you just take Skynet, like the in-band incentives that I talked about, and mix them with Filecoin storage incentives, this is still a problem. So I mean, Bob is, he's forced to keep it. He's forced to, or he has this like, you know, carrot, like market to serve it, but he doesn't have any stick saying you can't just hold onto it and say it costs $100 per packet.

But we can fix this by gluing everything together. I'm sorry in advance for this slide. So to fix this, you have storage incentives to make sure that they're keeping the data with collateral slashing. You have retrieve style, you have to make this retrievable. And that's especially important for rare files that are not frequently retrieved. So it's not like a huge profit for me to keep this hot. But you know, if you want me to keep the file hot, pay me to do so and pay me enough that it's worth it for me to collateralize. And then add the Skynet style thing to induce competition across, you know, all of the SPs

participating in the CDN. I mentioned Saturn here. Saturn should use this in a couple of years. And then that means that the file is kept at multiple collateralized locations. Some people are directly collateral slashing enforced responsible for serving it. And it's accessible quickly if people are willing to pay for it to be accessible quickly, because everyone is competing for client bandwidth payments. The market will pull new geo localized points of presence replications, you know, from the

ether from the incentives. And then back to that slide at the start, add bow. So this is, you know, add or just below the application layer for streaming applications. Or if you're sending large files, you should be validating it as you go. Perhaps I don't really know if application layer is necessarily right, because you need to like not pay them if they're sending you junk data. So I'm not really sure where in the stack that should go. Yeah. So I said application layer. But yeah, I'm not convinced. In band incentives to WireGuard. The WireGuard has every five minutes, they renegotiate the, you know, keys and the handshake. So that's a perfect place to stick a state channel because you're doing that expensive blockchain transaction eventually for the closeout for the and for the creation.

Yeah, actually. And you have to do a whole bunch of elliptic curve stuff. So that should be in the thread with the state channel creation.

PUB you just stick stuff on the end of the act and it's pretty cheap. It's like, you know, the establishment commitment is
Yeah, I mean, you could like standardize it and like ZK, I'd like just stick that in a ZK proof that all of these things are

ticks.

from the first slide and then you win and you're very happy and you have a CDN. So yeah, I wanted to cover upload very, very quickly. Yeah, so upload, like the carrot where you're trying to get speed is the same, like you just use the same protocol. They'll backstop to prevent the catastrophic case. You just have Bob post like a Filecoin deal or a Retrieve deal or some kind of like contractual SLA looking commitment to make sure that he's going to start publicly
proving that he got the deal or he's going to get slashed. So now he wants the deal and now we just do the transfer Skynet style protocol. It might be Alice might upload it for free.
She might subsidize the bandwidth, who knows. But yeah, you can incentivize the initial
storage contractual replications by just doing Filecoin replication deals. We all know how to do

that. Cool. These were some cool vague thoughts. What's next? I'd love to hear criticism in the

comments section. If like you have criticism of like the vague ideas here, love to hear it.

But concretely, if we wanted to move on these kinds of ideas, there is a lot lacking here.
This was a very vague talk. I did this in about a week in last August and then dropped these slides in one week. So yeah, two great starting points for work on this. If
anyone has like the budget to hire like a smart older undergrad intern or like maybe a grad student
in like distributed systems or mechanism design, the tit for tat incentive layer, like the Skynet
style incentives, modeling that with CAD-CAD strapped onto Nikola's Retrieve protocol, possibly model Filecoin as well. Think about realistic bandwidth costs, replication costs, cost of capital, different demand flows across different continents. This would be a pretty
involved model, but it would be very good to have and good to see what prices could start looking like under different assumptions. So that's thing one. Retrieve already has some modeling in it in
their like light paper, which is great and everyone in the room should read it. It's pretty cool. The other thing that would be cool is like a test implementation of strapping like the
hash-based Monopoly money straight channel onto, did I say straight channel? State channel,
onto an existing WireGuard or maybe NoiseQuick. I don't know. Martin was like,
NoiseQuick doesn't work that way. I'm only familiar with WireGuard, so I don't know. But seeing how fast you can get this thing going would be cool under different like block time and
blockchain congestion assumptions for the payments. Like how much can you do out of band? How much can you defer to later? WireGuard has this nice timer system that I talked about for the transport channel renegotiation and it already has like an extra thread for handling the expensive elliptic
curve ops. So I think that it's a really good code base to try adding this into. It's also already in the Linux kernel. It goes very fast. But yeah, like just seeing is this implementable?
How bad does it bog down the networking? Could it actually be used in prod? Yeah, it would be a cool

research internship project if anyone here is interested in pursuing this. In my opinion,
those are the next steps. Okay. That was my talk. That was less anxiety-inducing than I thought.

Anyway, if you have questions. Anyone? Am I scot-free? Oh, Alex.

For these state channel payments, I might not totally understand. Is there a mitigation to

somebody just being like, oh, give me, let me request a file at different offsets from like
a hundred different peers. And then never pay? Yeah. Yeah. I mean, that would be really, really expensive on their end. You can't really stop a start of connection attacks, depending on

who's like, so every time that you make a payment, you're basically like, put it, it's a little trust, you know, like we're, we're taking out a little loan and you're going to pay it back. We're taking out a little loan and you're going to pay it back. So at the start, when there's no
continued, like iterated prison, or when there's, you know, nothing yet, no trust yet, that's a

dangerous time. And at the end of the connection is also a very dangerous time because we could just be like, oh, like, I don't have to trust you to send five more packets. I'm just not going to
send the rest. But like, really the amount of like loans and these connections is like very, very small. Theoretically you could get it for free. Like if you just initiate a jillion connections with a bunch of different people, but yeah, that's like very expensive and it's not that weighty an attack on the service providers because it's going to be so cheap anyway. Like, it's like nearly free. So like, yes, you can do that. It would be super slow and it would be like

a mosquito bite level of bad for the network is basically, yeah. Yeah. So you'd probably already
have protection in place for something like that. Well, you don't even, I mean, you need to see what
the model would say, but I don't think there even needs to be protection for that because the, if you're doing it to the starter connection, the SPs would shut things down pretty quickly when they realized that you're not paying and it would be like minimal overhead. And then if you're doing

it at the end of the connection where someone just stops paying or someone just stopped sending packets, I mean, I was texting with an economist friend about this and we were both just like, yeah, people would just post on Reddit, like don't connect to this peer. You should blacklist them in your router because they just truncate the file six bytes or six packets before it's over. Like, that's like, just don't use them. And it would be pretty like, I don't know, like people just wouldn't do that because there'd be out of band reputational mechanisms for that. But you could also just initiate a new connection with someone else to get specifically the last three packets. If a service provider started trying to screw you out of things. Yeah. I had another question, but I forgot it. Am I safe? Am I done? You may have already covered this for, okay. So where would you like these solutions to live?

Like, I know we talked about, like when we were thinking about this, our referees were chain, like oracles. Do you, with the developments in FV, if VM, IPVM, like, like just like generally,

where do you think this solution fits into the architecture that we've been talking about for the past week here? Yeah. So Saturn, obviously. I think there needs to be work on the lib P2P team,

making this fit into the transport layer. Cause you can't just have one client tool and absolutely one applet, like a grand total of one application that uses the CDN thing. Cause this like really benefits from economics of scale. So switching out a transport to a paid transport would be really, really good, which is why I think wire guard is super prime. You could do it in TCP
too. I think that that would require a bit more modification and would be a little bit harder to
make compatible than wire guard just because of like the state of the internet and the state of the network. Magmos state channels. I haven't looked into them in detail, but I think that they would be pretty good. The FVM is yeah. Like helpful for this, like the programmability,

like could probably be like hard coded. And just everyone's using basically the same state channel

over and over again. But yeah, Bitcoin lightning does basically exactly this. So it shouldn't be
too hard. So basically, yes, the primitives you need are a little bit of protocol work,
make it work with Saturn. And I guess you'd have to deploy the retrieval pinning network if you

want that as a backstop, if you're worried about that, like preventing the data hostage situation.
And then yeah, state channels and that should do it in terms of engineering work. But I reiterate,
there should be some modeling work first. What are the properties that you're trying to get with wire guard or with a noise thing? I mean, so it has like encryption, which is cool, but that's not really relevant. What's

really relevant is they've got the code already written to renegotiate a something every five
minutes and then do very fast, slightly modified, like transport on the inside.

Do you just need like an in-band tag after every packet? Is that the thing you're doing on your transport? That's one option. You can append it to the TCP acts or you can just send extra UDP acts that are

like channel messages, like control messages or something. I'm familiar with the wire guard code
base and it just really struck me as a good place to put this, but that's super flexible and open to other suggestions. But yeah, the properties are just like renegotiate payment channel, strap it into the protocol, which is already like a weird protocol because using you know, freak TCP is not going to be a thing that your ISP is super into. I guess I'm wondering if you made a parallel like libpdp stream that was then sort of an
out of band. Yeah, that would... Side thing of tags getting sent where they're not necessarily guaranteed to be exactly interleaved in the same way because there's some potential revixing. Yeah. Is that problematic? Does it need to be really strictly against bytes or... So I don't see an immediate problem with that. I want to think about it more before I give a hard

answer. I think that having it... I don't know. I think that having the connections suffer together

when you're doing an incentive mechanism, if the connection suffering is probably kind of smart, because it's like, oh, I haven't gotten any payments, but I also haven't gotten any packets. I don't... Yeah, I don't think that the acts should be ordered if you're doing like a UDP
protocol, like obviously you should not put ordering where there is not ordering. So that shouldn't matter for like a UDP. It might matter for a TCP where you were trying to have

stricter guarantees. But again, I think that this protocol is going to be very fast. Everything's going to be very fuzzy in terms of when you're getting the acts. If you want to keep the amount of data that you pay for that you haven't received yet as small
as possible, which I assume you want to do, it would make sense to integrate this with the transport and basically give the peer a... only pay for the next congestion window of data,

or then to account for packet loss, maybe two congestion windows of data. So I agree. Maybe just do it as a quick answer. Yeah, I agree. I just, I think it's going to be really problematic for adoption if people are

like, oh, I didn't get my payment in time. I'm going to kill it really, really fast. Like, I think people should be reluctant to kill the connection for non-payment is probably the correct behavior. But again, we need modeling work to like properly know. Like, I think it should be very not strict. Like you should have to get pretty in debt before you can actually get the connection. And I think that's a good thing. I think it should be very not strict. Like you should have to get pretty in debt before, like once you've really established the connection, you've been like paying, okay, you should have to get pretty in debt before the service provider cuts you off. It was just my instinct about usability. I have absolutely nothing to back this up. So yeah. Anything else? Cool. Thank you guys for listening.

