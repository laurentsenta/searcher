
# Hello Helia - achingbrain

<https://youtube.com/watch?v=T_FlhkLSgH8>

![image for Hello Helia - achingbrain](./Hello Helia - achingbrain-T_FlhkLSgH8.jpg)

## Content

Hello Helio, this is a talk about another, it was a new IPFS implementation in JavaScript

but like obviously since there's about a thousand new IPFS implementations being presented
this week it's now another new IPFS implementation in JavaScript. My name is Alex Potts-Seedys,

I am on the IP Stewards team at Protocol Labs, I'm the maintainer of js-ipfs, js-libp2p

and a whole bunch of supporting libraries, so I'm sorry if I broke something.
So Helio, so what is it? It's a replacement for js-ipfs. We're trying to apply basically the last five years worth of learning

of what an IPFS could be. We want to make it smaller, more lightweight, extensible, more observable, and faster

and hopefully it's going to make you more productive. But why? Why though? Why would you replace js-ipfs?

Because it's obviously disruptive because lots of people have written applications based on it.
So there must be a clear benefit to the users, otherwise we're just causing pain and refactoring
for the sake of refactoring, which can be quite a nice feeling, it's never a good feeling for the users.

So what are the motivations for this? So from the start, js-ipfs was supposed to be a clone of Go IPFS at the time

and what is now Qubo, which means it had to copy the same API and implement the same features.

And there wasn't a whole lot of thought put into whether or not that was actually a good idea,
it was just like, no, it must be the same. Which has led to some weird stuff.
So like the Go IPFS, the Qubo API, Qubo was primarily designed, it seems to be a command line tool

and the APIs reflect that. So the HTTP API is generated from the same, like all the code is generated from the same source

and you end up with the same API, like you end up with what's an ergonomic CLI API translated into a web API,

which is a bit weird, which was then translated into a programmatic API, which is kind of the primary use case for JS

because we all use modules and we write applications on things we generally don't shell out to CLIs.

So not all the features make sense as well because it's very targeted at a server-side environment,
definitely not the browser. So there's a lot of stuff that just doesn't really play to the strengths of JS,
you end up with a lot of stuff bundled, which is generally quite a bad user experience for people who care about

the time it takes for a website to render, which is definitely not a concern for a CLI.

So it makes it big. It makes it really big. I was Googling pictures of, well, Mr. Big, it didn't work out,

but this Mr. Big, I mean, anyway, I guess you have to be quite old to remember this one.

I don't know if it's good or not. Anyway, so when things are big, it's bad for the users because it becomes complicated

to understand. It's bad for the maintainers because there's a lot of code to maintain.
The API had a lot of duplication because there's a lot of shortcuts to do certain commands and they all got replicated
into the API, which is not a good design tenet for an API. You don't want to repeat yourself.

And there's lots of wrapping of other modules' APIs, which just increase the amount of maintenance,

though it does shield you a little bit from breaking changes, so it's not all bad.

But it meant that because the bundles are so big, you were getting code included whether you wanted it or not.

And some progress was made towards splitting that out, particularly around sort of more esoteric hashing algorithms

and that kind of thing. But we kind of need to go a bit further.

This is a slight tangent, but bad connectivity is definitely a problem with JS IPFS.

When we had this all thing event last year, the number one thing people were complaining about

was the fact that it's impossible almost to dial server nodes from browsers.

The USP of JS IPFS and now Helio is that it runs in the browser. It's no use if you can't talk to the rest of the network.

And we heard this loud and clear and a whole bunch of work has been done on this.
It's definitely the number one problem, but it's actually completely unrelated to JS IPFS.
Some of you may have seen this diagram. This table is from the libp2p website.

It shows which transports are implemented where and by which implementations.

As you can see, the bottom section is kind of the web-friendly ones.
So you've got WebSockets, WebTransport, WebRTC Browser to Server, WebRC Star, WebRC Direct.

Loads of the implementations do TCP. That's great. Browsers can't talk TCP.

They mostly all do WebSockets, but the problem with WebSockets is that you need to configure a certificate.

And that just appears to be a massive stumbling block. No one can configure an SSL certificate.
This is a web track. We are all web people. Configuring SSL certificates is our bread and butter.

It's super easy, but it still manages to be this tiny stumbling block that trips everyone up.

So it's lovely that it's there, but in the real world, when you try to connect to nodes on the network, it's just not a thing.

It might as well not be there, if I'm honest. So then what does that mean? That means that these server-side implementations go primarily, but also Rust as well.

They don't really cater for browser nodes at all, which is why it's so hard to dial anything.

No new JS implementation is going to change that, unfortunately.

But that is changing. So there's this new transport called WebTransport, which is super exciting,

which lets us dial server nodes from the browser without a certificate.
There is a certificate, but the protocol allows you to use cell-sign certificates and then does a tiny bit of the noise handshake

to then do noise encryption on top of that. Oh, no, sorry, it doesn't do it on top of it. We validate that the remote peer is the peer we think we're talking to, and then it uses the built-in encryption in the transport itself.

Suddenly, that tiny, tiny stumbling block is no longer there. So we can now have browser nodes dial server nodes directly, which is going to be a complete game changer, I think,

for JS, IPFS, or IPFS in JS in the browser. I think this is amazing.

If you're running Chrome, because it doesn't work anywhere else, that is the slide downside.

But you can run it in web workers, and you can run it in service workers. So you don't need to worry about upsetting the UI thread or anything like that. It's fantastic. It's really good. Very excited.

The other new transport that's arriving is WebRTC. We really had WebRTC. Alex, I've seen it. It's on the repo.

It was on that diagram you just showed. Yes, I know, but this is the new one, the new, new one, the new WebRTC.

So the old one had a hard dependency on a signaling server.
So with WebRTC, the first thing that the two clients need to do is negotiate how they're going to connect to each other.

And this is called the SDP handshake. Now, the old version had a WebSocket server that you had to run.

We ran a few, but they always fell over, that kind of thing. You had to run it. Made this hard dependency on this other server to communicate with, to do the SDP handshake.
And then you can connect, which is great. The implementation wasn't perfect.
It did do full noise encryption on top of that. So you were double encrypting everything.
WebRTC also has its own stream muxing capability that we completely ignored.
And did stream muxing on top of the double encryption on a single data channel. That has all changed. The new version uses WebRTC's encryption, uses WebRTC's stream muxing.

It's great. It's really great. Unless you're in a web worker or a service worker where it doesn't work,

because WebRTC is only available on the main thread. So we've made significant progress.

But there are some caveats. You should, incidentally, stick around for the universal connectivity demo that happens right after.

I'm doing this talk and I'm doing the next talk as well. And then it happens after that one. It's going to be amazing. They're going to show connecting from all the different implementations to all the different implementations
and how they all work together. And basically how we now leverage WebRTC and WebTransport and all that kind of stuff to actually really, really get JS going in IPFS, in JS going in the browser.

It's going to be like, I'm super excited about the future for this. Enough libP2P. That's a lot of libP2P chat. OK. So what is Helia? Helia is a very simple API. We definitely want it to be as minimal as possible

so that people can build abstractions on top of it rather than it trying to do everything for everyone.

So I'm going to go through each one. But yeah, so you've basically got a block store where the blocks live. You've got a data store where the data lives. You've got some pinning and some garbage collection. And then libP2P for all the networking there. So what's a block store? It is, hopefully like it sounds, it's a store that you can put blocks into and you put them in with a CID

and then you can give it a CID later and then you get a block out. If the block is not there, it is wrapped with bitswap.
So bitswap will kick in and try and fetch from the network. You receive a promise of a block,
which will then resolve unless you abort it, obviously. And you have your block.

Other data transfer methods may be available. So at the moment, at the moment Helio just bundles bitswap.
But I would like to look into how we can not do that and just have it able for the user to put their own data transfer protocols into it as well.

So that we can then experiment with new fun things and not have to, again, like bundle code that isn't being used.

But for the moment, bitswap is what it has.

The data store. What's a data store? It's basically a data, like a key value database.
There are implementations for IDB, for level, even a file system. If you want to put the files in the database and the database in the files.
You can do it if you want. It's very simple. And it's used by internal components like the DHT and IPNs name resolution service.

What else? So pinning. Pinning is really important because you don't want to have, you know, unbounded storage.

So you need some way of saying, well, these blocks are important and these ones aren't. So you have to pin blocks, pin DAGs. And in the process of pinning it, you'll walk the DAG and make sure that all the blocks are present.

Which is obviously important. But garbage collection is a thing. So you do actually need to be able to delete those, the unpin blocks quickly.

And this has been a significant challenge in the past for both JS and for Go.
And then underneath all that is libp2p. Which I'm just going to skim over. It basically provides a networking layer. So peer discovery, data transports, etc.

That's it. That's all. That's all Helio is. Data store, pinning, garbage collection, libp2p. There's no file system in Helio. So maybe, I don't know, interplanetary file. Well, IPFS, as we've seen from the new website, is content addressing and transports.

So it's definitely an IPFS. But there's no file system bundled with Helio. All of that will be provided by other modules that you will then compose with the Helio node that you've just configured.

Enough talk, Alex. This is really boring. A demo.

So the first demo that we have is just putting and getting a block.
So the first thing that I do is I create a Helio node.
I create a block. So just text encoder. Turn the hello world string into a UNA array. Hash it. SHA256. Create a V1 CID. Put it in a block store. Get it from the block store. And then just print it out. Nice and easy. So if I run node demo 1. Amazing. It totally works.
God. Tough crowd, really. Crikey. Another demo. That's boring. So what else? So obviously the networking thing is super important. So here we're doing a little bit more in our creation. So we create a data store and a block store. So this is just using in memory data stores and block stores. We create a Helio node. So we pass the data store and the block store in. We actually want to pass the data store into libp2p. So it doesn't use its own. I mean, it could. That's fine, too. So what is it doing? So we create a libp2p node. We listen on a WebSocket. Socket. Configure the WebSocket transport. Noise. Yammox. Pass the data store in. We're going to create two of them. We're going to get one to dial the other. And then same, same. So we create a block. We put the block into one. And then we pull it from the other. And then we stop the node.
It's exactly the same.

Consistency is key. I should have asked. Can you see that? Is that big enough? I can make it bigger. It looked bigger on my screen, but then I'm closer to my screen. So that explains a lot. Okay. Great demos. Right. File systems. Because that's what we're here for. This is IPFS, after all.
So in the beginning, there was a UnixFS. And then there was a UnixFS 1.5. I mean, sort of. Has it landed in Cuba yet? I'm not sure. I haven't checked the issue for a while. I don't think it has. UnixFS v2? Is that a thing? Is that a thing? Don't ask. I don't want to talk about it. Anyway. So what happens after that? I mean, why would we couple ourselves to this? So let's not. Let's just not do it. Let's just try and support anything. And let people experiment. So there's no blessed file system in Helio.
Because I would like to see things like WinFS being treated as equally as UnixFS.
Because it's all down to the application. And that gives the users the choice to use the file system that works the best for them. All that matters is, as long as it talks blocks.
Because Helio presents a networked block store. So as long as you can put blocks and get blocks, go nuts.
So a quick demo. So here we have, very similar to before, I've moved the create node function into its own file.
So that we're just not distracted by the noise. And so here we have a UnixFS implementation using Helio. So we create the two nodes again. Get them to dial each other. Create a UNA array from a string. Create our file system. So you'll see, we have a Helio node. And we're passing it into UnixFS. And so we have a file system. And so we call add bytes. We add the bytes. We get a CID back. We're going to create a directory. So you'll notice here that we're not really, we're not importing an enormous tree.

We're just doing a single chunk of bytes. So what we're going to do is we're going to put it into a directory. So first of all, we need a directory to put it in. So we just add a directory. We get an empty directory CID. And then we can use the copy command to copy the file that we added into the directory.

And give it the name file.txt. And so then we're just going to print the CIDs out. And then that's it. Then we switch to the other node. We create another UnixFS. Pass node B in. And we just list the CID directory which has been put into node A.
And then stop the nodes.

So there you go. So we've created a CID. Created an empty directory. And then we have put the CID into the empty directory. And then on the other one we've listed it. Where we print out the name and the CID of the directory content. So you see file.txt and then the CID. That's it. That's UnixFS. And the important thing is of course it could be anything.

It could be any file system. Okay. Observability. We have logging. Like one of the things that happens a lot with IPFS implementations in general

is there's a lot of moving parts. It's very hard to work out what's going on. Logging is definitely your friend in this. It's very easy to enable environmental variables or local storage in the browser.

And you get this incredibly readable display. You can all make that out, right? You know exactly what's going on. There's a bug in there somewhere. No, it's awful. I mean, not a great user experience. What I'm trying to do with Helio is introduce this idea of progress events for everything. So we have progress events for things like adding files and exporting files at the moment.

But it needs to go deeper and be able to, like, you should be able to, like,
it's an important bit of context that's carried throughout the application, like the execution stack of a given operation. And then so these progress events can tell you what's happening, where things are stalling,
and so where you need to look to see why things aren't working.
Demo. I mean, I'm going to actually, no, not a demo.

Because, like, I'm just going to go on a little tangent. So we talk about SIDS quite a lot. And I can't call them SIDS. Like, I have to call them CIDs because, you know, like when people talk about SIDS,

maybe they think of this guy from Ice Age, or maybe this guy from Toy Story.

Maybe this guy, if you remember my talk on debugging things from IPFS camp, SID vicious.
No, not this SID vicious. This SID vicious. This is the original SID vicious. Incidentally, this picture is in the National Portrait Gallery in London. So this is art, actually art. But no, I don't think of SID vicious. I think of this guy. This guy is a guy called Sid James. He's a comedian from South Africa. But he's on TV a lot, like a lot of movies in the U.K.
He died on stage, incidentally, which is terrible for a comedian, I guess.
But like he's a comedian.

He's not the most famous comedian to have died on stage. It was this guy, Tommy Cooper, who actually died on TV. He was on stage, but on TV, which is immeasurably worse. Anyway, Sid James, he was in the Carry On movies. And whenever anyone says Sid, I think of this. Anyway, sorry about that. So back to the demo. So, progress events. So this one's a little more in-depth because we're going to use IPNS for this one. So we have a UnixFS and we have an IPNS, which is imported in the same way as UnixFS. We have DHT routing for IPNS. So one of the things that is really hard to work out what's going on with a lot of the time with JSRFS is that so much is bundled by default. It's not always clear, like, what's going on and where things are going wrong. So with Healy, you very much have... You very much are explicit about the things you want to enable. So IPNS has a DHT routing and it has a PubSub routing. And the PubSub routing is great, unless it isn't. It's hard to work out which is the one that's gone wrong. So if you want, if you're trying to debug things, now we can just say, right, we'll disable this, explicitly enable DHT, explicitly enable PubSub, and you can see which one works and which one doesn't. Anyway, so in the demo, I've got three nodes. We're going to publish a record on one. One node is going to host that record and then another node is going to resolve it. So we create the three nodes and then we connect them in a row. So the publisher dials the host, the record host, and the record host dials the resolver, so they're connected in a line. I've got no peer discovery going on, so the publisher will not contact the resolver. We add a file, same as before. We create an IPNS with the first Helio node. We pass in the DHT and we create a key.
So we create a peer ID that we use to publish the IPNS name.
Now, there's a little bit of sleight of hand here, because, I mean, we all know how IPNS works, right? I mean, I have to look it up sometimes. So what happens is you have a peer ID
and you use that as the IPNS name. And when someone wants to resolve that IPNS name, they look at the peer ID, they find the nodes on the network that are close to that peer ID, CAD close, with the DHT resolver, and they say, do you have the record for this, the most up-to-date record for this IPNS name? You get a whole bunch in the back, you compare them, and you choose the most up-to-date ones. So that means that you have to be able to predict where the record for the demo to work, you have to be able to predict where the record is going to be stored. So this createPeerID function, all it does is it generates a peer ID that is closest to node B, who, if you remember, is the record host. So we can guarantee that the record will be hosted by the intermediate node that's sitting in the middle of A and C. So we import the peer ID into the keychain of node A,
and we use the key to publish our IPNS entry.
And then on node C, we create a UnixFS, we create an IPNS with a DHT resolver, and we just resolve it. I'm just going to comment that out for the time being. And there we go. There we go. So... And then, yes. And then we just... We cap the bytes from that CID, and then we print them all to the terminal. So if I do node demo4... Dun-dun-dun. Hello world. Look, it totally works. But don't take my word for it, because we're interested in the progress events. So we can see, if I charm that back in, when I actually do the resolve... So this horrendous mess is the actual IPNS entry. Dun-dun-dun. So we scroll down, and suddenly we see lots of events happening.

So the first thing that we do is we try to get the record from the local datastore.
It's not found. So then we have to go to the routing. So we make a DHT query. This is the peer that's responded, and here's the value, which is the IPNS record, which we can then resolve to a CID, and then pull it down. APPLAUSE But wait, there's more. Because we can put the in-progress events in the actual resolve as well. Sorry, when we're capping the CID as well. So I'm going to run it again. Make it a bit bigger. And now we see the DHT query come back, and now we're resolving the CID. So we've gone to bitswap. We've done a find providers of the CID. We've dialled some peers to do the file providers. We send them the what list. And in comes the block. Put the block locally. And then here we have the actual export. Progress events. APPLAUSE Amazing. So all these come in in real time as well. So I'm looking forward to seeing some wonderful visualisations of the internals of IPFS nodes. All this stuff will eventually be supported remotely as well via RPC.
It's going to be great. I'm very excited. Thank you for sitting through that demo. That was very long. Phew. OK. The big questions. Is it ready? Is it ready? Can I use it? I mean, we're hyped, right? That was great, right? I hope so. I really hope so. I mean, Helio 1 v1 has been released. So, yes. APPLAUSE Not only that, Unix-adverse v1 has been released. APPLAUSE IPNS v1 has been released. This is incredible. So, yes, you can totally use it. I mean, v1, what does that even mean? All that means is v2 is coming soon. Right, really. But, I mean, yes, v1 is here. You can totally use it. So how can you get involved? Port your apps. So there are no features in JSIPFS now that aren't available in Helio. I believe. Examples. So we have an examples repo. Like Russell has been helping out loads, porting examples. There's a whole slew more of them. If you check the Helio repo, there's a hit list of the most important ones. So, yes, please do come and help. And talk to me. So I'm here. I'm here all week. Try the fish. The fish was great last night. Anyway, like... This is Tommy Cooper again. Never mind. Yes, come and find me. Come and tell me your use cases, what you're excited about. What are the problems you have with existing implementations? And we'll fix it all. It's going to be amazing. So, at the last thing, while I'm still on the stage, I talked about a Cambrian explosion of IPFS implementations. I want to see a pre-Cambrian explosion of browser apps written on JSIPFS and IPFS in JS in the browser. And Helio, etc. It's going to be amazing. I'm super excited. That's it. That's me. APPLAUSE Are there any questions? Go. I'm here to you. One of us. Thanks, Alex. I was just curious. Can someone use Helio in the browser? Yes. Any gotchas or anything they need to be aware of? No. Zero defects. APPLAUSE If you go to the examples repo, there are examples on how to bundle Helio with Webpack and ESBuild. And there's Next.js as well, I think we have now. And there will be more added for more specific browser technologies. But yes, absolutely. Hi. I have a question. I saw during one of the examples, you casually just threw a DHT in there. I did. Is there DHT support? Of course there is. You saw it with your own eyes. LAUGHTER Can I use Helio in Electron? Yes, you can. There's a demo. There's an example running in Electron. Of course Electron doesn't support... Helio is ESM only. Electron doesn't support ESM as the entry JS.
But you can load it using the dynamic import function. So yes, with that caveat. Thank you. Any other questions for Alex?

Thanks, Molly. I'm just curious, what's coming next, Alex? It's finished. We've won. There's nothing coming next. What I'm hoping comes next is things like extra file systems.
So building on top of the primitives that Helio presents. So doing things like, I would love to see WinFS. I'd love to see WinFS running on top of Helio. I believe there's a TypeScript implementation of it. Anything that spits out blocks. That's what I want to see. People experimenting. I'm curious if you know of any examples of... I know it's just hitting V1. But of things that people are building, or that you would like to see people building using Helio. What are some examples or use cases that you think are really exciting?
Or that you know people might already be working on? That's a good question. Because it is box fresh. I'm really keen to get it... I'd love to run a bootstrapper node on it. That would be really nice. Properly battle-test it. People who are running...
So there's the service worker example that Russell's put together.
That's running basically like a gateway in your browser. Which is all built on top of Helio. Anywhere you would have used JSIPFS, you should be able to replace it with Helio.
Are there any plans to take things like IPFS Share, I think was the website.
Or there's the one that Jim Pick worked on back in the day with PeerPad.
Take any of those more meaty browser to browser applications and get them using Helio?
Definitely. Because also the latest versions of libP2P will not be backported to JSIPFS.

It's purely a factor of the development hours that are available. So anything moving forward would look to use Helio. Because then you get all the nice new transports available in libP2P.
So WebRTC, WebTransport, etc. No more star-star signaling craziness.

Just to be clear, Alex, what's the future for JSIPFS?
So the repo... If there are any emergency security fixes, I guess we will try to pull those back.
But the repo itself will not have any further development done on it.
Because we have the equivalent functionality in Helio.
