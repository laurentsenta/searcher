
# Measuring IPFS - Yiannis Psaras

<https://youtube.com/watch?v=nvQ18xUua20>

![image for Measuring IPFS - Yiannis Psaras](/thing23/nvQ18xUua20.jpg)

## Content

So Dietrich put me here in the spotlight to talk about ProbeLab, which is another way

of saying data-driven protocol design and optimization.
So this is what we do. We're collecting and analyzing protocol data that make up all the components of the IPFS

network. Because you know, things sometimes might work, but not in ways that you want them to or expect

them to. So you've got to go and dig into the details to see how things work.
And that's what we do at ProbeLab. If you navigate to probelab.io, you'll find this new web page and a collection of plots
out of the tools that we have put together in order to dig into the details and see how
the protocols actually perform. For example, you're going to see DHT lookup measurements from several different regions
of the world. What is unique about what we're doing is that it's not just a collection of plots, it's
got context as well. So we're including details about what the experiments look like, what did we do, why

did we do it this way, as well as links to normally GitHub repositories where the tools

that we have used live. So you get a whole view of what we've done to see if it's convenient for your purposes.

What we're also doing is that we're producing weekly reports, which you might have seen around. And these are kind of snapshots of the performance and health of the network on a weekly basis.

So more kind of short term and bounded. As part of that, for example, we're monitoring a bunch of websites, the time to first byte from again from several different regions. Your website might be there, so go check how it performs.
And we compare the time to first byte from several different regions through Kubo and

through HTTP, just to see where we are. This week, for example, you'll see that more often than not, content through IPFS, through
Kubo, is loading faster than HTTP. Which is great. It could be, you know, content addressing and universal caching that make up all the magic. So go check it out, see if your website is there. If it's not and you want it to be, just come talk to us and we can probably add it.
Now we know that IPFS as a system is composed of many content routing subsystems.

And we know that we are focusing on a few of them right now. Primarily the IPFS public DHT, but also we're looking into bitswap, hydras, and so on.
Now as we go on, we want to expand to other content routing systems, either on our own

or with your help. So if you're building a protocol, if you have a protocol or a content routing subsystem,
and have measurement and monitoring tools, come talk to us and we can work together to

showcase how brilliant is what you're doing.
So where to find all these numbers? Right now we have two places. You have two places to monitor. One is stats.ipfs.network, which points to the weekly reports, which is, as I said, a

snapshot, a summary of the health and performance of the network. And then it's probelab.io, which is what I introduced before that, which is more detailed,

you know, nitty gritty stuff about protocols, their parameters, you know, how does the performance

evolve if you change some of them. So it goes into much greater depth. Now soon, what we're going to do is that we're going to bundle everything under stats.ipfs.network
so that you can navigate from there to different places. Yeah, that's it. We're having a measuring IPFS track after this session here in a room near you.

So figure it out. Come with your questions and come to find out more of what we're doing. Thank you. And I will put in the spotlight Will Scott, I think. Thank you. Thank you. We'll see you in ten.

