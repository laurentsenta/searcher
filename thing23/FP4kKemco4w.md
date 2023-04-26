
# DHT Double Hashing Updates & Migration Plan - Yiannis Psaras & Gui

<https://youtube.com/watch?v=FP4kKemco4w>

![image for DHT Double Hashing Updates & Migration Plan - Yiannis Psaras & Gui](/thing23/FP4kKemco4w.jpg)

## Content

Hello everyone, welcome to this very exciting talk. So I'm going to talk about the DHT reader

privacy plan in contrast to what Masip was expecting or wanting. I'm not going to go

into the details of how it works. It has been detailed before in IPFS camp and IPFS thing

last year. So we're just going to go through what it is in a nutshell, the benefits, and

then basically we're going to focus on this second part, which is the migration strategy,
because what we're talking about here is a breaking change to the protocol, and therefore
we need to get some of these details correctly. So with that, I'll start by saying that this

has been called double hashing for quite some time now, since the starting of the design

that was primarily masterminded by Guillaume, which is the other presenter and author of

the presentation. But the double hashing as a term might not be very accurate, so we might

be changing the name a little bit, but generally it's a reader privacy upgrade. So it doesn't

touch the writer, the publisher privacy of the DHT. So in a nutshell, what we have is

traditionally, we have the multi-hash, which simplistically is the hash of the content

that you have. It's got some metadata, but anyway, let's say this is the hash of the
content. Then you have the CAD, which is the multi-hash with some more metadata. And then

basically what this double hashing approach says is that it's going to be the hash of

a sort plus the multi-hash. So this is what is included in the latest spec, and that's

why it ended up being called double hash, because it's another hashing step. If you

want to learn all the details of how you go from the hash of the content to the multi-hash then to the CAD, I definitely recommend watching this module on content addressing in APFS from

the ResNetLab on Tour tutorial. It's great, gives all the details. And then of course

the double hashing part is not included in there because it's very new.
Now the reader privacy upgrade, the approach, is actually three things. The first one is

that it's a CAD agnostic DHT lookup, which is different to what we do today, where the

CAD is in plain sight and everyone can see what CAD is being requested. Then the second

part is the fact that we can request, we can do prefix requests, so not ask for the whole

thing of what we want, but rather a subset of this. And I'm going to explain what this

means in a minute. And then we have provide the record encryption. So the provide the record right now is in kind of plain sight again, but with this approach it's going to

be encrypted using the CAD itself. So what are the benefits? So the plain CAD replay

is not possible. Right now if you're participating in the DHT as a DHT server or if you're sniffing
traffic in one way or the other, you can see which CAD is being requested, you can request
it yourself, and then you can associate the client, the requester, who is requesting the

CAD, with the CAD itself. So what content does this guy get from the network, and also

who is it getting it from. So basically everything is in the open. Now if we go to this approach,

you can still replay the request, but you cannot read the provide the record, because

it's encrypted and therefore you cannot find out what the content that this guy is looking for, what the guy is looking for, basically. The prefix requests that I just mentioned

a couple of slides ago basically return several provide the records, because you're not asking

for the whole thing, you're asking for a part of the prefix of the new hash, of the double

hash, and therefore there might be several provide the records that match this prefix

only. And therefore what you're going to get back is more provide the records than the

one that you're looking for. So through k-anonymity rules, the intermediate DHT server or anyone

else cannot know which thing exactly you're looking for, because they're going to think okay this guy might be looking for one out of these ten things that are matching my request.

This is not making it impossible to figure out, because at some points the hash space might be kind of more sparse, and there could be more just one or a few provide the records

that match, and therefore you don't have to guess out of a hundred, you might have to
guess out of one or two, which is not a hard guess, but still more often than not, and

given that the network is growing in terms of volume of content, it's going to make it
much more difficult. And then we have provide the record encryption, which as I said is

basically getting the provide the record and encrypting it with the CAD itself. So therefore

if you want to decrypt it, you can only decrypt it if you know the CAD itself. And therefore

the intermediate DHT server cannot really associate the client requesting the record

with the CAD, which is the kind of end goal for what we're doing here. Now as I said,

this is a breaking change, and those changes are a little bit painful. They cannot just
be included in a release, because everyone who has not upgraded is going to be excluded
from that point on. So we need to have a migration plan, and we need to get the vast majority

of nodes to upgrade to the new DHT at once. And yeah, a question for the audience. Do

you think, do you know why that might be a good goal for the migration plan?

So no one is left out? Yes and no. Yeah, that's idealistic. All lookups break? No, not all

lookups will break. We're going to cover that, but this is definitely a requirement of the whole plan. Sorry? Privacy? In what sense? Ah, right. Yeah, obviously those that don't

migrate, they're not going to be private, because that's how things work now. Yeah, the answer is due to security. So if you have a small fraction of nodes upgrading to the
new scheme, then it's much easier to see build that network, and create fake identities,

and kind of take over the new network before the majority of nodes have upgraded. Yeah.

So that's the goal, and the challenge is basically we need to make the date or the point in time

that everyone migrates and upgrades to the new release, to the new scheme, common knowledge

across everyone in the network. Which is of course difficult to do, because no one controls
the software, and it's difficult to do that at once. So that's where the migration plan

is focusing on, and the summary of it is that the team, the IP Stewards team and the Probably

team came up with, with help from others of course, is to use an IPNS key that is going

to be hard-coded in one of the Kubo releases, the one that is going to basically roll out
the update, and orchestrate the nodes through this Kubo release to be requesting this key

every so often. So as a clarification, everything in the next couple of slides that is in red

fonts is things we don't know, and parts of the migration plan that we need to get in place. So every so often, it could be every day or every week or every hour, we don't
know, it's just a detail that we need to kind of iron out. So that's how, if every day,

if there is an IPNS key that includes, simplistically could include a Boolean like true or false

or a date, then everyone that is pulling this IPNS record daily, and checking the fields

that we want them to check, they're going to know, this is the date or this has turned

to true now, so it should go and upgrade. The migration date of course, it needs to

now be setting the date in this IPNS key or the Boolean if it's true or false to the right

value needs to happen when enough peers have upgraded to this release. So this will need

to monitor, which we do through our crawler, and we are going to be able to see when enough

peers, I don't know what enough might mean, 50% of the network, 70, 30, I don't know,
we'll have to figure that out. Now, on that migration date, we have different peers and

kind of peer categories that need to follow their migration strategy. And by this, yeah,

by every peer, I mean DHC clients, DHC servers, content providers, all these different kind

of network players, which I'm going to mention in a minute and go in detail in a minute.

A couple of other parameters is the switch date. So this is when we're going to see that

enough peers, as I said, have upgraded, and we think that we are now ready to do the switch,

that the date is going to be fixed. And the transition period, which is something quite important, the period of time when peers in the network will have the opportunity to use

both DHCs. So it's going to be a little bit risky to just deprecate the old one just right

at once, because I don't think, we don't think that everyone is going to be in front of their
laptop to do the upgrade at a specific moment. So there needs to be some period there. And
the exact period, yeah, it's relatively tricky to set, but we need to get to that.

Now during the transition period, and this is where we get into each DHC, key DHC player,
we have the bootstrappers that are, of course, key part of the DHC network still, maybe not
in the future, but they are now. They should be providing peer IDs for both the new and
the old DHC, so that a new DHC client or server getting into the network and wanting to connect

to others, they know that they have peers to connect to. So that's the easy part. These
are a few. They're currently controlled by APL, so it's not easy to set that there.

Now the content providers, especially the big ones, by default, they should switch to
the new DHC on that transition date. They do have the option to stay in the old DHC

and keep on providing their content, but they will have to do so kind of manually to touch

the... either choose the right option in the APNS key or their own code base that they're

running on their machines. Now you can understand that there is one system that is running now,

and there is going to be a new one. If you're providing content and you switch to the new one and you only provide there, and your clients have not upgraded, so they're on the old system,

operating on the old system, so to speak, your content is going to be unavailable, unreachable,
which is not great. Yeah? How are you going to measure how many nodes have migrated, how many clients are operating

on the new system so you can figure out when you can make the later transitions and deprecations? So I'll get to that in a minute. The DHC clients are, I think, easy to have... it's difficult

to have an idea of how many DHC clients have upgraded, because this is not something we

can crawl. This is not something that we're monitoring. But this is easier with clients,
and this brings me to the other point. They can easily make two requests, one to the new
DHC and one to the old DHC, to cover the case where some content provider might not have

upgraded yet. Of course, this adds twice the number of requests to the two networks now,

but it's not the end of the world. It will add some more traffic to the network, but

they can prioritize to ask the new DHC first, not have two requests going out at once, so

that if the content providers, which should hopefully upgrade right there on the switch

date, then they're going to get their content right from the first request and not have to go to the second.

And then the DHC servers, which is the intermediate nodes in the network that respond to requests

for provider records, they might have to run both DHCs for a period of time until the old

DHC is deprecated. Of course, this adds more requirements to the DHC servers, because they

will need to provide the records for the old and the new DHC, and serve requests for both,

so the traffic is going to be increased as well there.

So this is it for the key DHC players. Any questions up to this point? Okay, no? So everything

is understood. Excellent. Right. So the timeline. Where we are today and where we're going to

get to. Okay. Yeah, so the spec is finalized, or it's very close to being finalized, so

we expect only minor updates if there are any. Then the first draft of the migration

plan is what I'm presenting right now, and we also have a discussion forum post which

went out on the day before yesterday. And I'll get to that in a minute. Then at the

end of this month, we want to receive feedback from the community in the discussion forum,

from you here, and we're going to, of course, meet with the team with the opportunity that
we're all here together, and make more progress as to when things are going to be ready. So

the announcement is going to be in the form of a blog post and relevant comms to the community.

Then the next steps, which are subject to change, and that's why they're pretty broad

dated there, is finishing the implementation and the testing, which is going to be hopefully

within Q2 of this year. And then, most likely towards the beginning of Q3 of this year,

we're going to have a migration plan finalized if things need to change. We're going to have

the Kubo release that is going to go out, and it's going to include both of the two
new DHDs, the refactored DHD code, so to speak. And then at some point, when we see that enough

peers have upgraded, there is going to be the migration triggered, and then it's going to be completed after the transition period has finished. So, yeah, that's the timeline

to migration. Admittedly, we don't have set dates yet, but look out for updates in this

discussion forum blog post, sorry, in this discussion forum post, which is going to serve
basically as a point of reference for anything that we do. So, we're going to be posting
updates there. As I said, there might be a blog post, but we're also going to post it here. So, if you're interested, just follow that. And how to reach out to the team is

the usual suspects through the IPFS Discord server. There is a libptp-privacy channel

there, but you can also reach out to libptp-implementers and ProbeLab, and most of these channels are

also findable in the file going slack. There is a working group session scheduled on this

topic to make some more progress and hopefully get to define the parameters that I put in

red the slides before, the transition period, how often the IP and S keys should be fetched

by all of the nodes in the network, and all those unknowns right now. Hopefully, we can

get some answers. So, if you're interested, I think that is scheduled for Wednesday, so

in three days from now. If you're interested, just reach out to me and we'll be in one of
the rooms on this floor. So, yeah, as pointers now, the spec is a source of truth, as I mentioned previously. If you

want to know the details, just get to that. There has been a lot of work done on this it's IP IP 373. There has been lots of discussion there. You see lots of comments, lots of commits, lots of revisions.

So, if you want all the detail, that's the place to go. And Guillem has given some very nice talks, as I said, in IPFSCamp and IPFSThing last year.

So, these might contain a bit of out-of-date info, but mostly, if you're interested in the basics of how things work and why they work this way, I think they're still valid.

Yeah, and that's it. Any questions and what should we explain better?

This might be in the details, so feel free to reject this question. But could you speak a little more to the threat model, maybe, of the content privacy?

Because I'm thinking, for example, if somebody knew up front certain content IDs that they wanted to track or block, it seems like they could trivially decrypt the provider records.

Yeah. So, the main threat model here is that we don't want anyone in the network to be able to observe traffic, CIDs, who is requesting what, and who is serving the thing that has been requested.

So, that's why it's called reader privacy. Indeed, if someone has got the CID, then things are becoming a lot easier. But in the general case, we want to primarily avoid those that sniff traffic from the network and can build dictionaries of what happens at which point.

Any other input from Guillem?
Yeah, so I would say the privacy of the request highly depends on the secrecy of the CID, which means that if I'm sharing with you my holiday pictures and they are not publicly addressed on the web, then it's going to be totally private.

But if you're trying to access to, I don't know, a Wikipedia page that is very popular, then DHT servers that know this very specific CID will know that you access it.

So, for very public data, it doesn't give much privacy, but for, let's say, more private data, it is able to provide privacy.

What about the flip side? What solutions can you think of in terms of blocking specific CIDs, for example? Because if all the CIDs are encrypted, if you want to remove some unsavory CID, how would we go about doing that with the reader privacy?

So, someone like, you'd have to know the CID, right? Kind of similar to today, you have to know the CID of some bad content that you might not want to serve from the network.

So, as soon as you know that through some out-of-bound means, then you can do the same as you do today, I think.

That's right. I guess what I was getting at is getting to know that CID is now more difficult because if there is a specific circle in the system that distribute content that we don't want them to, as long as they don't leak the CIDs themselves, we would never know what those CIDs that are being looked up, right?

From an outside perspective. Yeah, but you cannot read. Yeah, correct. But you cannot, you or no one else, unless they know the CID, can decrypt or see what's in the content.

So, everything is encrypted. So, that's why, yeah, so right now, if you have a CID that might fall into the hands of someone that should not look at it, they can look at it. Then, this is not going to be possible unless you explicitly want them to know what the CID is.

Yeah, basically, I think if you have a blacklist of second hash to block, then it's easy to block, and otherwise, you cannot block the content routing of something you don't know what it is.

Yeah. In terms of the migration plan, have you thought, in terms of, has there been any consideration, now that we have this pluggable content routing layer, that instead of running the old DHT version, it would fall back towards IP and I, for example, so you don't have to have this duplicate logic and maintain that and just do a clear upgrade?

Yeah, so the fact that IP and I now exist, there is a fallback option, and therefore, the transition period running both DHTs might not even be needed. That's what you're saying?

Yeah. That's correct. We're taking that into account, that CID.contact is now part of Kubo, and I guess that it's not going to be removed, I don't know when or if ever, but by the point we get there, which is going to be in a few months, it should still be there. So yeah, we're taking that into account. It might help.

And maybe a follow-up, for testing, what's the plan for testing if, yeah, I guess just testing that it'll work in production before rolling out 100%?

Yeah, so what can happen is that we can, as I said, there is going to be a new Kubo release out, but it's not going to be triggered, the migration is not going to be triggered directly. So during that point, we will have the option to test it by, you know, kind of on purpose migrating our own private cluster machines to interact with the public network at the same time.

Do all the double hashing logic. So, of course, we're going to have tests beforehand, but they're going to be kind of live tests when our machines have migrated, but not everyone else's.

With any kind of privacy improvement, like one of the challenges is how do you explain the limitations of it? Yeah. And the benefits of it to, well, you have kind of a lay audience, but also you have an audience, I think, that we support at the foundation, a bunch of very privacy conscious sort of use cases.

And I think that they have a very good understanding of kind of a web to threat model and less in a decentralized way, just through experience. So I'm sort of trying to work out how I would explain the change here.

It strikes me that one of the ways we could explain it is by talking about the steps that have been taken to protect DNS lookups. Right. So there's been a whole sort of debate about DOH and like how, you know, how DNS lookups are actually plain text and have been for a while and the movement to that.

Is that a useful analogy? Can you think of another one if I'm explaining it not to a lay person so much, but someone who is used to a traditional web security model or internet security model?

Maybe an abstraction we could have concerning this is that we would use a different content identifier for content routing and for data transfer, which means that now we have the CID, which is kind of the master content identifier.

And now we'll hash it with a specific hash, which will give us the counter routing identifier. But then once you know where the content is located, you need to derive a new content identifier.

So for instance, you hash the CID with another salt and then you will request the content using this other name. And so it means that even if you were, if you observe, if you sniff some traffic in the content routing system, you wouldn't get the data transfer name for this specific file and you wouldn't be able to fetch it.

So that's like more changing around the namespace. If that answers your question.

Does this answer your question or is it not? It gives you some more fuel. Yeah. So, yeah, an analogy that just comes to mind now, so I don't know if even if it's too accurate, could be, you know, we have the DNS system where you're asking for some URL through the browser, but then you have services such as the ones that are making the URL shorter.

So basically that's an identifier of something that I cannot understand what it is. I might have an intuition if I've used it before that, you know, three letters stand for, I don't know, some website normally.

But otherwise I'm asking for something that I don't know. And therefore, if everyone else on the path sniffs my traffic, of course, the DNS server will know what to do, right? But everyone else that is just sniffing my traffic from, say, my Wi-Fi access point will not know now what I'm requesting.

I think probably what we have to do is to spell out some threat models that people are familiar with and go, okay, how does this change what the capabilities of an attacker have?

Definitely. Because if someone's tracking your… yeah.
Yeah, definitely. We're going to have very clear communication, yeah, through written, but both, you know, in some form of video recording of, you know, what can be done and what cannot be done from now on.

And we've got a threat model section on the spec.

Just to sort of follow up on Danny's question and the earlier question about threat models, to make sure I'm understanding this correctly when I communicate with some of those project partners and activist organizations.

NSA style mass snooping has now become very difficult with this new change. But if there's a particular, like, high profile CID that's known to the ecosystem, this does not prevent somebody from figuring out exactly who is requesting that CID.

And they could even, if there was a list of those resources, still do a mass surveillance program on that full list to see who is accessing those resources?

Yeah, correct. Okay. As long as you have the HD server located at the right places in the XOR space.

I mean, there are optimizations there as well that can be done in the future, but I think they will be based on this base model, like, you know, changing the sort and doing things like that.

I think our optimizations that we can think of as a second step to avoid that case as well.

But yeah, what you're saying is basically valid.

Would those optimizations or whatever future change, would they also require an expensive upgrade like this?

I don't think so. Thank you very much. If you're interested, reach out and let's have a session separately later on this week.

Okay.

