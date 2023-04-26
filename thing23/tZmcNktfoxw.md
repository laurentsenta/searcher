
# Measuring on the fast track - Asmir Avdicevic

<https://youtube.com/watch?v=tZmcNktfoxw>

![image for Measuring on the fast track - Asmir Avdicevic](/thing23/tZmcNktfoxw.jpg)

## Content

Cool. Okay. Thank you. My name is Asmir. For those who don't know me, I am an engineer

at Number Zero, mainly working on the IRO project. Yeah, I'm a mostly numbers kind of

ops type of guy. I like to stray from the main path and just do, like, a lot of different things, but I try to always come back to the numbers because that's what I asked for. At

Number Zero, we're building IRO, and we're trying to do our own IPFS implementation that kind of closes the gap between Web 2 and Web 3. We were at camp, and we then presented

some numbers which people found interesting, and today I just wanted to expand on that
a little bit and present what we did, how we did it, and, yeah, maybe we can share some
experiences. Yeah, so I don't know if anybody recognizes this image. I highly recommend

the talk behind this. This came out because I tried to find an image for a slide, and
I wanted to do a what, how, why about the presentation, and Wetman is a really funny
talk that I like to listen every now and then and have a good laugh, so I highly recommend
it. Anyways, the what. So, yeah, I like being pragmatic about the stuff I do, and so most

of my work incidentally also has to be pragmatic because we want to move fast and we are a small team, and I just keep building and building more and more tools to measure more stuff and produce more numbers. Over the past year, there were a lot of things that we built,

and I just want to show them and maybe present the impact it had on our work, how it shaped

the course of our work, and make a small summary and distill some takeaways, some learnings,

gotchas, I don't know, painful moments, something like that. So, yeah, the how. I'll explain

these images. This is an old kind of computer or server that I had laying around the floor.

Now it's cleaned up, and this is where our infrastructure for metrics lived for quite a while, actually. I now moved it into a nicer cabinet, but it's still the same stuff. I
got a lot more gear that's on my table that isn't in the rack, which it should be. Anyway,

so we started about a year ago. We knew we wanted to do numbers from the start and just
like optimise bit by bit and do very focused precision level adjustments to what we do.

We wanted to do smaller pieces instead of the full blown IPFS implementation. We wanted to take out the individual bits and do them one by one and slowly iterate on those instead
of trying to do the whole balloon, because there's a lot of stuff to do. So the hard
part for me is this was basically the dark metrics ages at number zero, because we had
no infrastructure, we had no code, no dashboards, no nothing, except there were furious engineers
trying to produce as much code as possible and they wanted numbers yesterday. So yeah,
I had to survive that. And the why for the presentation, I don't really have a super

strong reason, it's just that I like to share engineering banter and just share and learn from everybody's experiences, and I hope to do the same for you two here. Q&A is formally

at the end, but you can grill me on the spot, like this is meant to be very casual and you
can just raise your hand and ask questions throughout the presentation. Cool?

So the timeline. I used Ali to generate this image, it's a Capybara traveling through time
from the Big Bang. No strong explanation, I just don't know why it has wheels, I apparently

need wheels to travel through time. So yeah, let's go to the timeline and I'll just start

showing. So commit number nine was implementing metrics. I really had to rush this one and

just show some numbers so we can start iterating. It was fairly simple, like this was collect
simple counters, I don't know, numbers of requests, bytes served, bytes received, I
don't know, just to get something going. And it was exposed as a single Prometheus endpoint like most people start out. Obviously that's a little bit hard to consume for them
so I had to build more. Yeah, those that are interested in the Rust
specifics, I started with Tokio's metrics crate and it was nice, but the whole community

otherwise was using Prometheus client and different tooling, so for compatibility reasons

with libpdb specifically because we wanted to dive deep into that, I shifted the library

that we use underneath. It also allowed me to hack a little bit more so we did more things
to make it easier for us. Yeah, the second bit that I had to do is just
prop up some infrastructure and you saw the box that hosted our infrastructure for quite a while. I wanted to avoid lock in because we didn't really know what tooling we were

going to use and how far we would need to scale or not scale and we just wanted to be
as fluid as possible with that. So I basically took a very simple Prometheus stack. I took

timescale DB which is basically Postgres with Centwix to get the counter data in and what

I did to kind of like help the devs out a little bit is convert all their code to kind
of like be pushing data out to push gateway instead of like scraping as you usually do

with Prometheus. This had some bad, well, not bad, but it had some properties like the

push gateway continually keeps the last value that you sent it and if a node dies or otherwise

doesn't report any more data unless you scrub your push gateways regularly, it just reports
the last value you've ever seen. And if it drops off, you don't know that it dropped

off because you see like a constant line and if you don't monitor closely, you lose track of it. And I set up a Gopher Fano dashboard so we can just see that that was our portal
into Metrix. Let's see. Yeah, so the first tool that I built just to keep things going

is a small finger utility. It basically hit a few well-known endpoints that we had set
up on our deployments just so they would generate traffic and people could see their own thing
working, doing something because otherwise the only method of testing was somebody had to manually go and try to fetch stuff or see stuff and just make our lives easier. It wasn't

really a super helpful tool. It just kept things going and it was useful in the first days just to see if everything got broken down or stuck or whatever.
So moving on from there, this was actually our first proper tool that actually brought a lot of insights into our work. Back at camp, Thibault from Cloudflare actually presented

a very similar tool which was also similarly named. And I was really glad to hear about
that because that meant whatever I built here wasn't really a totally off idea. It had some
sense to it and was useful beyond our own walled garden. So what I did here is we deployed

across a number of regions and we wanted to build a few test cases to have a playground

where we could do apples to apples comparisons to other implementations. The easiest ways
for us to do it is just use gateways as an entry platform for all and have a tool that

kind of hammers those with specific requests and test cases. I had a few boxes that would

spin up random data and push them to the network and see how fast they would resolve, what's
the latency, time to first byte, time to, I don't know, download the whole thing. There
were cases where you would do a repeat request so you can test the caching properties that
it had and so on. This turned out to be really good because I deployed it about the time

we had our first working version of Iroh at that point, or at least we thought we had
that with it. The first numbers that came in, they were horrible for us. We were doing,

I don't know, 5, 10, 15% of all resolutions at all, not looking at the numbers of the
time or latency it took to do things. This actually motivated the whole team to go all

hands on deck and see what the hell was wrong with it. So at that specific time we were

just starting out with our BitSwap implementation and BitSwap is a finicky thing. The spec is super light and it seems super easy to do. In reality it's probably the hardest thing you'll have to implement regarding the whole IPFS network thing, at least initially. We

had a lot of trouble seeing why it didn't work. You can see that it doesn't work, but
it's really hard because, again, superficially it's very easy. And then we started digging
into it to see. So we had to backtrack a couple of times and we went and compared against

Kubo and its implementation, what it did, and obviously had a lot more implemented at that point. So we started to backtrack and just keep adding more and more of the surrounding stuff to BitSwap until it started kind of working. We did a lot of refinements and we

got up to par with the rest of the gateways and eventually we managed to be like the top

three in terms of numbers, ability to resolve and everything else. So things were rosy.
We were really happy. This was maybe like a month or two before we had to go to camp.

Remember the rosy moment because it wasn't that rosy. So this is the second tool that came in. It came in about halfway when we were working
through the gateway checker stuff that we discovered. Internally we called it the test
stream, but basically George from PL was kind enough to hook us with a substream of the

gateway requests on a certain region and basically forward us the requests as they came in. So

we can do some real world testing in the whole thing.
And I basically took that stream, made a simple utility that would replicate the requests

across a number of machines that we had standing. I compared to like IRO from the main branch,

IRO from a dev box that we used to continuously test, Kubo for a couple of versions, and we

basically just iterated on those. The first thing that popped out to us is we
were back at the beginning because our numbers sucked again. Turns out the real world is

very different from artificially set up tests and it's really hard to try and predict what

you're going to encounter on the real network versus whatever we have locally set up. A
few machines here and there and you try to transfer data. That usually works out kind of well, but if you start joining the actual network and serving actual data over the gateways,

it's going to be hard. It took us quite some time and a huge effort
from the whole team to start figuring out all the issues. We had issues with UNIXFS pathing and we had issues with consuming data from all the different NFTs that were being

served. I think they didn't provide their records to the network. You only had the root provided. So we had to work on actually fetching the data out. And we slowly built that stuff

up. And we came to solid numbers on the whole thing. And again, we built it up. We're going

to draw back again. But it was one of the tools that actually showed
us the value of continuously testing and testing with real stuff. In the same time, we were

doing a lot of benchmarking, like benchmarking the ad performance, get performance, and so
on. And the camp numbers that we showed and numbers that we might share here, in some

we are better than Kubo, in some we are worse than Kubo. And I can pretty certainly say that those numbers don't always reflect the reality. Kubo beat us out functionally on
a lot of things, even though we had better numbers on raw ad and get performance. We
managed to get back on our feet and actually do a pretty good job on resolving stuff and
doing stuff. But real world testing is king. Until you put it like testing production,

basically. There's no substitute for that. You'll have to burn yourself on that.
And the performance benchmarking we did, there was a lot of fun with that. I initially took a cloud box and basically ran the code side by side and different tests. And you get some

numbers. If you repeat the same test throughout the day ten times, you'll get different numbers.

Not all of them are different. Wildly different numbers. So what happens? Well, it turns out
cloud providers have virtualized and abstracted so many layers that you never really know where it runs or how well it will run. The first issue for us was this was very inconsistent

on cloud providers. So we used a provisioned I.O. machine or whatever. Which was nicer,

but it still wasn't really that consistent because sometimes you get a slightly faster machine for whatever reason. Even the same configurations of machines never really perform the same across our testing. Same kernel, same system, everything else.

So what we decided to do is build our own boxes. You can't really substitute having your own hardware, a controlled environment, fixed dependencies, everything else and then just running your stuff to test against. So I highly recommend whoever is doing any kind
of sort of benchmarking, unless it's a fleeting benchmark locally to see relative to yourself how you're doing today. If you want concrete numbers over time, fixed boxes, dedicated
boxes. Yeah. During this time, we had a lot of issues.

We discovered that even when everything was rosy and things were working, after three

days, seven days, 15 days, things would lock up. And we were having abnormally large, like,

download or upload numbers. And it was odd. It took us time to figure out things that

were happening under the hood. One of the issues we actually never really resolved because we just decided to move on from that and do a different thing.
But we had to instrument everything. Like, logs didn't really help because there was
so much volume of data and everything else coming through that you couldn't really, like, humanly filter it to a sensible amount. Like, even if you filter it down to a set of logs, you don't really understand them if you look at them that way.
So metrics. Just numbers. More numbers. And just aggregate more and more numbers until you start making sense of it. For our BitSwap implementation, we literally

instrumented every for loop, every if, every event that was sent out, everything to get
a little bit more sense. So we had a lot of run loops and a lot of, like, branching predictors

and a lot of calendars for events and just trying to connect, like, when this event happens,

what happens there? And it's terribly hard because it's a very soft system and you're talking to a number of nodes. So we decided eventually to kind of, like, try to isolate that out and test out some things. I'll touch on test round a little bit later.

But yeah. We had a lot of trouble in this period. But it helped us a lot and got us

ready for camp. And we actually managed to deliver a functioning product at that time which had good perf characteristics and we moved on. We used this tool later and it actually

helped us to get to the point where we were saying we were going to do something else. And basically it was, like, changing tides. We decided at Iroh to just go back to zero

and revisit all the little bits that we were doing and just take control of our full stack and build it from the ground up. Test stream showed, like, after all the, like, different

fixes and improvements and optimizations, we were having decent results. But eventually
things started breaking. Or for some certain use cases, we were having suboptimal results even though we should be having probably good results. One of the things that really put us off was we found out that Rust and Go, even though
they should talk together perfectly, they don't. And it was really hard to maintain
connections between Rust and Go clients. And that essentially meant we were kind of cut off from the public network in a real sense. Like, sometimes it worked, but usually it
didn't. And that's when we knew we had to take control of our own stack and just start
doing really, really iterative design on very small bits. Anyways, our priorities changed in terms of where we want to put effort in. And I had
to build more tooling. So I built a new repo. It's called Chuck. And the reason it's called
Chuck is because I just chuck stuff in there. It's a lot of, like, stuff to where I have
to play around, a little bit of scripts to help me out and do stuff.
The priority itself, the repository itself is really not that important by itself. But

it was the birthplace of NetSim. And NetSim is basically our homebrewed version of TestGround.

We did a couple of runs with TestGround. Those unfamiliar, it's a way to simulate a larger
set of network connected machines and just let them play out with a little bit of coordination

and metrics collection added onto it. It was a little bit heavyweight for our use case. We didn't really need Docker eyes or Kubernetes environments spinning up thousands

of nodes and so on. We wanted to test connectivity. Two machines was initially enough for us and maybe five later just to make sure the basics work and then we could talk about measuring
the whole network. By the way, thank you, ProBlab. Those are good numbers to have.

So we decided to do our own. And NetSim is a small tool based on Linux namespaces. It
uses Stanford's Mininet to prop up the network locally and it allows you to set up a number

of nodes with isolated spaces to run processes and then you just let it play out. It's weightless

overhead. We could really quickly iterate on it. Our dev loop on TestGround was fairly

slow. Building continuously takes time and we wanted to move fast. So this was a lot
easier.

also we didn't really need all the bells and whistles and coordination that TestGround provided. So this was very easy to just keep spamming more and more different test cases,
scenarios and actually get fairly raw performance out of the box that we were testing on in general. It was also running on a dedicated test box and what

we wanted to use it is to measure it against Web2 technology because that's where we're trying to go. Kube is great functionally to test against but it's not our performance like benchmark. We wanted to get to performance numbers that are in the Web2 realm to make it useful and possible to regular folks.

So I built a small tool, small web server that serves content and we fetch it via

curl on the other end. The reason I did this is I wanted to make it apples to
apples and whenever I say apples to apples it means there's a lot of bias in the decision-making process and bias is inherent to all these measurements.
Whenever we do measurements I know they're going to be biased one way or the other. I think it's important to be just aware of them and make sure it's a
fair comparison because you can optimize web servers and curl responses and
everything else for your whole life and try to get more performance and
optimizing Web2 was not really our play. We were in the like IPFS space. So I

just wanted to give it a fair playground to test against. The end result of that

is that we decided to own our own metrics and basically go public with
numbers. There's a very small subset of the numbers live at perf.iro.computer. On camp we promised we're gonna like keep doing numbers, numbers,
numbers because people like that and now we're just gonna like put them out there so people can do and incrementally see what we did by each commit and by each day how stuff performed and if it falls we own it and we figure out why it fell

if it grows that great even better. So yeah you can follow along numbers that

we now have out there and yeah I took heavy inspiration from perf.raslang
because they also have their own counters and like on bad days it's bad on good days it's great. So you can follow along. Besides this I recommend
having like a discourse place where you can chat with other people and just have

them chime in. We have a perf channel on our discord that some people have joined and it's been really useful to have like people from a completely different place
just come in and see something and just drop you a line or a hint or something and it really helps out like getting a different perspective on things we were doing and it's really helped us through us throughout the old iterative process
that we had so I fully recommend having a place to talk with people. Yeah that's

for the most part it. Here are the takeaways that I kind of like to count for myself here but I hope you take some too. It's basically make sure that stuff
people build is continuously benchmarked because if you don't really test and benchmark it people let it slip and slide somewhere else. Biases are always

present in whatever numbers you give there. If you're on cloud you're obviously biased towards some sort of like configuration. If you're locally then maybe my own box that dedicated to testing is flawed because it's one
platform and the other platform performs very differently for the same good or whatever. It's really hard to cut out all the biases but if you do have them try to be aware and just like adjust for them and make sure that it's fair so if you're testing against something make sure they try to like even the playground. Yeah test against real stuff. Synthetic

benchmarks lie a lot. So perf numbers whenever you generate them like micro benchmarking that's good for your day-to-day development but on the grand scale of like things working or performing well production testing. Yeah

use dedicated hardware. More metrics are greater for development. They're not necessarily the best tool in the world for better decision-making. I tend to distill metrics to a small number of those that we deem important like top
level performance indicators whatever to like kind of steer a ship but day to day more metrics better always. And if you can do one thing like automate everything
and make it repeatable because only then you can like iteratively test on something. If it's not repeatable that test is like a one-off just like curiosity. Yeah that's about it. Here's a big thank you on the screen and if

there's questions or anything I'm happy to answer. Other than that here's my some contact info how you can reach out and yeah. It seemed like a big fork in the road was trying to replicate behavior seeing in Kubo that didn't match I guess a spec or like a lightly specified. Yes yeah
that was a very big part. Can you dig into like what you were working from and then how that went? Oh sure so I'll touch in two bits. One was because

personally I was working on doing the first pass on our own gateway and the second one was doing the whole thing that bit swap. Bit swaps spec is fairly

concise and very idealistic. Turns out that as soon as you'd like want to be

part of the network it's not that it's not part of the spec it's just that the spec says what it has to do but the spec doesn't tell you like what everybody else is going to be doing on the network. So sending out requests and receiving
responses that's great but once you start adding more nodes people figured out that that was overwhelming them so they started like limiting how much messages you send out or receive back. There's issues because once you

want to receive all the bits of messages you have to parse them you don't really know the contents of it or who sent it or why and then you have to like go through them analyze them see if you want to read them or discard them or whatever which introduces a lot of like volume issues you start getting a lot of data in whether you like it or not. So people start really limiting stuff
around we did on our end too and there were things like where just

what was there were more issues so out of spec.

I can't remember specific details can I get back to you just just being me like
there was a lot like I can just share you a whole dump of the discussions on that. Again the business spec is not the issue that it was out of

spec like the implementation versus the spec it's that the spec is very simple and covers very little but the real world is very different in terms of like what you get. Eventually you want like a lot of state management in there you
want to remember which peers streamed what so you can optimize. You have to
manage them a number of peers because otherwise you get overloaded and there's a lot of little funky things that you have to do to maintain stable bitswap like discussions with other nodes without just like nuking your own

machine even though it's by spec. The gateway was a little bit different story

when I started writing it there was no spec but fortunately just around the time where I've written mine by copying some of the Kubo stuff over to Rust the spec came out and I was fortunate enough to be very close to the spec and it helped me iron out the like last bits so I think the gateways so far are quite
up to spec for example compared to everything else. Yeah hope it helped a

little bit but feel free to catch me around later and we can chat more about it. Anybody else? I have a quick question on the dashboard that you were showing.

What are the numbers like it's gigabits per second one to one one to ten

are these just network node numbers or is it yeah okay okay so it's not node

numbers as in node one to node ten is one to ten right okay and it's the same

everywhere okay interesting. So this is a very simple snapshot. I'm going to expand on these numbers quite a bit over the next few weeks or quarter or whatever.

Okay and and the latency are like link latencies?

Yeah, we're trying to get the loss numbers to like between the cross links of those serving and those receiving.

Okay. This is just for like comparison sake to see the impact. They don't really serve a super high purpose right now but we want to intend to model some specific real world use cases that we know about in terms of like user perceived latency numbers or loss numbers and basically try to replicate or emulate such networks which are a lot more complex than like

just doing one to one transports with a little bit of latency. Right, okay. Excellent, thank you. Thank you. Really quick question. What was the reason to have like locally dedicated hardware as like in comparison to just...

You can rent boxes like dedicated boxes. We do some boxes on Hetzner too and just run numbers but if you want to benchmark stuff you need to use dedicated hardware not virtualized hardware.

Gotcha. Cool. Okay and if there are no more questions then yeah let's thank the speaker again and yeah carry on with Guillaume.

Thank you very much.

