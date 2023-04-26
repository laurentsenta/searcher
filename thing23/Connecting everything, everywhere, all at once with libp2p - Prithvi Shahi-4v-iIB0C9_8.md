
# Connecting everything, everywhere, all at once with libp2p - Prithvi Shahi

<https://youtube.com/watch?v=4v-iIB0C9_8>

![image for Connecting everything, everywhere, all at once with libp2p - Prithvi Shahi](./Connecting everything, everywhere, all at once with libp2p - Prithvi Shahi-4v-iIB0C9_8.jpg)

## Content

Welcome everyone to connecting everything, everywhere, all at once with libp2p. As you

can see on this slide, I think we beat the record of most people involved in a single
talk. Was there any other? So I'm standing here, right? But this has really been a big

team effort, that's what I'm trying to say. Okay, cool. Before we talk about connecting

things, let's first talk about libp2p. I assume most people here are familiar with libp2p,

so I'll just walk through this really quickly. It's a peer-to-peer networking library. There's one specification, and then that specification is written, implemented in many different languages, like Go, JS, Rust, Nim, C++, Java, and so on. It implements a bunch of low-level

features like transport protocols, encryptions, authentication around transport protocols,

mechanisms like hole-punching, and then once we have this base layer, we can then build higher-level protocols on top, like for example, AcademiaDHT or GossipSub, or protocols like
BitSwap. And the big wish, or the big statement, is all you need to build peer-to-peer applications.

Okay, when we talk about connectivity, what I usually do is I pull up this very colourful

table of protocols that you need to connect things, and then I got the feedback that this

is not very entertaining, so then I rewrote my slides. And what we're going to do instead

is we're going to talk about a movie, and then I'm going to be really sneaky and then talk about Libidipi. Okay? So, Hugh here has watched the movie Everything Everywhere All

at Once, won a couple of Oscars and things like that. Okay. Cool. Okay. So, what I think

is that, well, what's the movie about? It's about a bunch of people travelling through

the universe, right? And what I think, what my suspicion is, is that they're actually
using Libidipi under the hood. And today I'm going to prove very thoroughly that they're

using Libidipi under the hood. Okay. So, how am I going to prove this? Well, I'm going

to showcase a small application called Libidipi Chat. There are not enough chat applications

out there yet, so we definitely need a new one. And it's this Libidipi Chat application,
a peer-to-peer chat application, and through this, we're going to demonstrate that in the
movie Everything Everywhere All at Once, they have actually been connecting everything, which are browsers, non-browsers, phones, laptops, and so on, everywhere in public networks,

on public networks, all at once, using Libidipi. Cool. Folks ready for this? Now comes the

sneaky part, right? From now on, we just have grey slides. Okay. All right. So, we want

a chat application, right? And we want this to be distributed. So, the first thing that
we're going to do is we're going to spin up a Rust Libidipi server somewhere, right? It's publicly reachable, easy scenario. And then, given that it's quite alone out there by itself,

we're going to also spawn another server, which is called Libidipi. They're talking with each other, not very hard to connect to servers. They have full connectivity. You have access to UDP and TCP sockets, so not a lot of trouble here. Okay. So, let's add
some complexity. And now, we actually want to chat with those two servers from my laptop

there. Okay? So, what we're going to do, use for that, is we're going to use QUIC to connect

from my laptop to one of those Rust Libidipi servers, to the Rust Libidipi server and later

on also to the Go server. QUIC in Libidipi has been there forever. Small caveat, it has

been there forever in Go Libidipi. We shipped it recently in Rust Libidipi, and there's more work in the other implementations. And what it allows us to do is connect a non-browser
to a non-browser. Why am I saying non-browser? Well, anything where you have access to UDP and TCP sockets. I'm happy to go into details about that later on. All right. So, obviously,

I'm going to demo all of this. So, what I have here is, yeah, my laptop. And then, I'm

going to start the Go node. And ha-ha, ta-da, the Rust peer actually sent us a message.

And what we can do here is send a message back. Right? Cool. That was my talk. Thank

you very much for coming. Yeah, thanks whoever is in the chat application already. Okay.

So we have been hacked, but it doesn't matter. We can move on. So now we have the laptop

connected to the Rust Libidipi node, right? And you're kind of like, this is not really cool. Like, connecting a laptop to a server, great. I'll add a little bit more to this,
which is another laptop. And like this. And what we can do actually with QUIC and also

on TCP is connect two laptops with each other, right? Even though they're both behind NATs
and firewalls. So we can do a technique called hole punching to then interconnect the two.
And this I'm just not going to showcase today because we're focusing on other things, but there are other talks previously, which we showcased this one. Okay. Cool. All right.

Now all of that complexity, let's introduce a very big one, namely a browser. And on that

browser we're going to run JS Libidipi. And now given that the talk is called connect
everything everywhere all at once, we actually want to connect this JS Libidipi node. Okay.

So what we're going to do first is connect the JS Libidipi node running in a browser

to the Rust Libidipi node. And we have one small problem. The Rust Libidipi node, we

have been lazy to set this up properly with a TLS certificate. So the Rust Libidipi node doesn't have a proper domain and it doesn't have a signed certificate. So the browser would usually never allow a connection to the Rust Libidipi node. It would always need a signed certificate within the browser's trust chain. But what we can actually do with the Libidipi WebRTC direct protocol that builds on top of the browser's WebRTC stack is connect

to this public server, this Rust Libidipi node, even though that Rust Libidipi doesn't
have a signed certificate. Now, in case you say like this is super insecure, like we built

encryption and authentication and so on on top of that, as like everything in Libidipi is always encrypted and authenticated. Okay, cool. So let's do that. So for that, I have

this cool application here, the universal connectivity chat app. And hopefully, ta-da,

this is now running JSLibidipi in my browser. And what we can see here on this line, kind

of hard for everyone, but what it says there is WebRTC direct. So what we actually did is this laptop was JSLibidipi connected to the Rust Libidipi server, even though that
server doesn't have a signed certificate or a certificate within the trust chain of the

browser. Okay, cool. Well, this is easy. I can then go to the chat, and then I should

probably do it like this. And then thank you, whoever this is. I can chat with this anonymous

person that is probably sitting somewhere over there. And I can say, hi, Go node, because

no one else knows the URL yet. And this should then, yeah, ta-da, show up also on my Go node.

So actually, as you can see, my JS node is somehow connected to the Go node earlier, that's also running on my laptop. Okay, all right. So we have a WebRTC direct connection

now. Obviously, the arrow to the Go Libidipi node is missing over there. So let's do that
one as well. And for that, we can use the Shiny Web Transport Protocol, building on
the Shiny Web Transport Protocol from the JSLibidipi browser to the Go Libidipi node.

And again, Web Transport allows us to make a connection to a remote node, even though

that remote node is not within the trust chain of my browser. Okay, cool. So if I go back

to my chat application, I can actually see here that it already did that. It connected

to the Go node. What you can see here, I'll make this bigger. We connected over UDP, QUIC,

and Web Transport running on top of QUIC from my browser to the Go node out there. Okay,

cool. What are we going to do next? Now let's add even more complexity to this. And let's

add yet another node to this. And let's add yet another JS node to it. And this is again
running in the browser. And now you again are missing a link right between JSLibidipi
and JSLibidipi. Wouldn't it be cool if the two browsers could actually connect to each other? All right. So for that, we have the Libidipi WebRTC protocol, again, building

on the browser's WebRTC stack. And this allows us to connect the two browsers with each other.

And in the idea case, even open between them. And I'm going to showcase this as well. Hopefully

in this case. So again, and then the node, it will connect to the Bootstrap node. To

both of them, actually. And eventually, it will also, ta-da, connect to the browser.
So the third connection. And now what I can do here is hello, browser. And that should

now show up in the chat. Cool. So we connected to two browsers. And at this point, we have

connected everything within our whole chat application across all the many nodes. Cool.

And now, given that no one has seen this chat application ever before and no one has ever used it, you can now all use it. So grab your phones, your laptops, and so on. And you can

go to this URL or go directly not through the QR code and then actually use the new
best chat application ever. Right now, we don't small disclaimer, we have a tiny bug

on Firefox. So you'll have some trouble with Firefox. But if you use Chrome or if you use

Firefox in, I would say, a week, we'll actually have that resolved as well. Yeah. And now
you can use that as a chat application from now on throughout your entire life. Cool.

What is next? Well, as I mentioned, we have this small bug fix, right? In our Liberty

WebRTC stack for Firefox. We want to get WebRTC direct. So from a browser to a public server

node into GoLiberty. We want Firefox itself supports the web transport protocol. Every

browser out there can connect, actually, or the majority of browsers can connect to GoLiberty nodes. Eventually, we want to add web transport support to RustLiberty, even though we still

have to work a lot on our quick stack in RustLiberty. So that's not happening anytime soon. And then also eventually we want to have WebRTC browser to private. So to add even more complexity
to this huge matrix, we want to be able to connect from a browser to a GoLiberty node, even though that GoLiberty node is behind a NAT or firewall. Cool. Then obviously we
want to add emoji support. Then after we shipped emoji support, we'll ship the feature on how

to travel through universes. And then lastly, we'll add ZigglypityP to this entire thing.

And then really have everything connected everywhere all the time. Cool. This work is

all in public, and we actually already have a next-door contributor contributing to this

universal connectivity app. So I'm very sure that this chat app is really taking off at
this point. And yeah, he actually joined multiple community calls at this point, and he ported

all his patches to our stack, which is wonderful. Where can you learn more? Well, you all have

this. You all scanned the QR code, right? So you have that. Then if you want to know what you're actually running on your phone there, this is public on lib2p.universalconnectivity.

Then on connectivity.lib2p.io, you find all the details about all the different transports.
A lot nicer displayed than my earlier matrix in gray. And then lastly, all the protocols

that I talked about today are fully specified, and you find them on lib2p.specs. Cool. That's

all from my end. Thanks for joining, and yeah, happy chatting on the new chat app.

Happy to take questions. So Max, if I was an auditor, was there any

... Sorry, is there anything you were covering over in this demo or in the Universal Connectivity

app? Is there any magic that we're... Yeah, anything we're covering over, or is this all legit? Ah, you don't trust this? I'm just poking it. Okay. Yeah, so at this point, well, obviously we have smaller bugs, right, in the UI and

so on, around how this actually works. But at this point, you actually have pure Rust lib2p, connecting to go lib2p and js-lib2p, all at least in master or released.
Cool. If I can ask one more, you've talked about web transport and webRTC. I guess, why

would I use one over the other, and are there any caveats? So whenever you can, use web transport, because web transport builds on a lot newer QuickStack,

and thus gives you a lot more performance. Just, for example, connection establishment way faster, or proper stream multiplexing and so on. But then, as a fallback, you can
use webRTC direct, which allows you the same capabilities, but on the older webRTC stack.
What browsers support web transport? Yeah, web transport, I think at this point,
Chrome or Chromium, and then Firefox and Nightly. Yeah.

Quick question about other transports, like BLE or other near-field connectivity. Can

you give us an update on where those are? I don't think we have had any updates on this,
in the sense that most focus has always been on the Internet Protocol stack. There's always
been community members popping up with Bluetooth and things like that, but I don't think we
have made any bigger progress on anything that is not running on IP within the larger

lippity implementations. I have a question.

I think it's on. Okay. I have a question around webRTC. I think
I remember seeing, reading, whatever, that signaling servers are sometimes involved with

webRTC connections. They're what, sorry? Signaling servers. Yeah. They're needed? Yes. So even in your case, you need some signaling service. So is there in this diagram, essentially,
is there something else, or does one of the P2P nodes do the signaling? How does that
part work? Yeah. Okay, cool. Let's dive into this. So in the case of webRTC direct, we can actually get around without stun and turn. So we don't

need any signaling and we don't need stun. This is really cool, and this allows us to establish a connection. Otherwise, you have a chicken and egg problem, right? Where's your peer-to-peer part if you all depend on the same signaling server, right? And without a signaling server, how do you establish the first peer-to-peer application, right? But the cool thing about webRTC is that it actually allows us to make that very first connection to a public node. And then from there, we can use this public node. We don't
need to use turn. We can instead use pure lib2p. Lib2p has a relay protocol. And instead

of using the official turn, we can use that relay protocol instead. Now a small caveat. So signaling is peer-to-peer, but the webRTC browser stack still needs stun

servers, right? So right now, you could either provide your own stun server or use one of

the bigger ones out there. But the important part is that if the two of us want to connect over webRTC, we don't need to use the same stun server. So you might just use any other
server. So it doesn't introduce any centralized infrastructure in that moment. Okay. So essentially, you would have the signaling service, but effectively, they're not...

They can be any lib2p node out there. And actually, so for example, in the IPFS network, we have every public node act as a limited relay server. This is not a problem for the public IPFS nodes because it's very limited and very restricted in the sense of what you can do. But this enables anyone not public to use those servers to transfer very small

signaling bytes, basically. And we can make use of that without, again, having centralized infrastructure. All right. Cool.

If two browsers connect over web transport and they want to get a message... They connect

to a server over web transport and they want to get a message to each other, that would
presume that there's a relay going through the server? Or you use a gossip protocol. Like in this demo, we're actually also using gossip sub,
which is a publish subscribe protocol. And in this case, they both connect to the rest
of the lib2p node. And this way, in case they don't have a direct connection to each other, like in the case where WebRTC, for example, doesn't succeed, they can either make a relayed connection or they can actually gossip the messages across. Does that make sense?
Okay. Thanks. It adds a lot of complexity. Sorry.

So Max, you said gossip sub is used in this particular example. How is peer discovery happening? Peer discovery right now? Ah, okay. All right.

So this RustLibDB node is running the Kademlia protocol and as soon as one of them joins it, it will do a random walk, it's called, and will discover other peers. So for example, it will basically ask GoLibDB, hey, do you know anyone else out there? And in this case, it doesn't know anyone else. But in case the GoLibDB node here connects, the GoLibDB node will ask the RustLibDB node, hey, do you know anyone else out here? And the RustLibDB server will actually tell the GoLibDB laptop that there's also a GoLibDB server. And this way, they slowly discover each other. And this is, for example, if you open the app, it will slowly more peers will actually pop up. And the more people here open the app in their browser, your browsers will actually discover the network and this way discover the other browsers and then try to do a WebRTC direct connection, right, do the hole punching, and in case that doesn't succeed, either relay or gossip the messages. Cool?

Okay. This sounds fine for servers and desktop browsers, but it sounds very, like, heavy for mobile radio. Like, this sounds like it would just burn battery on mobile browsers.

So I don't think, like, if you build a larger application with this, I don't think you should connect to everyone within the network. I don't think that makes sense, neither from a server nor from a browser.

Within your protocols, you need to be somehow smart who to connect to. But, for example, in the case of GossipSub, you can kind of like delegate the further gossiping as a browser to a server node where the server node has more connections and can actually have your message travel through the network.

Okay. I mean, this is a common thing with P2P chat applications. There's a few of them where they're very bad for mobile experience because of the battery issue. So there's nothing here that would solve that magically, I guess. It's left as an exercise to the implementer.

I'm pretty sure this chat application is not yet ideal, and I don't think I have figured out the battery drain feature, but I would argue that you could figure this out with, like, being more advanced around, for example, gossiping to the first hop and then have that first hop gossip further. But, yeah, this is hard. This is a hard space.

Just a quick answer to that. You can think of Loop2P as just adding some flavoring around the connectivity to make it transport independent. It should work like any normal HTTP system. So think of your browser on mobile making a request. You can configure Loop2P applications to behave exactly the same way and that way not cause that battery drain issue.

I think this might be about, in, say, GossipSub, creating a lightweight client that then wouldn't maintain that many open connections or would only connect to a specific relay or things like that. And that's the way of tuning that particular problem.

I also think in mobile specifically is where BLE and other kind of near-field communication things will be super, super useful because there's a classic problem of you have a phone, you have your laptop, you don't have a Wi-Fi network, you want to move some information from your phone to your laptop.

And now these days, Apple and Google do this really, really well with their protocols, but peer-to-peer hasn't caught up to that and we need to close that gap. Then you would actually be able to do this really nice magic link wormhole moving your information.

