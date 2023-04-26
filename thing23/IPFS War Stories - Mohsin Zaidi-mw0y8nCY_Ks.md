
# IPFS War Stories - Mohsin Zaidi

<https://youtube.com/watch?v=mw0y8nCY_Ks>

![image for IPFS War Stories - Mohsin Zaidi](./IPFS War Stories - Mohsin Zaidi-mw0y8nCY_Ks.jpg)

## Content

Good afternoon everyone. My name is Mohsen. I'm one of the maintainers of the Ceramic

Network. I've met a bunch of you in the past. It's nice to see everyone again. In this talk

I wanted to present some of our struggles using IPFS for our production systems. Struggles.

We've used vanilla IPFS, JS, and Kubo. And that's important because we've now got a larger team, but

before we didn't have the bandwidth to piece together components that would work the best. So we relied more on vanilla components so that we could focus on building our own protocols.
Some of this mix and match that is now possible wasn't available, so we can do more things now

that we couldn't in the past. Some of the problems I'm going to discuss have been fixed, either by us

or for us, with a lot of help and support from Protocol Labs. So I see Steve here,
I don't know if Lidl's here, Gus is here. People who've helped us a lot, so it's very appreciated. Some of these things could be things we've just been doing wrong, and we'd love feedback. Lastly, I'm likely going to finish ahead of time, so anyone else has more stories,

feel free to share at the end. All right, so how does Ceramic use IPFS? Ceramic is an advanced streaming protocol with streams
of hash-linked commits that are stored in IPFS. These are IPLD commits, IPLD data structures.

Ceramic also provides additional primitives for identity data composability and for data modeling

across nodes and applications. Stream tips are synchronized by sending requests over PubSub,

and the commits that are discovered via that mechanism are synchronized using Ritzwap. Ceramic also posts these new tips to Ethereum, bunches them together in a Merkle tree,

and posts it to Ethereum, posts the root CID to Ethereum, posts those commits to IPFS,
and also propagates them through PubSub to the rest of the Ceramic network. This is critical for Ceramic because it is our source of eventual consistency. So these were some of the key challenges we faced running IPFS in production. I'm sure a lot of you could identify with these. These are the things we struggled with the most. I'm going to cover each
of them briefly. The graph on top is the memory utilization for when we were using JS-IPFS

in production. As you can see, it has a very pretty sawtooth pattern of how it would

go up to about 100% and then go back to zero because it would crash. So, the silver lining.

And that would prevent our anchoring process from completing often and would cause a lot of work to

be repeated. We ended up pre-anchoring the whole thing. We had to do a lot of work to get it to

work. We ended up preemptively rebooting. So we would reboot IPFS, rush to do the anchoring,

and then once anchoring was done, we would push everything and then we would reboot it again. So we ended up doing that to get anchoring to work. The bottom graph is for Kubo. Due to API calls,

number of API calls, or other network conditions, it would also have an unbounded number of
go routines, which would result in out of memory crashes. And Gus was very helpful. We had a long

discussion about this. We did add, we can and we have added rate limits. But ultimately, the best

solution would be to have back pressure because you can't really enforce rate limits. You can't
hard code them because something else could change that would make your hard coded limit not correct.
Sometimes there are timeouts without any other reason, like the memory and CPU will be fine,

it'll still time out for data we know is available on the node. It will not respond with it. It just
refuses to respond with it. These are more probably specific to us. We're heavy users of the POP sub

network. It's hard to scale and be highly available when you have pet servers, which is how POP sub

works with IPFS nodes. Each node has its own identity and it's not interchangeable with other nodes. You can't just swap an identity. You can't share identity. That makes it hard to spin up more

nodes behind a load balancer and treat it like a cluster that has an identity. There is an IPFS

cluster operator, but that's not what it's meant for. The other thing you can't do is you can't
upgrade a node without downtime. Every time you want to upgrade one of the nodes, it has to go
down so it'll release the repo lock and then you upgrade it and bring it back up. There's always some amount of downtime. This is another problem we've faced a lot, POP sub connectivity. I know

other people have as well. With JSIPFS, nodes would suddenly just stop receiving POP sub traffic

and we wouldn't know why if they went too long without receiving a message. To solve that, we set
up keepalive lambdas in AWS. Then we just added code to ceramic to just send keepalives every few

minutes within a certain amount of time so that there was some traffic going on POP sub so that it wouldn't die. That helped a bit. It did not eliminate the issue. For that, we wrote more

automation that would just restart IPFS if there were no POP sub messages received for 30 minutes.
With JSIPFS, well, with that, it definitely helped. Kubo does better, but it still has

times when it just stops receiving POP sub traffic. It happened recently. It's very

infrequent with Kubo, but it does still happen. This was terribly painful, migrating from JSIPFS to Kubo. We had to write code to migrate the

pin store because JSIPFS had the S3 plugin for the pin store. Kubo did not. Again, some of this

is stuff we could have built. We just were in the middle of migrating. We didn't have the time to do that. We ended up spending a lot of time on that. We also had to write code to migrate the

pin store. We ended up spending time anywhere migrating the pin store. The block store naming

format was inconsistent between JSIPFS and Kubo. It was like even on the same repo version, it was

inconsistent. We were terrified we would have to migrate the entire block store, which is probably a terabyte of information in each of our environments. Thankfully, we moved to repo version 12, which was consistent on Kubo and JSIPFS,
so we didn't have to touch the block store. It had the S3 plugin with some tweaks, so we got to work. Secure web sockets were not supported in Kubo out of the box. All our multi-addresses were secure web sockets. We had to change all of our multi-addresses,
ask all of our partners to change their multi-addresses so that we could all use each other's nodes. It is now supported out of the box in Kubo, but it wasn't back then.

Debugging. Yes. I have looked at Kubo logs many times. I've just looked at the first page and

then closed the tab because it's hard to tell what matters. Logs can be verbose. They can include

errors from other nodes, which has been really weird. There's no easy way to identify sources

of memory growth, no easy way to identify sources of delays. Some takeaways, like building production

databases over vanilla IPFS can get tricky. Definitely dragons, not the cute ones. Some

more problems can definitely now be solved by putting together components in a better way that

wasn't possible before, and also building new components over existing ones, both of which

are avenues we're exploring. So putting together a better combination of packages, and there's been

some great presentations today just about that. Some great new protocols being built that we can
now pick from. And it's great to see that the community is aware of all these shortcomings.

Nobody hides these problems. Everyone's forthcoming about them, and a lot of people working to fix them, which is great. Now for some fun random weirdness, which I don't know

how many people would have seen these things. My very favorite is the one at the top, is that every
other dagget RPC call would fail. Like every other call, every second call would fail. And that was

because the IPFS HTTP client defaulted to using keepalive true for the node connection pool,

which meant it used reused connections, but Kubo did not support connection reuse. It would kill the connection as soon as the first one completed, and the second one would try... So node would try to reuse the connection, and this was also a bit of a weird interaction with
the AWS load balancer, and the IPFS HTTP client would make another dagget call. That would fail

because Kubo had killed the connection from its side, so we had to set keepalive false to
to not kill every other call. That was a very interesting investigation. IPFS dagget can stall for hours and successfully resolve some connection limit problem, we're not

sure. The third one's also very interesting. You're connected to a peer that you know has the

data you want, but it will time out trying to get that data. And then you give up and you kill that

IPFS CLI call, and then you run it again and it works right away. And we've just seen this pattern happen so many times, so it's not more like happenstance. It's just... This is... What if you turn off the timeouts for like seven minutes, and it still times out, and then comes back to me? This is our newest struggle. Some of this is potentially the way Ceramic interacts with IPFS,

so it's not like an IPFS-only thing. A large number of duplicate messages have been propagating

through the Ceramic network, and we've adjusted the seen message cache TTL, so there's a cache

inside pops up for messages that have been seen within a certain window of time. We've adjusted the TTL, so with Protocol Labs' help, we made the TTL configurable, so we were able to adjust it

to its larger window we set now. And we also found a problem with the cache implementation itself.

It was a first seen cache, so it would see a message once, and then even if it saw it a bunch
of times after, it would take the time it saw it first as the start of its window for the TTL.

That way, you would see a lot more duplicates get through, so we re-implemented that to be
a last seen cache, so now it would be at least X amount of time, which is configurable from the

last time you saw the message before it was let through the cache again. So that helped.
Now we see fewer duplicates getting propagated, but now our nodes are still getting somewhat

overwhelmed, like receiving messages that get validated and parsed and then rejected because

they didn't make it through the cache. But the CPU utilization is at like 50-60% just trying to reject those messages. But we're looking at this from multiple angles, from the ceramic side

and the PubSub side. At least we can think of good signatures when you're rejecting. Yeah, so if anyone else has any stories to share, you're welcome to. I think I finished way ahead

of time, but that was the intention. Cool, yeah, so this might be a good session for comments more than questions, or maybe answers

from the community. I know a feeling that we've had at Fission sometimes is like, clearly we're just holding the IPFS wrong, and everybody else here must be doing this

better than we are. So yeah, anybody have anything they want to share? Answers, questions, thoughts?

JS IPFS to Kubo. Why did you start with JS IPFS?
Initially, Ceramic was meant to run in browsers, and then it seemed like a natural transition from

it's in JS, run in the browser, and now we switch to Node.js, run it on the server. It's no longer supported in the browser, but JS IPFS stayed. And then we used it for a while, I think a couple

of years, until like mid-2022, last year is when we switched to Kubo. So that was the reason. It

was more historical that we picked JS IPFS as the thing.
Awesome, thank you. I'm interested in your kind of interface with folks in the rest of the IPFS cinematic universe.

So when you were trying to fix this stuff, right? So you mentioned that you worked with Protocol
Labs to fix some of these problems. There's clearly a few things here where like, it shouldn't be down

to you, they're fixes that would make the whole network better. Did you find like it was useful

to be raising bugs, raising issues in the GitHub repo? How did you decide when it was time to like

pick up the flashing red telephone and call someone at Protocol Labs and say, this is fixed? And
is there any ways that you would like that relationship to improve for you and for other

people? Because of course, some of these things just don't scale, right? You can't. And yeah,
I guess those are my three, four questions. Yeah, I mean, we found that our interface with... So we started out with GitHub issues

and communicating with the team on Slack. And it's always been super responsive,

mostly been super responsive. And when it's not been, it's not been like a, it's been understandable. There's never been like a inordinate amount of delay getting back to us. There's always been a way to kind of escalate if we needed to. And kind of GitHub, Slack,

maybe other ways sometimes. But it's always been very responsive, very detailed responses,

timely. We've had to do make fixes ourselves because if it was something Protocol Labs

knew and accepted as a problem and requested for like contributions because they didn't have
like the resources. And I mean, our experience has been good. Running into those problems is

never fun. And solving some of those problems is very much not fun when you don't really know
what's going on. These are complicated systems. But I feel like we've generally had... It's,

I know it's, it's difficult. It's rough for us. So it's easy to feel angry.

And we've felt upset a lot of times, but that's something we worked through.
Has the war been won yet? It's a good question. A few battles have been won. The war is not won yet. No. Yeah.

You've done some trenches. Yeah. All the run. Also, did you get an opportunity to work with Helia? We have not. I just heard from Daniel before this talk that he's had a great experience with Helia.

Give it another shot. Yeah. I was telling him it'd be, it's kind of hilarious, a little sob, to go from JS to

Kubo and then back to Helia. Full circle. Yep. Which it's very interesting. I'm definitely
going to give it a shot. If it works well, we could switch to Helia. We need some time
before we implement. We've gotten to the stage where our scale requires implementation of some
custom protocols. And at this point, it will take a little bit of time for us to get there.

In the meantime, to address some of the things we're struggling with, if there are ways to

swap out some components or maybe even Kubo itself with Helia, if that helps get us through
the phase we are in right now, that would be perfect. So definitely going to take a look at Helia, some of the other protocols that were brought up today. Gold card mirror sounds
very interesting. So we'll definitely take a look at some of these. Thanks a lot for speaking to us. Thanks a lot for sharing with all that you have learned and if you could go back and do things.
Or are you starting from scratch today? Are there any recommendations that you have? Or said another way, how would you want to evolve things in the future with the battle scars and newest learnings that you have? And totally understandable if you don't have an answer for that yet. It's something you have to think about. But if there are things that come up to the top of your head, love to hear them. Yeah. I mean, off the top of my head, I think I loved the, like to go back in time a little,
I loved the transition from Go IPFS to Kubo. I understand why Protocol Labs did it. It was
becoming the IPFS implementation. Now it is an IPFS implementation, not like everything for

everyone, which it was becoming. Now, since then, it has become easier to mix and match

components. So now people, I would say, should look at picking like the best of class of what

they need versus being like, oh, Kubo should do everything for me or Helios should do everything
for me. So paying more attention to individual components and how to like put them together.

Hopefully that keeps getting easier. I know it is easier now than it was in the past. If it keeps getting easier, that would be one recommendation would be to pick what you need versus just picking. Not every team can do that, though. We're probably in the same boat.

We're probably in a position where we can now if we wanted to. But the easier it gets, the easier smaller teams can just be like, oh, I want these six packages. That's all I need. And I'll put them together. The easier it gets, the easier it will be for smaller teams to do that. So that's one of the things I would say. So Danny kind of asked this and you answered of like, sort of like how was connecting in and everything else. So Fission and James already kind of said it where, you know,
we spent a long time. and again, it was go IPFS was in a lot clunkier stake. You know, we found every release and continues to be better, but we were sort of wandering around in the wilderness for six months, nine months before we pierced the veil of PL specifically cake and why we're like, oh, yeah, totally. Yeah. Yeah, like ignore those public channels where everyone's like, why the fuck is that?

I mean, all this stuff doesn't work. Here's the three people to talk to. So I wanted to just like add that for the record. I think there's actually still not good onboarding for IPS operators that the default experiences. I threw it up. I ran it. What that face basically and a bunch of us have now been like, oh, you know, we know where the bodies are buried.

Don't touch that port port. What do you mean? Everybody turns off garbage collection in production, right again? Laughter from everyone, right? But but that's not reflected in public documentation or best practices. So I really want to take this as a call for great.

We should reboot IPS operators. We should continue to make it as an on ramp for folks, and we should continue to work together in looking for saying like, hey, we're about to put two engineers on something for six months.

Before we do this, does someone else want to help? And I see that very like collaboratively sort of thing, right? Like we're actually still sort of in a situation where we're like, well, thank God Gus is running all of those test runners.

And in part, I now understand why we haven't been able to help more. So that was really helpful for me. So, you know, I also picked in what you were saying just recently. So this is the question. Part of my comment was was that you're like, oh, we're about to start doing some, you know, at our scale and for our use case.

We now need to do some custom stuff. So we're gonna like look around for sort of things like that. And I that's the same sort of thing. I'd love for us to be able to be like, like, like actually tell everyone in the community.

Hey, if you're about to embark on that six months to engineer sort of thing. Let's talk to see one who else wants to throw money, resources, requirements, what's whatever into the pot. This is always hard. Because ultimately, it's like, no, we need we need to move quickly for our own resources or like whatever kind of thing like that.

So maybe just like, again, sort of like a public call for like, hey, awesome. At the very least, it would be amazing for you to publish, like, here's what we're trying to do. And we ended up picking up x whenever whatever you decide to do so that we can all learn slash have an opportunity to say like, Oh, yeah, that's great. We'll totally contribute in in x way. Does that make sense?

It does. And it's funny you say that there actually is a write up off what we're planning. Nathaniel has written up it's, it's been shared in some places, I think with the number zero team, or some people has been shared with it had not been broadcasted. But that's a great idea. We'll definitely send it out to more people. So definitely, that's a that's a good point. And the other thing I was going to say is, thank you for starting the IPFS operators challenge. I know you and Brooke did that.

It's been kind of like a godsend because it's a way to pierce the veil. It's not just one issue among I don't know how many issues in Cuba now 1000 1200. It's just it's not one issue among 1200. Now it's like, there's an issue and there's some escalation path at least. So both great points. Thank you.

The other point I wanted to make slightly more generally, so we had the specific instances of rather than getting any kind of back pressure. If you sent too many requests at Kubo, it just fell over and then you rebooted it and now you're back in a healthier state.

And when gossip sub, the slowest node on the network is the one that is verifying too many signatures and suddenly has a queue that is longer than the timeout time. So every message that verifies is out of the timeout window because that's how long the queue is.

So you can have a channel that's got 30 real data center servers on it that is dealing with this flood of traffic because a raspberry pie can't keep up with the channel and just keeps throwing the messages back in five minutes after they happened.

Thinking about ways to make stuff gracefully degrade, I think, would be very helpful of maybe if the raspberry pie can't keep up, it would be nice if the raspberry pie had a bad time.

But the racks of servers in data centers were still happy. And I think there's just a few places in Kubo where taking this approach of, okay, how can some, I mean, I know a lot of people define a distributed system as when a computer you didn't know existed can break your code.

But there's definitely these times where we weren't even being deliberately attacked. It was just a slow node brought the whole channel down. Nonetheless, how do you protect a PubSub channel from someone who's deliberately trying to spam the thing?

Thank you, everyone.

