
# Implementations Showcase: Lassie - a new golang implementation - Hannah Howard

<https://youtube.com/watch?v=k6J6nZQCVRU>

![image for Implementations Showcase: Lassie - a new golang implementation - Hannah Howard](/thing23/k6J6nZQCVRU.jpg)

## Content

Hey y'all, my name's Hannah, I'm a software engineer at Protocol Labs and I'm here to

tell you today about a new IPFS implementation called LASI.

So LASI is an IPFS implementation in Go, just like Kubo.
A couple years ago we renamed Go IPFS to Kubo and like magic, and Juan's intention, about

a year later we have a new implementation in Go. But where, so Kubo, as Steve said, is like the super versatile Swiss army knife of implementations.

LASI is designed to be like a scalpel and by that I mean we started with a single goal

and built it from the ground up. We want to help you get your data. We're building LASI because we believe that downloading your data from IPFS should just
work. Basically, we have a guiding saying, which is like, if you want your data, just tell

LASI to fetch it. What does this actually mean? So as IPFS networks have grown, we started to see like a proliferation of transfer protocols,

but for your average person, your average user, like what is a transfer protocol? Do they care? Does anyone who goes to a website today think, well, I guess some people might still use
FTP as opposed to HTTP, but like for the vast majority of people, they don't care. They're just thinking, get my stuff. And so we in LASI attempt to speak multiple protocols.
Today, LASI already supports both BitSwap and GraphSync and within the next month or
two, we're going to be adding a trustless HTTP protocol transport.

It'll be sort of modeled after the gateway. Maybe in the future, we might want to add some additional cutting edge protocols, such as the ones you may hear about from IRO or maybe Fission's Carpool.
We're going to be, as new protocols appear, LASI is going to keep up with them.
So of course I forgot that I have bullet points on my slides.

Cool. Yeah. So another thing about LASI is we sort of have identified that when people go looking
for their data, they don't want to worry about how to find it.
They just want the data back. So in LASI, we find your data for you.
And that means we speak to both the IPFS DHT, but we also speak to the Filecoin network.

So we can retrieve content from Filecoin. And this is all done through IPNI, the network indexer. It actually handles most of this for us. We can speak to the DHT, the storage provider network. We can even track down some providers that don't put their content in the DHT, but tsk,

tsk, you should put your content either in the DHT or even better, put it in the network
indexer. And we'll keep adding new and better ways to find data as time goes on. So how do you use LASI? From the start, we've designed LASI with three core use cases that we are attempting to enable.

The first way to use LASI is simply as a command line executable. You can download it as already compiled and ready to go and run it immediately to fetch data. We've sort of designed it like most Unix commands to be pipeable and composable, and I'll demo
that in a moment. The second way to use LASI is as a lightweight HTTP gateway to IPFS and Filecoin.

The LASI server exposes a trustless HTTP gateway interface, and that serves car files to you.

LASI does go beyond the trustless HTTP gateway spec to support full querying via pathing

and depth parameters, and we are in the process of writing a proposal back to the spec through
the IP process so that other implementations can implement some of the querying tools that

we've put in. Finally, we've designed LASI from the start to work as a library that you can easily incorporate

into your Go application. We want developers to use LASI seamlessly to add retrieval from IPFS and Filecoin to

your projects. Essentially, we see this as the superpower of LASI. If you have a Go application and you want to be able to get content from IPFS and Filecoin,
you can just plug in LASI and get that content. We would love to partner with other teams in helping you to integrate LASI into your systems. There are a couple of things that LASI does not do, and this is somewhat intentional.

LASI is lightweight, and being lightweight means you have to make some cuts. Our sole goal is to retrieve data, so there's some stuff that gets left out. Specifically, we don't store blocks. We have no block store in LASI. We just return CARV files to you and let you manage your data and decide what you want to do with them. Because we don't store data, that means we also don't announce to DHC. This is a retrieval client. It is not designed to be a server node you run on the IPFS network to provide data to

the network. LASI is, through and through, a client, and so we do our best to stay almost completely

stateless. We hold a little bit of information that helps us retrieve data a little faster when we're running as an HTTP server, but beyond that, there's no config file on your hard drive.

Any knobs you want to turn are done through environment variables, and there aren't as many of them as in KUBO. We've done all this for us to keep us focused. We want to work on getting retrieval of data right, and so we're trying to keep our features
set intentionally limited. So let's take a quick look. We're going to look at a quick demo of LASI in action, and as I said before, we have a
command line, and that command line can pipe its output to other commands, so here we're going to fetch a SID, pipe the output of fetching that SID to the CAR tool, and we're going

to use the CAR tool to convert it from a CAR file to a FLAT file, and then we're going to pipe that output again into FFmpeg and see if it will play it as a movie.

Let's see if this works. There we go. There's a couple extra things in here, but you can see we're piping to CAR, we're piping

to FFmpeg, and immediately you automatically get a video playing on your screen.

So yeah, exciting. Let's yeah, and now it's a party. Let's see if we can get to the next slide before everyone starts to dance. All right. So yay. Cool. So LASI is a new tool, and it's in development, but it's not a prototype.

We've already built it out a fair amount, and it is already the primary tool that powers

cache misses in the Saturn network, and so through the RAM project that Will spoke about

earlier where all of your gateway requests are ultimately going to be filtered through
the Saturn network, they're ultimately going to be fetched with LASI. We are already in testing downloading about 140 million SIDs through LASI each week, so

it's definitely getting used. We're increasingly recommending LASI as the easiest way to download your data from Filecoin.
A funny thing about LASI is that we originally started as simply a tool to retrieve from Filecoin, but we realized along the way that these networks aren't that different, and you should just be able to get data from everywhere. And we have a full team focused on ironing out the remaining bugs and trying to improve
performance. We're invested for the long term, so if you want to integrate LASI into your project, I would say maybe go for it. The only area we have a little bit of work to do is in our documentation, and that is going to also be improving soon. Cool. If you want to hear more about LASI, come to the data transfer track. We're going to be doing... It starts right after this morning's keynote. We're going to be doing a deep dive into the software architecture and how we want it to evolve over time. Alternatively, you can come find me at the conference. I'd love to chat with you and how LASI can benefit your IPFS project. Also, I think... Is Kyle here? Not yet. One of the other team members, Kyle, will be here. You can also talk to him. And a special thank you to Kyle Huntsman and Rod Vag, who have put in so much work over
the last few months in getting LASI's first iteration out the gate. They're awesome team members. Finally, use it. Give us feedback. You can file a GitHub issue. You can hit us up on Slack. You can complain about us on Twitter. You know the whole drill. And that's it. Thank you so much. Cool. Thank you.
