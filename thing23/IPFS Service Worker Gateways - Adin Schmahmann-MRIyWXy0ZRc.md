
# IPFS Service Worker Gateways - Adin Schmahmann

<https://youtube.com/watch?v=MRIyWXy0ZRc>

![image for IPFS Service Worker Gateways - Adin Schmahmann](./IPFS Service Worker Gateways - Adin Schmahmann-MRIyWXy0ZRc.jpg)

## Content

Hi everybody, my name is Adeen, I work at Protocol Labs, I've been working on IPFS

for 4 and a half years or something now, and excited to be here with you all.
And we're going to talk a little bit about how to put IPFS gateways and sort of put them

into the browsers, but more on the web platform side, like service workers and fetch API and

not like C++ code that lives in the rendering engine kind of stuff.
So gateways, thankfully, a number of folks have talked about this and seen this interact with it, so it needs less explanation. But IPFS, we move around content addressable data and graphs, and people would like to

see that, you know, I have a UnixFS graph of a bunch of bytes, I'd like to render my Koala JPEG, please, and do it in my browser, and gateways sort of let you do this.

Gateways have a few different jobs, somewhat simplified, it's I would like to find this

stuff, I would like to find who has what I want, get it from them, verify it's correct,
and then render it as my JPEG or website or video or what have you.

Processing is things like content and peer routing, data transfer, fetches for data transfer,
and then verifying and rendering is basically the IPLD and processing work.
The things in blue are the things that some people associate with as like, oh, these are the IPFS things, because these are what they see running on big public gateways that are run by protocol labs or CloudFlare, but these other things are all valid parts of the IPFS

ecosystem as well. You heard Will talk about how maybe we add GraphSync in on the data transfer side, but

there are so many more things that are out there.
And as part of this, the way that many of us interact with these gateways is via public

HTTP gateways, things that are run by, say, protocol labs or CloudFlare.

And these sort of have downsides. So there are resiliency pitfalls associated with this.
The ones that most people think of when they're like, oh, there are resiliency problems with the fact that dweb.link is run by a company is operational ones.
Someone messes up a DNS config or a browser blocks a DNS name or a machine overheats or

somebody accidentally turns off IPv4 on the machines or the NGINX caching is wrong or

none of these have happened in real life, I promise. Or someone decides that running one of these public services is expensive and they decide to shut down. But there's another one that happens more under the covers, which is ecosystem resiliency
issues, which is needing to get buy-in to test new things.
So if you go back here and you're like, I would like to support BitTorrent and mainline DHT or I would like to support CarMirror or BAU for like three blocks or MDN for like
two blocks, for SHA-2 blocks or WinFS. Like how do I do that right now? I can either run a gateway myself, but those are kind of expensive and like, didn't I just
feel like I wanted to work on WinFS and not like run a gateway? That wasn't really what I wanted to do. Or maybe I can go ask protocol labs and CloudFlare if like they feel like doing this, but they may have different priorities than I do or work at different timescales than I do. And that feels pretty bad. So idea, let's try and move more of the gateway work to the client side so that the set of
things I'm depending on the people who are running services to do is smaller and less
likely to get in my way. And less expensive for them too. I could like do this by running a Kubo node locally or embedding it in my browser or something,

like Brave does. And then sure, if I want to add, you know, Graph Sync support, I'll just, I'll fork my Kubo, I'll add the Graph Sync thing or a Car Mirror. Like I'll add my Car Mirror support with the plugin and be like, hey, look, it does Car Mirror. And then I'll deploy it locally. But that's kind of unfortunate because I now have to deploy this binary to everybody.
And I hear this web platform thing has remote code execution on like millions of, you know, billions of devices across the world and everyone's cool with it. Why don't we use that? And let them, and let the browser do the work for us. And this involves letting us, you know, okay, so let's try and do this in a browser.
Let's choose what we want to delegate and what we want to do ourselves. So we have find, we have fetch, we have verify and render.
We have APIs that we can use to sort of cross these boundaries. APIs that will do, you know, everything for us, fetch, you know, find, fetch, verify and
render, which is, you know, the trusted gateway API, you know, IPFS.io, IPFS CID.

There's the trustless gateway API, which Will mentioned, where you have things like ask for a car file. And we have a routing API where you say, who has the stuff?

The more of these things you need to do, the more expensive it gets. So find is relatively cheap. It's, you know, you sort of do, you do a database lookup, say, I'm going to do a DHT, I'll do a DHT lookup, I'll query an IP and I node, return some result.
Fetch is pretty expensive. You have to proxy all of the bandwidth of everybody who feels like using IPFS stuff. So that sounds pretty expensive. And then verification is, so I did the finding and then I did the fetching and then I had to do the processing for all of the graph data. And that adds even additional cost.
And so there's, there's a spectrum here. So if you use the, the trusted, you know, Raya version of this, the client does nothing

IPFS related. It uses CID.contact to find it, Saturn to fetch it, Bifrost gateway to render it.
And there you go. Little Bear Labs' Chromium racing gateway, they do rendering and then ask someone else to do the rest of the work. I think Alex's talk after me will do something similar. Brave embeds a Kubo node and says, we're doing everything, get out of my way.
And I'm like, hey, I feel like there's a missing one. What if we just, what if we let somebody else do find for us and we just did fetch and render, which are the most expensive parts of the process?

So why now? Is this like a new idea? Has nobody had this idea before? Like what's going on?
Some of this is like, there's been a lot of growth in the last year and people pushing on new protocols in various ways who are getting held back by this more, right?
As we get more growth, then becomes more pressure on the controlled and expensive endpoints
that say support my thing, which is harder for them, right? And sort of stifles the growth. There are bear market conditions, give you another look at like expensive infra, that's

public for everybody. Maybe not optimal. The IPFS ecosystem and the web platform have improved to make this easier for us now.

We have services like Elastic IPFS that have web socket support, secure web socket support.
We have a trustless gateway API that allows for fetching blocks if I just slap a TLS cert

in front. If I don't have a CA signed certificate, I now have web transport, which is usable within

lots of browser contexts, unlike WebRTC, which is not and has lots of won't fixes sitting

around in the Chromium and Firefox code bases. But libpdp is adding support for WebRTC, too, which helps us out.
And we now have APIs to ask somebody else to do the fetching, whereas before that was not, we'll say they existed, but not in a way that was very useful unless you had exactly
one thing you were trying to do. Not very useful if you wanted to experiment or grow the set of data transports you were
supporting, like having HTTP data transports not previously supported.

So let's take a look at some demos. See what we got.
Let's start with... Yeah, let's take this guy. I could have two browsers because of the service worker.

You just want to make everything a little cleaner. I'll clear everything out so everyone knows I'm not cheating.

And away we go. You got to register your service worker the first time. That's sad, but not necessarily a game-ender. All right. And away we go. And we asked CID.contact, hey, anyone know where this CID is? They said, yep, some of these folks have it. We said, okay, which ones speak WebTransport and WebSockets? Okay, these ones do. Connected to them with the B2B and use BitSwap, fetch data, render data, got a video.

You know, we can do more of this.
Here's the docs website. Let's see if the demo got a smile.

Oh, not so smiley today. Of course, five minutes before I started talking, everything was fine, but you never know.

Let's give it one more shot.
Kick everything and start again. All right. I hear turning it off and on again always works.

Demo gods. All right. And something slightly different.
Okay. And you know, we can do directory listings and get our JPEGs and so on. And in the interest of time, I'll skip the website one for now. But let's say you were like, okay, this is cool. I've heard of BitSwap and fetching data and whatever. Have you heard of, like, my cool new protocol? And this guy Marco shows up and says I would like to please support HTTP fetching of data.
And I would also like to support fetching of HTTP data over libP2P, over web transport,
because I think HTTP is pretty cool and I would like it to work even if you don't have a certificate. But I've only got, like, three days to do this. And the only way I can figure out how to do this is to, like, hack it really deep into JS BitSwap and see if that kind of makes it work. Could you please merge this into your upstream code base and deploy it to all your users? And maybe you're like, yeah, cool, let's do it. I trust you. You're awesome. And maybe you're like, no?

But because this is, you know, sort of like the browser world we're operating in, you

can say instead you don't have to run your own gateway to proxy everyone's traffic for

you. You can just basically deploy a website. In fact, you don't even need to deploy it on Fleek or anything. You can just deploy it on, like, as a CID, load it through dweb.link, and then, like,

load your thing. And it will go and fetch I guess I should close the tab so you can see what happens.

But dweb.link will load, like, the service worker loader thingy, which will then go and

load it, right? So it's like Marco has been able to deploy his loader without asking anyone, without
needing to do much. He could have used it with, like, he could have deployed it with, like, IPFS, he could have put it on GitHub pages or whatever, and he would be able to do it.
I could do this, and I could say, oh, you know, something broke, Marco, that's sad. Good thing I didn't deploy it on my production system. Or maybe it all works, right? And that experimentation is fine. It's like part of it's like a healthier version of how to do this, because it doesn't require
spinning up humongous amounts of infrastructure to give this a shot. Yeah, demo guys decided not to smile today. That's great. Let's go back here. We can come back later. All right. So sort of where we're at. So we delegated out find, we did fetch, and we did verify, and we did render.

We have tools to help websites integrate. So you can use either the fetch API or you can use service workers.
As you have seen, service workers have rough edges.
And this means that when you integrate, you can do something like closer to, say, what WebTorrent does, which is give you JavaScript hooks to deploy your code that fetches and
renders your data for you. Or you can use the magic with the service workers. And depending on, you know, your inclination, you might decide it is easier for me to take my existing website and use the service workers and make the magic work.
Or it is easier for me to, like, use the fetch API and have more control. Both work. You can do the same code with two different wrappers on top. That sort of let you do this. It's easier to fork and add a support for a new protocol.
Or a new data structure. Like if you wanted to support WinFS, you could do this without needing to redeploy everything.
And it's certainly the least expensive gateway you can run, because all you have to do is like deploy some JavaScript to somebody's web page, and then they can, you know, cache that and whatever. And maybe let's not proxy all of the data, which is by far the most expensive part of this process. So where do we go from here? This is put together pretty quickly. We do not have full compliance with all of the gateway specs. You know, some of the edges around exporting tar files and range requests and, you know,

some of the, you know, redirects files and whatnot are still missing. Let's clean those up. There's still a bunch of, as you can see, battle testing and hardening of this that needs to happen before it's like widely deployable. But it would be pretty cool to use something like this so that if you hit, you know, a
central service, right? Like do you have.link? And you get a 429. Sorry, you asked us for too much stuff. Or hey, you asked us for a 100 gigabyte file. And like, no thank you. Or something like that. You could try again in a minute and see if that's okay. Because, you know, maybe your spamming is now acceptable for you to try again. Or you can just do it yourself. And that is an option that's now available to you.
And then adding in support for, you know, alternative protocols and data types that

aren't, there's no current way to get at with the commonly deployed gateway implementations.

How do I join the party?
Try it. Report back. As things improve, see which things get better. Which things that don't. Which things, you know, still need more work. File PRs. File issues. I've seen a number of different things where people have tried to do, tried to work in
a very similar vein now that we're starting to unblock a lot of this.
You can share ideas. We can share code. Either seems good. Both seems better. If you have something you feel like has been missing, right, you have a data transfer protocol. You'd like to see how it works. Let's see if our abstractions are workable enough to plug in your thing and make it operable.

If you run an IPFS implementation that supports QUIC, enable web transport so people and browsers
can fetch your data. If you can't, they will need either certificates or WebRTC, both of which are pretty sad.

Enable web transport. If you have GoLoopP2P as part of your thing, enable web transport.
And for those who are here in person, there'll be some hacking going on tomorrow. You can check the channel for this track for more information.
I want to thank a bunch of people who did the lion's share of the work.
Russell and Marco have been awesome at both putting the demos together and plugging all

the pieces together and finding all of the things that have been lying around for a while and making them operate. And finding web transport bugs and fixing web transport bugs. And implementing HTTP over LoopP2P inside of a web browser and then plugging it in.

And then there are a lot of people who do not work directly on this, but whose work we have directly or indirectly benefited from.
Particular shoutouts to Mo and Alex.
And all the folks, many of whom are now hanging out in the DAG house who worked on the IPFS

service worker demos from four years ago and JSIPFS gateway code from around then.

And to the many more of you that I've talked to or have helped this move along.
Thanks. So I thank you all for coming.

