
# libp2p performance - Max (mxinden) & Marco Munizaga

<https://youtube.com/watch?v=2h9jth3nvJw>

![image for libp2p performance - Max (mxinden) & Marco Munizaga](/thing23/2h9jth3nvJw.jpg)

## Content

Oh, hi Marco, what are you doing here?

Oh hey Max, there's just this thing going on. Ah, this thing, I see. Ok cool, you know a little bit about libp2p, right? Yeah, I know a little bit about it. Oh what a coincidence, I'm actually using libp2p right now. Oh, very cool. I wonder, is anyone else using libp2p? Maybe a show of hands? Ok who has worked on libp2p? Any contributors here? Ok wonderful. He just said he's using it. He just said he's using it. Alright, so libp2p, good to have you here. I have questions. I have questions on performance. I gotta go fast. So what are you trying to do? Well I have many things I want to do. One is I have bit swap requests, right? And I want to send those bit swap requests to many nodes out there, to a subset of the nodes that I'm connected to. And I need to request data from them. And I want to request this data in a way that it has very, in aggregate, very high throughput. I want to retrieve data very quickly. But then, a completely different use case, I also have Cademlia, which is a DHT implementation,

and what I want to do is, I have a bunch of connections open to the network, but I don't want these to be very costly, right? I keep them open constantly. And then also, whenever the user does a request, I have like a burst of requests to send to a subset of those connections, and the requests are tiny, right? Not like bit swap, those are huge. But Cademlia is tiny. And I want them to come back with a response really quickly, right? So latency is super important for me. That's interesting. So with latency, I think there's a couple things we can do here.

Kind of to back up a bit, can you remind me what the TCP stack looks like in libp2p?
Oh, yeah, right. I remember that from the good old days, right? Me back in kindergarten, I was playing with the libp2p building blocks, right? We had lots of fun back then. And our teacher actually told us how the TCP connection is established, right?
So I'll walk you all through it, right? Yeah, yeah, please. Cool, okay. So we establish a TCP connection, right? So that's one round trip between the two of us. Then what we do is we do negotiate our security protocol.
So we negotiate, that's another round trip. Then we actually do the security protocol handshake, right? That's another round trip. Then we negotiate the multiplexer that we're going to use. That's another round trip. And then I'm negotiating the application protocol that I want to speak. Like for example, Kademda. That's another round trip. And then I can send application data to you. Wow, that's a lot of stuff. I've already stopped paying attention.
I think we can optimize some of this. Oh, no way, wonderful. Because I have to go fast, right? Yeah, you've got to go fast. So what if instead of we wait to negotiate, we're just optimistic about this?
Wow. I just assume you'll speak the thing. This is an entire round trip just gone for every single stream. Hang on, does that mean I can use streams super cheaply?
Yeah. Oh, cool. So I no longer need to pipeline requests on a single stream, right? I can just use one stream per request? Yes. Oh, wow, cool. I think this is super important. And in case we're ever at a conference, I think we should put this on the slide, right? I think you're right. OK. So I think we can also do one more thing here. So we are doing a round trip to select a muxer. Can we move this up a layer? Can we include this in some early data? Whoa, another round trip. Cool. OK, so does that mean we save an entire round trip for every connection? Yeah. Whoa, cool. Back then in kindergarten, that would have been cool. Right, I'm kind of hooked. I wonder whether we can do even better. I wonder whether we can go quicker.

So Max, there's this protocol called QUIC. Oh, no way. So just like how we put the muxer earlier in the layering, we can do the same thing

with the security layer, and we could put it all into that first handshake. So with QUIC, we can actually do that whole thing in one round trip. Whoa. Super cool. All right. So back to perf and metrics. What do we want to measure? Yeah, OK. So, well, in general, I think it kind of makes sense to just test latency, right, and then
throughput. Those are the obvious ones. But then I would like to do a little bit more advanced things, right? Cademlia is kind of hard, like I want to do my requests. And BitSwap is a different use case. So I think we should test all of this. But I wonder how. This sounds super complex to have these advanced use cases tested. Well, Max, let me tell you. It's actually pretty simple. So there's this perf protocol, and it's client-driven. And here I'm going to explain the whole protocol. Cool. OK, let's do this. Maybe I understand this. So the client sends the server, you, a U64 that tells you how many bytes to send me back.
Cool, got it. And then I just start sending you a bunch of data. Oh, wonderful. OK. And OK, then I send the amount of data back to you that you initially requested for me.

Yep. And then I measure it, and that's it. Wow, this is super simple. But, well, I wonder whether we can really do all our use cases with this simple protocol.
We can. Just a quick note, do not run this in production. Never. This is purely for testing. Cool. So, yeah, I mean, one of your use cases was latency. So let's talk about how we would do latency. So I would open a libp2p stream, and then I would tell you to send me back one byte,

and then I send you one byte, like ping. Bong. Yeah, exactly. And sorry, Max, I don't think you'd be a very good libp2p implementation.

And so for throughput, it's very similar. For upload, for example, we open the stream, and I just start sending you a bunch of data. I tell you don't send me anything back. Once I finish sending you data, you close the stream. I measure that. Same thing, but opposite for download. And then we can do even fancier things, like, OK, how many connections per second can I handle? And so it's that same exact thing, except now we do this over k connections. Very cool. All right. So how do you set all of this up? Yeah. So this is definitely... What we do here is we have two AWS nodes across the US continent on some pretty big machines.

Why these big machines? We don't want the machines to be the constraint here. We want libp2p to be the constraint.
And... You're running different implementations? Which implementations are those? Yeah. So we're running Go, Rust, Zig, and the HTTPS server to compare.

Very cool. Isn't there one missing? Yes. JS coming soon. Cool. All right. So you have built all of this. What are the challenges? Yeah. So there's this open question of using jumbo frames. And what are jumbo frames? Well, they're any packet that's larger than 1500 bytes. And when you try to send these over the internet, they'll get dropped or fragmented.
But if you're in a data center or in a private network you own, you might be able to use these. We chose not to use them, because we want to try to shoot for something that's a bit more realistic to what most users will see. Okay. Cool. And while you're running the internet, right, and I heard there's this thing called packet drop. What do we do? Yeah. So one thing we have to keep in mind is that packet drops are not uniformly distributed across time. So if I give you these two diagrams, where green is successful packet and red is a packet
loss, which one do you think is more realistic to real networks? Well, the internet is a very chaotic place. I would say the second. Yeah, exactly. And we have to make sure that the test environment we have does not look like the first. Cool. All right. So that seems a little bit bare-bone. What would be, in case you could ever dream, what would be your ideal setup? In case I could dream. What we really care about is realistic environments. And an easy way to do that is just get real network traffic. There's some ideas here. We could collect a bunch of real network traffic and then replay it against a system and then see how that system performs. This isn't just my idea. People do this in practice. Folks who want to benchmark low-latency systems basically replay packets.

But we're not here yet, and we're not close. OK, cool. Well, do you have a demo handy now that we meet here? Yeah, that's right. By coincidence. There's a question mark there, that's why. So here's a larger font. Make it a billion times bigger. So this is just the high-level definition of what we're going to run, which implementations
we're running. And then this is somewhere in here is the benchmarks that we're running.

This part's not important. So here's the benchmarks we're running. And then let's run it. So this connects to each node, starts up the server, goes to the client, has the client

do its thing, and then records the measurements. Very cool. Yeah, maybe we should have done this with a smaller size, because 100 megabytes might

take a bit. But yeah. So let's do... Cool. And then close it. OK, wonderful. So I wonder... Well, this is really interesting. I want to subscribe somehow and get updates on all this. Yeah, so to follow along, go to libpdp.com. That's the main repo where we do all our testing of stuff. And specifically for this work, look at PR163 and just follow along there.
Cool. Thank you very much, Marco. I'll let you do your thing. And yeah, thanks. Thanks. All right. Any questions? What's left for the JS implementation? Oh, yeah. What is left for the JS implementation to be included in the list? Probably like, the one that's in the middle. Yeah. Yeah. Yeah. Yeah. Yeah. Yeah. Yeah. Yeah. Yeah. Yeah. Yeah. Yeah. Yeah. Yeah. Yeah. Yeah. Yeah. Yeah. Yeah. Yeah. Yeah. Yeah. Okay. Think that's it. That's probably what she said. She was думаю over action. So I guessed it then. Our sixth. Sixth feature. If you're in a lot of stream drivers and want to continue Yankees, here's something

that you can do. If we want to continue a few

of our Yankees

here, I'm going to show you a way to manage Yandere JavaScript.
So go to were. If you're doing Chrome, you can use Quake, right? Yeah, you can use Web Transport in Chrome, which does add one round trip, but hopefully will not in the future. Cool. Thank you. Cool. Thanks. Thank you. 