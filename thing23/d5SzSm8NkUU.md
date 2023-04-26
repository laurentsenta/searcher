
# Fetch Content Like A Border Collie: Introducing Lassie - Hannah Howard

<https://youtube.com/watch?v=d5SzSm8NkUU>

![image for Fetch Content Like A Border Collie: Introducing Lassie - Hannah Howard](/thing23/d5SzSm8NkUU.jpg)

## Content

Welcome back. I hope you had a great lunch. So in this afternoon, the first afternoon

session we're going to hear about a couple of things. First we're going to hear talks about some multi-protocol clients. There's going to be a talk from me going into LASI

and then we're going to hear from Giroppo talking about RAPID. And then we're going
to have a panel to talk about the Move the Bytes working group, how it went, what we
can learn from it, and what we should be doing going forward. I promise this talk that you're
about to hear will be the last time that I will be up here talking at you, so that's good if you're tired of hearing from me. But so I want to talk to you for this first

talk about LASI, which is the new IPFS implementation that we announced this morning at the keynote.

So this talk, if you came to the keynote, you probably saw the awesome, like, it's great,

polished, these are the wonderful things it does version. So that was that. This version
is going to be a little bit more about how the sausage is made. So we're going to go into some architectural, technical architecture, how it works, does what it does, and some

of the compromises we made, some of the messy parts, some of the awesome parts. Cool.

Before I do that, though, I want to talk about something, which is what I call the coordination problem, subtitled protocols are easy, using them is hard. And to illustrate this, I want

to, I'm hoping there's enough Gen Xers in the room to remember this bit of 80s, 90s

anti-drug propaganda. So this is BitSwap, the protocol, very straightforward. This is

Go BitSwap, the library. Essentially, when you go from the protocol to an actual implementation,

it gets messy. But there's a good reason for that. And the reason is that BitSwap is not

just like an interface to the BitSwap protocol, it's actually an entire retrieval coordination

layer. When you make a request to BitSwap, you say, give me this SID. You figure out
how to get it, you figure out how to distribute requests between peers, you figure out how

to break up requests. It's so complicated that for a long time, I had trouble convincing
anyone, convincing folks at Protocol Labs, that you would ever want to build a retrieval
coordination layer outside of Go BitSwap because they just assumed it was so complicatedly packed in that you couldn't extract it. So this is actually the core problem we're thinking

about addressing in LASI, is how to coordinate retrievals among protocols that already exist.

Interestingly, this has been a little while coming. This is a long architectural document

I wrote up in 2021, thinking, oh, we'll get to build this multi-protocol thing any day

now. As it turned out, it took a little bit of time. But we did have some antecedents.

Bill Scott built this awesome little prototype Web3 retrieval client that essentially worked

with the network indexer to transfer data over BitSwap and GraphSync in a very simplified

way. And a lot of the ideas there are actually fed into LASI.
And then I want to talk about the actual generation, how LASI came to be. We started with a particular

problem which might feel a little unfamiliar at IPFS, which is how do we retrieve data
from Filecoin? And we wanted to... There was a product that we had built to essentially

try to bridge the gap between Filecoin and IPFS because Filecoin at the time was only

speaking GraphSync and IPFS still only speaks BitSwap. So we built this server called autoretrieve

which essentially would listen on BitSwap and then take requests from BitSwap and translate
them into... Attempt to find the storage providers that had that data and then make a GraphSync

request. It was a terrible solution to a problem that we had created ourselves when we built
these two different networks. And my team, the Bedrock team, actually got

pretty heavily involved in that and we kept sort of iterating on the Filecoin retrieval
part of it. In particular, we wanted to figure out how can you find data on Filecoin quickly
and then retrieve it quickly. These are problems that were not initially addressed in the original

sort of shipping product with Filecoin, which is Lotus. We didn't build a content discovery

mechanism into it, but in the intervening time, the network indexer had come into being
and it allows us to find storage providers that have content on Filecoin.
So we were starting to build coordination around retrieving data from Filecoin through

GraphSync. And it was getting complex enough that we thought we should make this a separate project. So we built Lassie, which is a Filecoin retrieval client. This was the original mission

of Lassie, was to make Filecoin retrieval really easy. We basically... We wanted to

build a simple... People would always say, how do I get my data back from Filecoin? And we wanted to build a very simple tool you could simply hand to people and say, here's how you get it back. And then I made the mistake of getting on a plane to Switzerland to meet with the Saturn team. And when I got there, they told me that

actually this library we built was going to be the backing library for all of Saturn and
it was going to be an IPFS and Filecoin client. Never show up a day late to a conference at
PL. And that's how Lassie came to be. And what we ended up doing, the first big task

was to add BitSwap into Lassie. And through doing that, we have evolved it significantly

to be a sort of architecture for arbitrary protocol retrieval. Arbitrary retrieval over

multiple protocols, including finding your content.

So to get into how it actually works, I want to first look at the larger sort of ecosystem
it exists in. The primary ecosystem Lassie exists in for the time being is Project Raya,

which is this decentralized gateway architecture. And so what we built is on each Saturn node,

each Saturn node runs some Saturn software and then it runs an instance of Lassie. And Lassie exposes an HTTP interface within the Saturn node. We actually built it that way because Saturn is all written in JavaScript and we needed an easy way to talk to a Go program. So Saturn nodes, when they don't have things in their Nginx cache, they make an HTTP request to Lassie and then Lassie basically talks

to the network indexer as a mechanism for finding things and then it talks over either
BitSwap or GraphSync to people who have the data that you're looking for. But what's going
on inside of the little Lassie circle right there? Okay. Get ready for a lot of architecture.

I'm going to spend a second here and hopefully you can read this, and I did my absolute best

to keep this as small as possible, but it still got big and complicated.

So this is essentially the high level view of Lassie. At the top of Lassie, you have

essentially two interfaces for interacting with it. This is assuming you're interacting with the Lassie binary and not interacting directly with the library. There's a CLI interface
and there's an HTTP server. Both of those, I said in the talk this morning

that Lassie does not actually have a block store. And that's mostly true with a caveat.

We have a very temporary block store when you make a request. We have essentially a

car file streaming library that exposes a car version 2 block store to Lassie, but then
streams a car v1 back to the client as you get data.

So the CLI and the HTTP server essentially create a new instance of this car stream,
and they give it to the Lassie library. The Lassie library has sort of a top level retriever
interface that speaks to a client to the network indexer. And essentially using that client,

it makes a network request to the network indexer to get back to essentially go from
a SID to a list of people who might have the content you're looking for.

And what we get back from the indexer and from the client is a candidate stream. Essentially
this is an asynchronous stream. We don't get a list of candidates, we get a stream of candidates that may be coming in over time. We get a candidate stream and we send that stream into

essentially a protocol splitter that divides them up into different protocols that we're
going to want to retrieve with. And then we have clients for each protocol that do the
retrieval. The coordination between the protocols right now is actually pretty simple. We race the two different protocols. And because of the
way we built this car streaming library, as each protocol gets data, they can put it into the car stream and it properly returns the data to the user.
So that's the basic architecture. Now I want to talk about a couple of features you might see here. There's no actual content routing library in Lassie. We work entirely through

the network indexer. And this is an intentional choice. If anybody was at IPFS camp, you might

have remembered Dig's talk about BitSwap and how he was complaining that one of the biggest
mistakes we make in a lot of our software architectures is mixing content discovery
and data transfer. And so in Lassie, we separate these out by actually moving the content routing

to a different service. So we have no option to bring content routing into Lassie.

And it also, there's a couple of advantages to this. First of all, it pushes our network indexer team to keep making content routing there a super awesome solution that has super fast response times. It limits the amount of, like, one of the most annoying things

I find in, like, looking through BitSwap code is that it's dealing with, like, multiple
forms of content discovery instead of a single stream of where things are coming from. So

you have to decide, oh, well, I'm going to get this BitSwap local content discovery, but I'm also going to talk to the DHT and I'm going to have to make decisions about it. It just gets complicated. And so what we want to do is just have a stream of candidates. And that's what going through the indexer allows us to do.
Now that does make things a little complicated because in order for the indexer to be performant,
usually because the indexer also talks to things like the DHT and some fallback BitSwap
peers that don't announce in the DHT, in order for that to be performant, we want the indexer
to send back results as soon as it gets them and continue to send back results as it finds

more through mechanisms that may take more time. So that does mean that we have to deal with this asynchronous stream. I spent a lot of time thinking about the best way to do it. I used to be a JS programmer and then a Java
programmer at one point, and I was really into this thing called Rx that's like a ReactiveX,

reactive programming. It's super cool. So I went on a whole adventure on, like, maybe we should be using Rx Go, maybe we should be getting deep into it. And then I realized that actually there's a built-in streaming mechanism in Go, and I finally found a use for Go channels that makes sense to me. So that was awesome. And once I realized that,

I was like, oh, Go channels are just streams. They're not like a magic asynchronous mailbox
like Erlang. So that's how we deal. That's how we pass around candidates. We pass around

channels of candidates coming in and deal with it.
Cool. So there's a lot of cool things you can do when you don't have the logic baked

into your actual library. First of all, you can completely override it and use a manual
peer list. And we actually allow you to do this in the CLI. If you know who you want to retrieve it from, you can specify it, and then it will just retrieve from that person.
You can do cool stuff like block and filter out peers. This turns out to be really useful when you're in a prototype network that's rapidly sending requests around and occasionally
create AWS bills for certain other groups. You can also do stuff like force it to use

certain protocols. Because you get results back from the indexer, you can essentially

say, oh, I'm going to drop all of the candidates that are on this protocol and only use another
protocol. So that's a pretty cool thing you can do. Another thing that may not be evident from the diagram is that once you exit the content

routing interface, essentially in the retriever architecture, every single module has kind

of the same interface. The idea is we like to say retrievers all the way down, meaning
that you've got a bunch of retrievers and each retriever tends to have child retrievers if it needs them. This might seem a little bit architecture astronaut-y, but it does

enable you to do some really cool stuff that we've already started using. For example, you can A-B test different retrievers in production, which is something we need

to do, especially when it comes to performance. You might have seen there was one of the retrievers

called the protocol splitter, and the simple version of it is that it just puts the graphs sync candidates to the graphs sync client and the bitswap candidates to the bitswap client, but you can also do other things like, oh, I'm going to actually group a bunch of bitswap candidates and not send any graphs sync candidates at first until I send the bitswap candidates and get bitswap going first, right, if you want to prioritize a protocol.
So there's some cool stuff you can do there. You also can separate coordination across

protocols from coordination within a protocol, which turns out to be pretty useful.
Cool. So how does bitswap work? Apparently this is what a Kluge machine is. I didn't

know this until I image searched Kluge. So how does bitswap work? We kind of hacked it

with GoBitswap for the time being. What we do is we essentially pretend to be the DHT

for bitswap, and we remove its, we essentially say bitswap currently, when it can't find

peers in its local set of, local swarm, it goes out to the DHT. Usually there's a delay
on it. We remove the delay, put it down to zero, and then we provide it with the peers
from the DHT by overriding the interface that it essentially takes in to query the DHT.
We did this because we needed to get it up and running. We don't want this to be the

permanent state. It could look a lot of ways in terms of the evolution. We may end up doing

some more bitswap implementation inside of Lassie, or we're also working with the Kubo
team to improve upon the sort of bitswap architecture so you can get a lower level interface.

One question that's been asked is, is bitswap permanently broken? Is it a terrible protocol

that we need to move away from? I would ask, is it broken or do we just need to fix it?

Part of that is that just bitswap tries to do everything, because again, it was built with the intent, as it evolved over time, it became the Uber library for retrieving

data on IPFS. The interfaces to bitswap are super high level. You don't get to tell it

what peers you want to retrieve from. You don't get to tell it how it's going to find
peers. What if we built a version of bitswap that had no more broadcasting of wants to
like all your friendly other peers? That's the source of a ton of network traffic. I

think there's a talk somewhere else today from ProBlab about the cost of doing broadcasts on our network. What if we got rid of the concept of content discovery through bitswap?

This was a key optimization we put in, but only because there was nothing else. Maybe that doesn't make sense. What if we just asked for blocks and got them back? If that were

all bitswap was, I think it could work. Maybe it's not the best block request protocol,

but it's not the worst. Let me talk... I'm going to do a... This

track is kind of jumping around to different parts of LASI. I want to talk a little bit
about the LASI HTTP interface and how it relates to existing HTTP implementations. LASI's HTTP

server exposes essentially a single path URL interface. It's get slash IPFS slash SID,

and then you can optionally put a path of directory and you specify it's in format car.

Now, if this looks familiar, it's because it's the gateway interface for the most part.

There's a couple of changes, though. This is what enables us... Basically, in LASI,
we did not want to do the work of removing the... Of essentially adding trust in by sending
you back flat files, because we really want people to verify their own data.

We only support car files, but we do some additional things. One of the missing components

of the so-called trustless gateway interface specification in the IPFS gateway as it stands

is that when you fetch a car file, if you fetch it at a path, it only sends you the

blocks in the car file that make up the result at the end of the path. And the downside of that is that you essentially lose the ability to verify your data.
So LASI, when you ask for a SID at a subdirectory, we're going to send you the SID and the intermediate

blocks to get you to the end of the path, and then we're going to send you the data so you can verify it. We also do a couple of...

other things. We support some pretty limited but also super useful scopes in what you can request. Right now, by default, all car requests, I believe, attempt to send you an entire DAG from the SID you request. It just traverses until it gets to the end. And we do support that, but we also support some other scopes. And part of this is... Part of the scopes we support is these are intended to support a trustful HTTP gateway being able to make

the smart requests it needs in order to serve up flat files to another client through LASI. If you remember the architecture or diagram that Will put up for REA, like the gateway, the regular IPFS gateway is just going to be making requests out to Saturn, and it's going to get back car files, but then it's going to do the translation. So we need to be able to make intelligent requests that allow us to get the data we need for translation.

So one of the... The second scope we support is the so-called file scope, name and work in progress. The file scope essentially returns back data, and if it's UnixFS data, it returns back exactly what you would expect to... Like, it essentially returns back the entire UnixFS entity that you are going to need... That you would expect to get if you made a flat file request.

So that means if you make a request for a file, then it's going to return back all of the blocks that go into that file. If you made a request for the directory, it's going to return back all the blocks that are needed to essentially LS the directory. Right? It's not going to return back all the subdirectories and files underneath it, because that could get huge, but you can... But the file scope will allow you to essentially LS.

And then the one other scope we have is the block scope, which is just essentially return back the single block at the end of the path. We also may have support for byte ranges in LASI. I was in the middle of working on that on the plane. We actually support it in Saturn, but for sort of architectural reasons, we intentionally kept it out of LASI for that purpose.

And the CLI has all these same commands. You can kind of use all of them at the CLI. So, fun fact, this HTTP protocol is the first protocol that we're going to support for backends providing data to us.

So, we're actually going to be working with a couple of key stakeholders or people who hold a lot of data, namely Filecoin storage providers running Boost and probably Web3.storage to add HTTP transfer to LASI. And the interface will just be the same interface that LASI exposes.

Now, when we do that, we are still going to verify the data in between, so we're not going to just be a super simple proxy. And we think that's important because we want to make sure that we're not returning stuff to the user that is not the correct stuff.

So, yeah. That's the HTTP backend. Metrics. I'm going to make a risky move and step out of my presentation for two seconds. One of the things that we have built LASI from the ground up, because this is something that we've encountered with some of our other projects where we built them, where Kubo's evolution, it began as probably one student project way back in the day.

And then it was iterated on by people who were just trying to figure stuff out. And one thing that we've gotten really clear on is to get to performance in particular, you have to measure everything. And so we have a lot of that built into LASI.

I'm going to just briefly show you our... This is our mega-grafana. This is actually tracking Raya stuff. You can see the entire funnel of requests. Where are we losing people? We actually lose some people because we can't find their content. We lose people because we filter out their content.

A bunch of other stuff in here. And then we also have super... We're very intent on getting Filecoin SPs to be a big part of the LASI retrieval story. So we've got a ton of metrics on how LASI's doing with different Filecoin search providers, why they're failing. We've got stuff about time to first byte, time to bandwidth and data transfer, how much data we're moving, success from various stages.

Anyway, it doesn't really matter. But the point is, we're trying to track everything super closely so that we can iterate and improve. And this is actually... We've shown this over the course of the Raya project, that by having metrics, which is essentially trying experiments, seeing how they affect the metrics, and then iterating from there, you can make a lot of progress in performance.

And I think that's going to be our strategy probably for a long time. Let me talk about what's next. So probably we have in LASI... I've said before that LASI is largely stateless. But when it's up as a server, a long-running process, we want to... There's probably some information that we want to track and make some good decisions about.

But essentially, we want to choose the right peers to get data from. We want to choose the ones that are fastest, the ones that tend to respond successfully. We want to choose the ones that may be closest to us.

So right now, we are storing some of that information, but we want to keep developing that information. We want to feed it into the requests we make to essentially try to coordinate between multiple protocols the absolute fastest way to get data.

So we're going to be iterating on that. We want to make good decisions. And this is a little bit like if you've dug into the BitSwap sessions code, it does a lot of this. There's a lot of really intelligent stuff there.

It's just that it's tied only to BitSwap. And so we are sort of making the protocol-neutral version of that.

Now, we're going to use some state in memory. We're not going to write anything to disk. And we may not do all of this ourselves. We may also, again, want to delegate out to another party who can sort of perform this at scale more effectively.

So we'll see where that goes. That's it. That's the talk. I don't know if I have time for Q&A. Yes? What? Q. Q. Go ahead.

Please. What is car scope all? So car scope all is – it means essentially at the end of your path in your request, you're going to try to fetch the entire DAG that is beneath the SID at the end of your path until you get to the point where there are no more leaves, no matter how deep it is.

So if it's a directory, you're going to fetch the directory listing and then the subdirectories and the subfiles and all the way to the very end, whether or not you're in Unix.

Is that the equivalent of not specifying car scope? Yes, it is the equivalent. Yeah. We're trying to be as least breaking as possible. So we are going – the default is currently effectively all. So, yeah.

Okay. So the second question, hopefully real quick. Are the golden retrievers in LASI, are they pluggable? Can I write my own one and put it in LASI?

So, I mean, yes, but Go's actual code module linking is not super great. We might need to just make an HTTP-based retriever.

Yeah. I mean, programmatically I can just – can I just give it another retriever interface and it will just use that as well?

Yes. Yeah. Currently it actually takes the list of retrievers in to the constructor.
Got it. Okay. Cool. Thank you. Yeah. All right. And I – one more question and then I apologize. I went over, so I want to make sure we don't go too late.

Hello. Hello. My question is how would a web client or a mobile client connect to LASI or how do you see an architecture working where those sorts of clients would use this?

Absolutely. I'm really glad you asked that question because I was like, oh, I didn't put any, like, really information about what's our web story.

And there's, like, so clearly if web – like, our web story is somewhat limited by the fact that we wrote it in Go.

And there was a lot of discussion, actually, when we were starting. So should we just try to do this in JS or maybe – mostly JS, like maybe Rust WebAssembler,

but that really doesn't actually work because of the way WebAssembly works. So the reason we stuck with Go is we wanted to build a reference implementation, essentially to show you how you do this coordination with the intent that if somebody wants to do it in JavaScript,

they'll have an entire architecture that they can draw from. So that's part one. Now, that's not really an answer. Yeah. But, I mean, it was pretty intentional because we think the hard problem is the architecture. Right?

We think the hard problem is how do you coordinate. Now, the – however, we do provide an HTTP server.

So the simple thing you could do is you can, as you write your website, on your server side, expose the LASI Go interface to access Filecoin content,

the LASI HTTP interface to access Filecoin content, and then in the JavaScript that you put on your web page –

and this may actually be, for the time being and possibly the foreseeable future, the best way to do this because it produces a light JavaScript client.

All you do is you write JavaScript code that runs probably in a service worker to intercept HTTP requests, which go to LASI,

and then as you get car data back, translate it back to the flat file. And that's what Saturn does.
Saturn actually already has that code in existence. So those are the two approaches. I know that's not a complete answer, but, again, we were pretty clear that we wanted to solve a problem,

which was how do you coordinate the retrieval among protocols, and the way we could do that in not a year was to do it in Go.

Thank you.

