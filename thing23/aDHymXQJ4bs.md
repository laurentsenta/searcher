
# How to build your own compatible libp2p stack from scratch in an afternoon - Marten Seemann

<https://youtube.com/watch?v=aDHymXQJ4bs>

![image for How to build your own compatible libp2p stack from scratch in an afternoon - Marten Seemann](/thing23/aDHymXQJ4bs.jpg)

## Content

Hello, hello. To the what is libp2p talk, I'm Martin and...

Hey folks, I'm Marco. And we're both on the Stewards team and we work on libp2p. So, what is libp2p, Marco?

Okay, so we start with a raw TCP connection, we use multistream to negotiate noise or TLS, and sorry, to back up, there's these building blocks.

Wait, wait, our slides are not on. Oh no, okay, pause. Wait, okay. Yeah, so libp2p, we have these building blocks, we have transports, we have multiplexers, we have secure channels, we have peer discovery.

So we start with a raw TCP connection, then we use multistream to negotiate noise or TLS, and then we use multistream to negotiate the multiplexer, yammix, or mplex.

No, no, Marco, this is not what libp2p is nowadays. What? So libp2p nowadays is mostly just QUIC on the transport layer.

So we have QUIC, we have other transports, we have fallbacks for TCP and WebSocket and WebRTC, we have WebTransport, which is basically also just QUIC, and let's look at some numbers.

So here you see two Grafana dashboards that I generated a couple of days ago, screen-shotted a couple of days ago, and it shows the distribution of transports that my Kubernetes is using.

So this Kubernetes is not doing anything special. I just set it up on a virtual server somewhere at Linode, and as you can see, it is handling about 90% QUIC connections.

So this is the dark blue and the light blue part here, which then for different QUIC versions, but for the purposes of our talk, this is just QUIC.

This is 90% QUIC, and then we have a little bit of TCP and a little bit of WebSocket and other transport.

So these numbers make a lot of sense when you compare them to what Google and Apple are reporting.

So they obviously have large QUIC deployments, both on the server and on the client side, and they also observe that about 90% of the time that they try a QUIC connection, they actually succeed in getting a QUIC connection.

In 5 to 10% of the networks, UDP is blocked because network admins think that UDP is evil.

So you still need a TCP fallback, but for the vast majority of connections, it actually works.

And in Lip2P, when we dial a new connection to a peer, we dial all of the addresses that this peer gives us simultaneously.

I know this is not perfect. This is not optimal. We are working on it, but that's what we currently do.

We don't give QUIC any advantage. We start the QUIC connection and the TCP connection at the same time.

And then we use the connection where the handshake finishes first.
And this just shows that QUIC is really QUIC. The handshake just finishes a lot faster than on a TCP connection, and that's how we end up with more than 90% of QUIC connections.

So now that we've established that Lip2P is basically just QUIC, why do people still use Lip2P, Marco?

So Lip2P gives you more than just QUIC. We have UPnP and Friends. We do NAT detection.

We can determine if you're behind a symmetric NAT, full-code NAT.

We can do automatic relaying with the whole Kubo network, IPFS network.

And we have hole-punching protocols built in out of the box.

So another reason why people use Lip2P is because Lip2P comes with building blocks.

When you're building a peer-to-peer application, you probably want some kind of DHT.

And Lip2P comes with the Cademlia DHT. And Lip2P also comes with GossipSub, which is a protocol to exchange messages between different nodes.

And that's used by both Filecoin and Ethereum. So by building on top of Lip2P, you can just reuse these components and rely on the fact that they are probably just going to work.

So this is how we think about a modern Lip2P stack.

So on the transport layer, as I said, it's like 90% QUIC. And then we still have the fallback transports for the cases where QUIC doesn't work.
Then on top of that, we have the NAT traversal magic.
It's very complicated. It's not fun to deal with NATs and how to get through them.

So it's really nice to have a stack that just does that for you if you need it.

And then on top of that, we have the applications like Cademlia and GossipSub.

And of course, you can build on these building blocks your own protocols that do the actual peer-to-peer stuff that you're interested in.

All right. So we kind of talked about what the ingredients are for a lean Lip2P stack.

So just to recap, we can really build any interoperable Lip2P client with really just these four things.

So we just pick any QUIC stack. Here on the right, we have the Wikipedia entry for QUIC implementations.

We add the Lip2P TLS extension, which is how Lip2P ties peer IDs to the TLS certificate.

And then we implement peer ID encoding, which is that slash P2P stuff. And then we send the multi-stream header when we negotiate streams in QUIC or whatever.

And so introducing ZigLip2P, a lean Lip2P stack.

For this, I picked MSQUIC. It's Microsoft C library for QUIC. I picked it because it's fast and well-tested.

It's also C, so that makes using it very easy.
The Lip2P TLS extension was pretty straightforward, probably about 200 lines of code.
Most of that is because I don't have a protobuf library. I'm using OpenSSL. Things are a little verbose there.

Peer ID encoding, also around 100 lines of code. I'll say most of that is OpenSSL trickery.

Multi-adder parsing, pretty straightforward. And sending the multi-stream header, also fairly straightforward to implement.
This is kind of like the source tree of what the implementation looks like.
You can see it's pretty bare bones. But, um... Will it blend? So, yeah, I mean, it's pretty bare bones, but it still is able to talk to every other QUIC Lip2P implementation.

And, uh... And, yeah, so this data here comes from our interop tester, which makes sure that different implementations can interoperate.

And if anyone is making their own Lip2P implementation, I would encourage you to also take a look at this and add your thing.

And we'll talk a little bit more about this in a bit. But why? Right? Okay, we have all these other great implementations, why do I want the ZigLoop P2P?

I think it's useful to have this as a proof of concept. Starting from QUIC, you can build a very lean interoperable node. You don't have to start with Lip2P if you find yourself already in a different path.

Lip2P gives you a ton of stuff, but with QUIC you're really a stone throw away from being interoperable to the whole Lip2P network.

I wanted to provide a C API to enable other implementations to bootstrap even quicker.
If they just want to use my code for the TLS extension stuff or something like that.

And I wanted to experiment with MSQUIC as a Lip2P node. MSQUIC has some really... I'm a really big fan.
They're really fast and I like their API a lot. And so I wanted to see what it would look like to have that as a Lip2P node. And really I wanted a different Lip2P library. So this thing has three dependencies. libc, openSSL, and MSQUIC. I make few assumptions of what the caller wants to do. And I focus on performance from day one. And it's no... I submitted the perf protocol very much inspired by this work.

Okay, so here's a demo. Demo.

Where's my mouse? Okay, so this is a node. You can see here I'm calling... I'm going to run a ping binary and you see a multi-adder. This is just some go Lip2P node that... You want to increase the font size? Yes. Good call. Can you hold this? Okay. That's probably big enough. Cool. So this is just like a standard multi-adder. You would see a quick node. Yeah. And there we go. We get some ping responses. So that's the Glut2P pinging a go Lip2P node.
And let's...

Okay. So... So should we merge something on stage? Let's do it. Okay, so we just showed off the interoperability head results, which is like, you know, the tip set comparing to all the published versions. But I want to take this a step further. And let me make this a little bigger. Can you hold it good? Yep.
Okay. So I mentioned a little bit about this interop tester. I showed you a screenshot of Zig head being tested against all the released versions.

Now I want all the other implementations head to test against Zig.

So just to be clear, this is now... Once this pull request is merged, on every pull request we have in go Lip2P and in Rust Lip2P,

this test will be... And NimLip2P. And soon JS Lip2P. And JS Lip2P. Fill in the description first.
This is going to test against Zig Lip2P as well. All right. Three, two, one. Cool.

Cool. That's all I got. Where are the performance benchmarks? Yeah, good question. There was a performance talk earlier today. Where were you? Okay. So follow the test plans. We have some infrastructure being set up here on performance. Like, I think as a bigger Lip2P effort, we really care about performance.
And we're putting a setup into test plans. So if you want to follow along, this isn't ready yet. We have numbers, but they're just not ready to share. But follow along on the test plans repo and specifically PR number 163.
That will give you all the perf information. Woo! Applause

