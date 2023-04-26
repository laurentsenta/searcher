
# We Moved The Bytes, Where Did They Go? - Panel

<https://youtube.com/watch?v=k_GcYmV5QTQ>

![image for We Moved The Bytes, Where Did They Go? - Panel](./We Moved The Bytes, Where Did They Go - Panel-k_GcYmV5QTQ.jpg)

## Content

So to just kick this off, I'm going to kind of introduce what the panel is, and then I'm

actually probably going to, Brendan, if I might ask you to speak first, since Mood the Bytes was originally your undertaking. So in November, I think as I mentioned in the initial track, we formed a group to make

data transfer awesome in IPFS. It was called the Mood the Bytes Working Group. It was originally set up with a goal of replacing bitswap as the default protocol in four months.

Is that accurate? That was the original goal? Yes. Okay, cool. So we've been running this group now for a few months. We've actually gotten a lot done. There's been some amazing protocols demoed, built, lots of stuff.

But we haven't actually, there hasn't been a bitswap replacement across the board yet,

so I want to kind of just reflect on where we're at and what the process was like and where we're going next. So I guess I would start, and I'd love to hear, like, Brendan, from you, like, what

made you want to start this group? What was it like running it? What do you feel like went well and what maybe do you think we could improve upon?
So yeah, is that a good intro? Take it whatever direction you want. It's a great intro. Totally. Thanks, Anna. Hi, everybody. It's nice to see all of you. I deeply miss all of you, and I really wish I could be there in person. So hopefully you can hear me. If you can't, Anna, interrupt me. But yeah, move the bytes. So I don't know that folks know the origin story behind this. We, Dig was about to go give a talk at IPFS camp, and I was standing next to Carson Farmer

from Textile, and we were like, you know what? We should stop complaining about this. And just like a lot, a number of us are working on this. And so we went around and just like quickly took a poll of a number of the folks who are in the room. I'm like, hey, would you join this working group if we made one to like try and do something about our data transfer story? And I believe we talked to some folks at DAG House, some folks at Vision, some folks at definitely a bunch of PL teams, folks like in multiple teams at PL. And everyone's like, yeah, let's do it. Let's go working group. And so we jammed a slide in and named it move the bytes in the like sort of 20 minutes before

Dig went on stage and said, okay, cool, let's do it. And then we actually had the first meeting, I think that Sunday, and just at IPFS camp,
just to say, hey, can we gather some requirements to understand what this was? And really, it definitely like from the get go, I was like, very fired up to try and make

a major community contribution in the form of getting us all to work together.

Because I think I caught a little bit of Philip's talk. We have a lot of data transfer protocols, we have a lot of knowledge in our community around how to move bytes around. And it just seemed a lot of us have sort of like openly mused that like, it's kind of tragic that we're working on disparate things. Why don't we try and coordinate and work together and build one thing or fewer things.
And so we started with the bytes. And we had numerous meetings and it became very quickly apparent that it was going to

be really hard to actually ship code. Like I basically just like threw together a timeline of like, cool, we're gonna and like, try something more experimental with the working group thing. Mainly because in other working groups, we'd sort of gotten this feedback that like, we don't always get stuff done. Like, what is the value of this working group? And if we're going to ask people to meet every two weeks and commit time to this, can we make sure that they get something out of it? And the thing I really wanted out of it was like better working code. And so that was the first couple of meetings, we sort of had this timeline that I kept posting up and like, cool, we're going to evaluate different options, we're going to go to like an implementation phase. And then we'll and then we'll choose a protocol and use that to be a replacement for bit swap. And that kind of got softened and softened and softened over time. And really, that was you could see that as like, not accomplishing our goal.

But you could also see that as just like, that was sort of the needs of the community
emerging at the same time, I think would be another narrative, where what I think we ended
up with in Move the Bytes was a really nice place to do knowledge share outs on work that's
been happening to date. And I'm actually quite proud of the videos, like the recordings from those meetings, and

the presentations that everybody gave, I thought we created a space for folks to really show some really interesting stuff. And I personally have been like, affected by numerous talks given by numerous folks
in those working group calls. And so yeah, the original purpose was like, heck, yeah, we're all gonna work together, we're gonna ship some code kumbaya. And the like, ending spot was like, you know what, maybe we should just have some space to listen to each other and be influenced by each other. I don't know if that's good or bad, but that seems to be what happened. And hopefully, that's like a nice summary of where we ended up. And I'm gonna stop rambling now, but does that feel like it tees things up, Hannah?
That totally tees things up. So I think I'm gonna reflect for a couple minutes on my own experience of Move the Bytes.

And then Martin Seaman from the LibP2P team is gonna do a couple minutes.
And then the remainder of the panel is not a panel, it's just a discussion, particularly
whether you were in the group or not, feel free to contribute your reflections.
And we'll see where it goes. I'm not gonna, this is a sort of like a retro, but not like I'm gonna have like a pluses and deltas sheet going and we're not gonna have, yeah. So we'll just see what we learn. Yeah, so my own experience, sorry.

It's hard to start without mentioning that my first experience of Move the Bytes was I was running said data transfer track and I did not have the information that the Move the Bytes group was going to be announced. And so it was a surprise to me, which was really exciting though, because it was sort

of totally aligned with what I thought needed to happen, was to get these different groups
working together. And I also from the beginning was a little skeptical of the four months around, let's

build a protocol in four months together that replaces BitSwap. And I think part of the reason that didn't happen is I think we actually revealed some

things that were inherent problems in an endeavor like that. And I think that in that, that's not actually a fault. That doesn't, again to like Brendan's point, that actually was a strength of the process

as opposed to like, oh, we didn't replace BitSwap, which at the end of the day isn't
probably the most important goal. I think we really, some things that I saw coming out is that people's, similar to the

talk that Philip just gave, like people's use cases dictate how they want to move data
to an extent that makes it hard to have a single mega protocol.

And we quickly came up against some people really wanted to just move blocks and other
people had other needs. And I think that, I think it was useful to start to identify all the different concerns

that kind of come out around data transfer that aren't just like, how do we technically move this data quickest? The other thing that I think is challenging about, I don't think this is really a solved
problem in our ecosystem, but how do we work together across teams where teams are actually
potentially different companies? It's hard enough within a large, different teams in a large organization like Protocol Labs, but we have here truly a network where many of the teams are their own entities.

And it's tricky because everyone has their priority list in a work stream and a manager who has said, these are the things you've got to get done. And it can be very, it's not 100% clear how to mix that up across organizations.

So I thought that was really interesting. I thought, again, that Brendan, you're completely right, the things that I learned just watching

people talk about what they're doing was so, so, like enlightening and I've been working

on this stuff for like four years. It's new ideas, new techniques. It's fascinating to realize how much people actually are reading each other's code and trying to figure out what we're doing and that it's nicer when we actually explain it
to each other. So I thought that was really amazing. I hope that when the question of what are we going to, are we going to continue to move
the bytes, I'm not sure we're going to have it run indefinitely, but I do hope that maybe

as needed, maybe we need to have these sorts of knowledge shares because it was quite cool.

Yeah, that was my experience of it. I want to hear from Martin. How did you get involved? What did you think of what was produced? Yeah. I was there in Lisbon and we had this big meeting around the table with Juan and everyone

and we didn't really know what we were doing. We were just collecting this list of requirements, like what should the new protocol do?

One thing I was very happy about to see was that a lot of people were advocating for making this HTTP compatible just because there's, that's one thing I'm excited about because

there's a lot of existing infrastructure around for HTTP, right, with load balancing and caching.

So building something that works both on top of what we have in LibP2P but can also be

applied on top of HTTP was very encouraging for me and I made a proposal for that later

during the working group. Another thing I found really interesting in the discussions that we had in the meetings

was that we tried to come up with a benchmarking solution and we said like, okay, we now have

like BitSwap and we benchmark this against the other protocols. And I mean this is already a little bit problematic because you're not benchmarking a protocol,

you're benchmarking an implementation and that implementation might not be the most

performant implementation of that protocol, but then the interesting thing was, I didn't

have the impression that we could agree on what the data should look like that we are

transferring because this is so much informed by what you're working on in your day-to-day,

what your day-to-day use case of this would be, that everybody had very different ideas

of how our benchmarking data set should look like. Yeah, it was super, did you, I'm curious, the LibP2P team, you guys built the HTTP on

LibP2P. We did, we'll demo it tomorrow. Yeah, was that partly inspired by the group? Were you planning to do that already? It was a mix of both. I think that project is one of the coolest things that I've encountered from the LibP2P
team in a while. Essentially it just solves a problem which is like, if you want to write request response,

can I write it once and have it run on LibP2P and traditional IP systems?

And essentially the solution is if you write an HTTP protocol you can run it on LibP2P
and also over a traditional web client, which is always a big barrier to entry when you

get into the mobile world where you're not going to be running LibP2P. So let that be an advertisement to the talk tomorrow. What time is that? You're asking the hard questions. Oh, okay. I tried. Anyway, yeah, I thought that was really interesting. And also it's interesting because we didn't ultimately ship an HTTP protocol, but that

group for me actually convinced me to be an HTTP believer.
I was one of the holdouts of, we've got to only use LibP2P.

I think it was a really interesting group to talk about real world use cases.
I would love to open up the floor to, raise your hand here if you participated in the
Move the Bytes work group. I love it. Yes. That's so cool. Did anybody, I'm curious, did anybody not attend the meetings but watch them later?
See? Yeah, that is super cool. So this is a panel, but it's sort of a fake panel because we're not trying to just talk

at you for the remaining time. So I would actually love to open up the floor to anyone who is in the group sharing anything
that worked really, that you, so I'll ask the question this way.

What happened in the group that was really valuable to you? Or what happened in the group that looking back maybe we could have done it a little
differently? And for the AV folks in the back, if you don't mind walking around with the microphone to
folks who are speaking so that you can get it onto the recording. Yes. Thumbs up. Cool. Anybody want to get started? I guess just raise your hand and they'll take the microphone over to you.

So one thing that I found really interesting that didn't get too far I think was that you

mentioned it I think just now Martin, the defining different test data sets and then
actually testing them because there's a huge difference between transferring a large video
file or transferring the Linux kernel with like, I don't know, 20,000 small files or
transferring some deeply nested graph data structure.
And yeah, I think that's something that could be taken further to like create some test

data sets and then actually try them on different protocols and see how it fares to kind of

both inform like the high and low points of different approaches and maybe improve upon

that. And I think that's, yeah, like having these measurements, even if it's difficult and you're benchmarking implementations and not protocols and so on, but I still think that that could inform some further developments very much. Yeah. To add to this, it's a lot more complicated than just defining a data set that's like

a video file or a lot of small files, right? You can also have the case where, do you want to benchmark multi-party transfers?

Do you have like a data set which is already replicated pretty well between the two nodes and you just want to do a sync operation? Like there's a lot of different dimensions to this problem. Can I jump in on that? Yeah, for sure. I was going to just ask you what it was like to run those benchmarks. Yeah. And I think I want to point to a sort of broader pattern here, because I think that question really invites part of what I was seeing at the beginning and part of the experience that
I left with working on trying to shepherd the group.
Because I think we got this sense that everybody agrees, yeah, we should have a set of basically data set test fixtures, right? We should have those. And our team put in a lot of effort to try and collect those things and to try and build
really frictionless. My perception was that, okay, maybe this is just sort of an organizational challenge. Everyone needs to put the energy into setting up a place where we can sort of have a common

test ground to actually run some of these simulations or at least establish them. And so the first plan was like, okay, cool, we'll gather the text fixtures and we'll start to define a thing that can actually run and compare these protocols. That turned out to be way too hard and lowering, but more importantly, we actually published
a guide to say, hey, here's how you submit your protocol for testing. Here's a rundown on how that'll work. And that guide was too complicated and nobody took us up on it. And we get the same thing where it's like, yeah, it'd be really nice. I think all of us would agree that it would be awesome if we had a set of data sets we could use to, you know, at Numbers Zero, we use the Linux kernel, we use a couple of other
key things that are just kind of our offhand things. And we try to be really public about the data sets we work with so that others can just easily grab those and compare them and run them in their own. But I think this is a real collective action challenge where it's like, how do you actually
get things done in a group across organizations? Because we all agree it would be really nice to have those things. And Martin, you're totally right. It only gets more complex once you have some of those commonly understood stuff. Sorry, it's like, if we all just agreed, like take this exact one megabyte of data and then
figure out how you're going to transfer it. The other thing we sort of saw in the group was we didn't even have consensus on how to measure stuff. Right. And like, that's, I think, not surprising. And felt like the beginning of a conversation, not the end of one. But it became really difficult for us to advance that conversation because we didn't have a

shared set of information that we're all looking at and agreeing. Like, when we started putting up these slides of like, here's the number of messages that BitSwap sends, everyone was like, well, what's a message anyways? And we and we and that's a really important question. Like, what is the message? Are we talking about bytes? Are we talking about packets? Are we talking about logical messages? Well, then if it's logical, logical message, like that, like, how does that compare apples to apples across protocols? Right. And it felt like we got to the beginning of this really exciting conversation. And then the like, who's going to do this to advance this conversation? Because you need to do real work to sort of get that to happen. Didn't happen. And I don't think that's anybody's fault. I think it's this really inherent challenge in working across organizations and figuring out as Hannah mentioned, like, you know, everybody has their own work stream and their own work to do. And like, how do you find and allocate time for a working group that like is that doesn't directly benefit your department or product? So I just wanted to jump in and say like, that's I completely agree. I want those pictures. I want them to exist very proudly. We tried to make them happen, and sort of fell flat. And I think that's part of why we're having a sort of open conversation. So I just wanted to like, let them.

some of the how could we improve flow into this chat. Hopefully that helps. I'm curious, Brendan, do you, how much of that was affected by the tools, namely TestGround?

Or how much do you think is just inherent to this problem? I think it's inherent to the problem. It's really easy to point fingers to the tools. I definitely definitely got like a lot like, Azmir from our team was just at one point he just turned to me, he's like, dude, I have work to do. You can't just come to me on Mondays and be like, I need this thing by Wednesday. And like, it's just killing my capacity to get anything done for org internally. And yeah, that was like TestGround is hard to set up. And we, at number zero, we switched very quickly to Linux namespaces, because it actually lets you simulate latency. But that's not the challenge. The challenge is like, that's Azmir's full time job at our org,

like someone's full time job is just measuring stuff. And the idea that you would then do that
and be able to figure out how to do that across organizations, it's just like really, really, really hard. So I'm pretty like halfway through, I switched my opinions and just being like, let's establish shared nomenclature and try and figure out how we measure in a way that everyone could just publish their own benchmarks. At least we can have a conversation from there. I'm curious, how do you do you think we can get to there? Like, how do we get from here to there?
Because we don't yet have a, how would we gather a set of common benchmarks and or datasets?

And does this problem eventually, like, I don't know, like, if you go on like the web, there's websites that are like, what is the fastest programming language for like, you know, like, you can find, you know, like the one programming language that was like,
I'm going to optimize for this test, you know, always. Yeah. So yeah. I think Martin has some like good input here, because like, there are other working groups that Martin, that you're a part of that do a lot of this kind of stuff. I don't know,
I'd be interested to hear you too. My sense is just that like, people should publish what they publish and we should all decide how to evaluate that as a community. But self-publishing

is probably the only thing that I see as an option. You have ideas? I have ideas. Benchmarking is hard. It is, especially when you move past the obvious

things. It's really hard to generate good data sets and good scenarios that work. So in a lot

of cases, and what I've seen in my work on the QUIC protocol is that the most meaningful data

you can get is data that's actually obtained in production, where you actually use production

traffic and just measure what happens, because in the end, that's the traffic that you care

about. You don't care about your test data set. And you include all the other factors

that might not contribute to your benchmark. Of course, that makes it a lot harder to design

and iterate quickly on protocols if you first have to deploy it and get real world data.
Yeah. And the LASI project, we do have load testing tools to demo stuff ahead of time,

just sort of assess things ahead of time, but we rely primarily on production metrics of all

the gateway traffic. Go ahead. Should I wait for a mic? Yeah, sorry, thank you. Yeah. So I guess that definitely feels like a good moment to ask my kind of question,

which is the sort of interface between the kind of work you're doing in this working group and

production. There are a lot of things that I've seen today that are super exciting. I love LASI,
I love RAPID. And I definitely see with that ambition to ship this stuff and replace BitSwap

in four months, right? There's a real strong urge to take these experiments and put them

into production and get them used. So LASI is now one of the ways we officially say to people,

this is a good way of getting stuff from the Filecoin network. And I guess my question is,
is how do we correctly say to the rest of the world, this is the stuff that you should use,

or this is an experiment. And when I'm playing around here, I can download and install LASI,

it's much harder for me to get repeat going. But how do we define a perimeter between the
experiments that you're doing here, stuff that ships, and how do we guide people away from the

experiments sometime and go, actually, we're not doing that anymore. That's something that's archived. And how do we signal the stuff that comes out of the working group? I'm sorry,
that was like a million questions. No, it was a good one. Brendan, do you have any thoughts on

that? You guys just had a major shift from one architecture of a product to another. So I'm
curious how you handled that messaging. Yeah, it's a toughie. I think culturally,

we have shied away from 1.0 in a way that hurts our ability to communicate with other folks. It's

funny to see this. We have this culture of our release of our release is 0.4.2. And yet, we're

also at the same time saying, and this is being used in Delta Chat. This has production usage.
And I think that's incongruent and hard. And maybe we could just go back to, well, if it's
being used in prod, call it 1.0 and have the courage to stand behind the spec you shipped,
which is really hard. And I think that's because it's totally fine to have, in my mind, this is

exactly the challenge. We get folks arriving at the outside of our community trying to play and use our technology. And we don't have a meaningful sorting hat for, oh, yeah, go here, not there.
And I've seen numerous efforts to try and do that where you catalog things and write documentation and say, go here, not there. But the truth of the matter is we're dealing in the cutting edge
of what's possible. Repeat represents an experiment in how to do really smart client-side

optimization. Is it a thing you should use tomorrow? I don't know. Talk to Jeropo. But I think giving users a capacity to set for themselves what they should and shouldn't be using,

yeah, I personally think that we should be a little, we should make it a little more culturally
okay for each other to say, no, no, no, you should just, are you releasing this? Maybe we should just put a 1.0 behind this and maybe we should make it more okay to have version two, three, four,
like major versions exist for a reason, right? That'd be my answer. I would add that the one other thing that's interesting is that in the past two years we went from two implementations of IPFS to fairly sizable

implementations, maybe production-ish ready to maybe, what are we, I don't know what the number

is. I don't want to say the number because I'm sure I'll get it too low. But it's at least, we had what, like five showcases this morning and they were all IPFS implementations that were saying, use this. And I think one thing that we're going to probably
figure out over time is messaging around, so which one and why? And hopefully we kind of all

work together and start to recognize that our things are good at certain things and maybe not at others. So, yeah, I don't know. And that will also enable teams to have focus if we can all

get there because I think Kubo has had to be the be-all and end-all for everything and it's sort

of like, you know, the jack of all trades, the master of some. So, yeah. Other thoughts, folks?

By the way, if somebody said something earlier and you want to react to it, you should feel free
to, you are an expert as well. Any other thoughts? Yes, go ahead. I had a more technical question about the working group.

In thinking about all these different protocols that were generated and ideas, I was curious to
stay on trend. Was there any work on ML-generated models? It feels like a lot of the data transfer

protocols are based on kind of like the usage patterns in production, as we were saying. Has there been any thought in that direction or have people thought about it? Just curious.
I have a very short answer to that, but I don't know if anyone else... Does anyone else actually

have like a more in-depth experiment on that? Yeah. The LASI protocol... In LASI, the protocol

that we're working on, we're like, well, we're going to collect a bunch of stats and make them

all exponential decay averages, and then we're going to produce a weighted average for each
peer, and then the way we produce the weighted average, we could just run it through a giant
testing structure and a neural net and... Not a neural net, an ML model until it takes the parameters. I mean, I proposed that to the team, but it was mostly as a joke because... I mean,

it's very... I think partly the reason we didn't get that far, we haven't gotten that far yet, is because in a lot of situations, it still feels like, oh, that's like a hyper-optimization that

we'll get to at the end, but maybe it's not a terrible idea. Yeah. I don't know. I think that ML and protocols is... Maybe there are people who are doing work in it, but it seems

like a relatively new kind of concept. Sort of. The major CDNs have been doing this

for a long time, but there's nothing that I know of in our space that is doing machine learning
at the protocol level. I mean, I guarantee you there's some chat GPT code sneaking in here and
there, but I don't know. I mean, I think it's a really interesting idea. Yeah. Yeah. Cool. Other thoughts? Reflections?

I'm curious... Can we ask folks if we should keep going? Oh, wait. What time is it, actually? We're... Do we have... We got six minutes. So you can either

have six minutes back. I do want to ask one thing, which I would love to hear from someone who either watched videos or attended the group, but is not otherwise... Was not coming from a
standpoint of deeply being engaged with data transfer and what it was like for you to attend
the group and watch. Does anybody fit that description? Curious. Or even midpoint? No?

Okay. You're all experts. You all think about data transfer all the time. Yeah. Well, I kind of love the idea of giving people five more minutes unless anyone has any
clothing thoughts. Yeah. Oh, one in the back. Move the Bytes Working Group gave me hope. Wow. So the natural question for me is, what is next for Move the Bytes Working Group?

That's a great question. Now that we... Can I be totally real? Yeah. Move the Bytes Working Group hurt my soul. It was a lot of work. And so, I mean, it was really nice to... I'm really happy with where

we landed, but I personally feel like we should wait until we know there's another buildup of

stuff to talk about. Towards the end, I was looking at scheduling the next talks and wasn't anybody

with a burning desire to speak. And for the first little while, there were people with burning desires to speak. I just would have people messaging me being like, I have a thing I want to talk about. It'd be awesome to pass the baton to somebody else, have them leave the group if they want to keep going. But from here, I think it would be... If the baton stays with me because nobody else

wants to take it, which has happened in the past, I will just wait until more folks message and then
maybe we'll put up the bat signal and say, okay, maybe we can reconvene the group at some point,
maybe towards the end of the year. That would be my inclination. My impression is the coordination was a good amount of work. You were the one who was doing
work that was definitely not in your work stream otherwise. Yeah. We have one. No? You were going

to volunteer, weren't you? To take it? No, everyone's like, I'm not raising my hand anymore.
I will say, I think there's likely to be a hiatus. We have a lot of really awesome projects that

have sort of partially emerged from this, partially emerged from other trends. But
like, I think it'll be interesting to see where some of these projects go first. I think that

see what the 1.0 version of Iroh, Bao, Super Transfer looks like. See where Lassie ends up.

See what the production repeat looks like. And see if we get some HTTP usage going around

the network. And then maybe figure out from there what we want to do. I also think,

I will say, I think it's a really interesting model. I don't really know if it applies to other
sort of areas of the IPFS system, but like having a bunch of groups of cross orgs talking about a

problem people are all struggling with for a long time and sharing their ideas. Which is what we do at the conference, too, but it's useful to do it kind of apart from that and give people a little
bit more time and a little bit more time to digest and consume and sort of think about things. So,
I don't know, maybe there needs to be the move the find the bytes working group. So, yeah.

Do you have any closing thoughts or should we wrap up and give folks a couple minutes? Let's do the break now. Yes, break now. Yay!

