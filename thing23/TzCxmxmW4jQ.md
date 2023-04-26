
# Implementations Showcase: Nabu - Java IPFS - Ian Preston

<https://youtube.com/watch?v=TzCxmxmW4jQ>

![image for Implementations Showcase: Nabu - Java IPFS - Ian Preston](/thing23/TzCxmxmW4jQ.jpg)

## Content

Hi everyone, I'm Ian Preston from Pegos. I'm going to talk to you about NABU, which is

a Java IPFS implementation. But first I'm going to answer a question that's probably on everyone's mind. Why Java? Do I really want to be writing abstract factory-factory

IPFS implementation beans? Well actually there are a bunch of reasons. It's a super popular

language. It's the third most used language behind Python and JavaScript, or C depending
on your metric. It's universal. It runs on mobiles, in browsers, desktops, IoT. Fang

are big users of it. 90% of Fortune 500 use it. And there's huge corporate buy-in, so

if you want companies to use it, that's a good idea. So I think some people here found that trying to run a Go binary at Microsoft is actually a big problem, whereas they're
totally set up to run Java. This actually surprised me, but it's apparently faster than

Go according to the Debian Benchmarks game. It has world-leading pauseless GCs. JVM and
Java are extremely durable and backwards compatible. I personally run code that I wrote 20 years
ago unmodified and run it again today and it still works. And runtime code loading,

A plugins are a first class citizen. But enough about Java, let's talk about Nabu. If the

clicker works. So Nabu, it aims to be a minimal, embeddable IPFS implementation, so you can

store and serve your blocks over standard libp2p. Our motto is root your peers, not

your content. So the idea is on the application level, you often have more information about where your data is, so we want to let people use that and say actually I don't want to just ask the world for this data, I want to ask this node by this peer ID. So you can
see for the get block call, we've added an optional argument which is a set of peer IDs.

If you don't supply that, then yes, we can still fall back to the DHC and do the normal lookup. And there's enough API if you want to implement GC or pinning externally if desired.

So what's the current status? We've only been working on it for two months, but it's already usable. We have the TLS security transport with early Mux and negotiation. Cadamly including

IPNS. There's BitSwap with an auth extension and the secure API to use it. There's a peer

to peer HTTP proxy, which is awesome. Everyone should use this model. It's also in Kubo.

Standard port mapping and Bloom and InfiniFilter block stores. If you want to know what an
InfiniFilter block store is, come talk to me later. They're super cool. So real world benchmarks. So this comes from Pyrgos. This is basically, can you mirror this DAG for me, please, as fast as possible? It's a champ. It's six layers deep, about

6,000 blocks or about 20 megs. So not actually much data, but a lot of blocks. And mirroring

this in Kubo currently takes 120 seconds using the pin command. NABU takes 12 seconds. This

isn't actually optimized. The theoretical minimum is, well, with BitSwap, which this
is using, is six round trips. So it should be optimally about a few seconds.

So our conclusion from this is BitSwap, the protocol, isn't actually bad with sensible data structures. So yeah, please do try it out and give us some feedback. And come to

my Sunday talk tomorrow on a better web and private data. And finally, a huge thank you to the IPFS fund, without which this wouldn't have happened.
Thank you.

