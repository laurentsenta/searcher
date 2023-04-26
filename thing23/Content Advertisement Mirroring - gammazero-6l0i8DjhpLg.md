
# Content Advertisement Mirroring - gammazero

<https://youtube.com/watch?v=6l0i8DjhpLg>

![image for Content Advertisement Mirroring - gammazero](./Content Advertisement Mirroring - gammazero-6l0i8DjhpLg.jpg)

## Content

All right, so as the introduction said, I'm here to talk about content advertisement mirroring.

So this is a specific type of mirroring that mirrors the content advertisements that are
used in indexing. So let's talk about what that means. So content advertisement mirroring

refers to storing content that advertises all of the multi-hash data that we index.

So multi-hash data gets indexed when it is advertised by an advertisement publisher.

They publish information that says, here's all the things that are available, and go ahead and index it. So what we want to do is we want to collect that data and be able

to use it for later. And we're going to be collecting that in a special type of mirroring that is based on CAR files. But we'll talk about that a bit later. First, some background into this. So as many of you probably already know, the role of
an indexer lets content providers advertise where content's available from. And it lets
retrieval clients search for this content by content ID. In order to do this, indexers

have to take in huge amounts of data from all the publishers that are publishing the advertisement of the content. And the indexer takes this data, transforms it into the indexes

that allow the clients to do their lookup. So indexers can be deployed all over the world.

One of the reasons we might want to deploy them in different places is to be able to have reasonable local indexing available. So I might not be able to reach indexers in

another country, but if I have an indexer deployment in my country, then I can get to it relatively quickly. My clients can learn where content is. But in having multiple deployments, what that means is each of these indexer deployments

is going to have to be able to pull in all of this data redundantly from all of the different
publishers. So one deployment pulls everything in from all of the publishers in the world,

and as does any other installation. So this can actually be quite expensive to do. So

if I have an installation, it's been running for months, and I want to start up another indexer installation, it has to pull in all of the data again. Or maybe we have to have

some sort of a way of transferring the data that we've already pulled into the new indexer,
the new indexer installation. Because otherwise, all this initial processing can take up to

weeks to get caught up. So the primary job of an indexer is to be

able to answer queries about where data is located. Since indexers are consuming all

of this content from all these publishers, they can actually play a secondary role, and
that's to act as a concentrator of all this information. So all of this information is
spread out amongst publishers all over the world. If the indexers or an indexer installation
is pulling this in, being able to concentrate this information in one place is actually
quite useful. One of the more obvious uses is to be able to use that to bootstrap a new

indexer installation. What that means is that we can now, instead of having to pull information

from all of the different publishers to get caught up, we can then take it in a bulk transfer

from an indexer that's already caught up and transfer that data to the new installation
and being able to bootstrap it that way. So this way you can use whatever is the least

expensive means of bulk data transfer. That may depend on your infrastructure, whether
it's an S3 sync or transferring a truck full of optical media. But whatever is least expensive

is probably going to be a lot faster and less expensive than having to take the weeks that
it would take to pull everything in from all of the publishers of content advertisement
data. So why is this useful? Well, some of the reasons already stated, but it's useful

for reducing the workload that's placed on the advertisement publishers. All of the publishers,
they don't get any sort of compensation for advertising their content. They advertise it because they want clients to be aware of where content's available. So if a lot of

indexers are hitting all of the publishers for data repeatedly, this can be quite a burden

on them. So this helps them out a lot with not burdening them. Reduces the bandwidth
cost within an indexer deployment. If you have redundant indexers and they're indexing
the same content or ingesting the same content, you can use a mirror to not have to re-ingest
the same content. Reduce the time to bring new indexers up. That's what we already talked
about. And making content, advertisement content available to other indexer deployments. And

this is something we'll talk about more, but being able, can we share this not just with our own deployments, but with the world and allow them to bring up indexers, allow any
party to bring up indexers more easily. So as far as talking about why it's useful for

bringing indexers up quickly, it's much faster. So how much faster? We're seeing anywhere

from four times to 20 times faster. And that's just for the data transfer alone. So in other

words, when an indexer uses a mirror, it's able to pull in the data directly from the
content, advertisement data directly from a mirror, as opposed to from all the original
publishers. So that's much faster because assuming that the mirror is reasonably fast and network local to the indexers that are pulling the data in. But it's additionally
faster because you don't get interruptions in the data stream. We've seen that a number of content advertisement publishers, they tend to rate limit their data or there's a

network instabilities. And that can cause a lot of restarts. Maybe you only get 50 to
a hundred advertisements at a time on a chain that might be a hundred thousand or even a

few million advertisements long. And each advertisement can have its own chain of data
associated with it. So that could take huge amounts of time if there's any sort of interruptions
in data stream. A mirror eliminates those because now you're transferring from something local to you and you don't have any external limitations applied. So given the amount of

speed up a month to a week at an average of four times speed up, that's pretty significant.

So we've already mentioned some of the additional benefits. Less expensive than depending on

your external bandwidth costs. You can have an internal transfer from bulk data. Less

burden on publishers. Useful for bootstrapping installations. And the last point is more

important one that we'll talk about is it's an alternate source of content advertisement data if shared to the public. And that's something that we don't have a clear solution for how
we want to do. But that's where we will be able to provide a way of helping others bootstrap

indexers. So all of this bulk data transfer, we're terming it a content advertisement mirror.

So it's a specific type of mirror, a content advertisement type of mirror that is. And
it's in its current form it's based on a collection of car files where each car file represents
an advertisement and that advertisement's associated multi-hash data. So right now the

ability to read and write these car files is built into our indexer implementation,
but it could certainly become an independent service. Or we could just rely on the availability
of car files as the means of interoperability. So as we've talked about the advertisement

itself, there would be a chain of advertisements. This slide shows a single advertisement and its chain of multi-hash data chunks. So this one advertisement and its associated data

is what makes up one car file. So we'll have any number of car files that will represent
an entire advertisement chain and all the associated data. Car files are compressed
and we follow some standard naming conventions. So we could really just share a large repository

of car files and that should be enough to be interoperable. Why a car file? Well, this

provides a data level interoperability so that different mirror implementations could
operate on the same data. That way we don't necessarily have to define a whole data exchange

protocol for mirroring. We just say, well, it's based on car files that follow a particular standard and a particular format. So our implementation is backed by S3, but it could be backed by

any file store. And that's another convenient thing about using the car files. It's very easy to put these on anything that can store files. So we have an S3 installation right

now that has roughly two terabytes of compressed car data and continues to grow. The car file

format is pretty simple. It is simply the roots of the advertisement, or the root CIDs

are the first two things in the car file, the advertisement CID, and if there is data
associated with the advertisement, the initial CID of the first chunk of the data chain,

and then the block of advertisement data, and then any following blocks of multi-hash

data chunks. And this is all there is to the car file. It can be read very quickly. We

use Carb-E2, so you can use an index to get to any piece of data in there. And this is

something that should be simple enough with our car utilities to allow anyone to interoperate

with the data in our mirror. We follow a naming convention with the compressed car files.

Basically it's named for the CID of the advertisement that's in it, but really the mirror will resolve

the name. So if we had different mirror implementations that maybe split things into different directories by publisher, by provider, however, it doesn't matter. The mirror can figure that out, but

the car file itself, once you have the car file and you know the format, then you can
read the data in it. So mirrors can be configured for reading, writing, or both. You can have

a read-only mirror, and this would be used when indexers want to use a mirror, but they

are not updaters of that mirror. So in other words, you'd have a different updater that maybe you have one updater and a lot of readers of the mirror, or you don't have any updaters
and you just start with a static bulk data import somewhere, and then everyone reads

the mirror to get up to speed. A write-only mirror is where you might have one indexer
or a group of indexers that are populating the mirror, and all the rest are readers.

Or where you aren't actually reading your mirror, you're just accumulating data in it because you aren't necessarily planning on using it for anything other than when you
want to start an installation or maybe you're sharing it with other parties. So you have a write-only mirror. And then a rewrite would be where, well, obviously you're reading and

writing, but you have maybe indexers that are both updating and possibly reading from the mirror if other indexers may have already indexed data. So this would be used in installation

when you have some overlap. So you have multiple indexers that are responsible for ingesting
data from the same publishers. So you might do that for redundancy in an installation.

So let's talk about the advertisement chain processing. So the advertisement chain, the

way that works is an indexer syncs the chain of advertisement only, and then it goes for

each advertisement and syncs the bulk data. And it's the bulk data where we get the huge amount of speed up. So mirroring, if you're going to be an implementer of a mirror, the
most important aspect of that is to be able to sync the bulk data. It almost doesn't matter
if you sync just the advertisement mirrors, at least from, sorry, sync just the advertisement chain from the mirror. In our testing, a chain of a thousand ads typically is about 150 milliseconds.

However, each ad syncing its bulk data often takes multiple seconds per ad. So if I have

an advertisement chain of a million ads long, I can get that in seconds where, and it doesn't

really help to use a mirror, but then when I'm syncing each ad, being able to pull it from a mirror, I might be able to do 20 per second versus one every two seconds.

So I'd like to show you a little demo of an actual two indexers that are running, and

I'm going to be syncing both of them, one from the original publishers and one from

the, a mirror that we have running in our S3. So let me go ahead and share that. All

right. So let's go ahead and run the demo. I'm going to run this, the one on the left

is the non-mirrored and the one on the right is the mirrored. So hopefully they're reasonably

caught up. So we'll actually be reading from the mirror on one side. Andrew, can I ask you to make the font a little bit bigger? Yes. Let me go ahead and do that.

Can I just increase it with, sorry, let's see.

All right. So we can see the progress here. So I'm tailing from the logs. So it looks

like this one's stopped. Yes, this one is done. So let's see what the time is. So one

minute, 28 seconds. And that's about what I've seen running this demo before a minute to a minute 30. And this one, let's see, is this, if this one is done yet, usually this

one takes a lot longer. Now it looks like it may have finished quickly. All right. But

that wasn't as quite as impressive as the first time I ran it when the second one took about five minutes. So, but I do want to show that we are actually reading these from an

S3 source. And if I look at the logs, we can actually see that we're reading these from

our S3 and we can see that they're car entries. So it did in fact read this from a mirror,

but the finish time wasn't that different this time. Anyway, so let me go back to the

presentation. At least we can see that that's working fine. All right. Well, everything

worked too well for the demo for both cases. So let's talk about the things we wanted to

discuss. Now that I've sort of gone over the car mirrors, what the car mirror is, how we

use it, what it's beneficial for. The second half of this presentation, we wanted to be
able to discuss where we go from here. So I think probably now is a good time to open

up with some questions and then we can jump into the discussions. I've organized these
discussions. There's I think five of them in order of what I think are most important
to talk about. And we can either go through these or just one at a time and then go back
and discuss them or we can just start discussing them now. But in the meantime, I wanted to

open it up if there's any questions so far about the indexer, because I kind of rushed through a lot of information there. So I'm sorry, the indexers use of that car advertisement
mirroring, excuse me. Are there any questions? You might have talked about the individual
pieces of this, but just to put it all together, what in the event that you try to mirror a

set of cars that it's no longer like in sync with the latest advertisement chain, what

happens in that case? Well, what happens is if you're only reading from the cars, then

you're going to get the older data that's no longer valid until your indexer indexes
newer data from the publishers. So the way we normally do this is when we're bootstrapping,

we'll use the car mirror to get up to speed with data from an ad chain that's read from

the publisher. So let me back up and be more specific.

So remember I talked about the ad chain itself not making a big difference whether we sync it from the mirror or the publisher. So what we do is we get the initial ad chain from the publisher, which doesn't take any more time at all.

And then we start syncing the ads. If the ad is available from a mirror, and we go in chronological order from oldest to newest,

and there's some optimizations like we'll look for the any deleted content on that chain before we start syncing the individual ads so we can skip ads that don't have any more data. We also look for what is the latest

set of addresses. That way we don't push old addresses into our indexer. We look for for data that would be updated and then we start syncing the individual advertisements. So if if the advertisement is still current,

then we can grab it from the mirror. Otherwise, we can skip it because it will have been replaced by a later advertisement.

So that way we only read from the mirror valid data that we know we need and that allows us to not have to go back and edit a lot of information or put invalid data into our indexer.

So yeah, we sync the chains from the publisher, sync the data from the mirror and then at the point we've set up, the chains have been synced, we know what data we need from the mirror, which is current.

And I just want to add something to what Andrew pointed out. So when we are processing advertisements, ignoring mirror completely, we still have to be tolerant of latency.

Because what we see as latest is not exactly latest anyway. So then the idea is whatever you think is the latest, you just get that processed advertisements and then later on, when the newer things come along, then you just add those.

The main thing to point out is that the protocol for processing information has to be latency tolerant anyway. And that's like a different, more interesting, perhaps,

problem that just having the data in multiple places. Right. We don't actually know if we're all, we're never, we're never completely up to date because as soon as we have information, the publishers may have published something that's more up to date.

So we are always catching up. But we do some optimizations, like we can tell if data has been deleted and then we know not to pull that data in. So for example, we read something later in a car file that says previous data has been removed. We know we don't need to load that data.

I was going to ask if you could describe how this would be beneficial to other people that are running their own indexer instances.

Yes, this is, this is in fact, the slide that's right in front of you right here. So that is something that I want to talk about. And how do we make this useful to other people? So initially, it would be very useful in a couple of ways. Firstly, in order for somebody else to be able to bootstrap their indexer installation, or if their installation, if they maybe aren't as caught up to somewhere else.

So maybe we have sid.contact, which is always indexing everything. And so it's probably pretty well caught up. Maybe somebody else's isn't.

Or maybe they just don't want to read directly from the publishers because it's slower or they don't want to burden the publishers. So they'd rather only read from a mirror. And if we provide that mirror, then that could be the alternate source of data to ingest. So those are the ways it could be useful.

So then the discussion goes into, well, how do we provide that? And who pays for it? And how do we, who updates it? Who, what are the policies? And that's really the discussion. So that's like a perfect segue into this discussion.

I want to talk about, say, public readable advertisement mirror. Let's say at sid.contact, and we have a mirror, which is pretty well populated. We could give this, we could open up access to the public. But how do we do that? Do we just share a big S3 bucket? Do we have a service on that's able to, you know,

do we have a service on that's able to, say, request certain car files or certain data? And what makes sense? Who updates this? Is it just us? Or can anybody else who might be more ahead update it? Or can you as a publisher?

So another topic I want to discuss, push your data directly into the mirror instead of publishing it on your, you know, as a publisher normally would. And that's maybe that's an alternative to publishing data.

So, so thoughts on on this. I don't have a proposal for how to do this, but this is the kind of thing I want to make sure we at least understand what people want to do with this. If we shared a big repository of car files as a read only mirror, would that be something that would be useful to people?

I guess one thing to point out is that the advertisement information itself is also content addressable data. We're already building a lot of systems and distributed exchange mechanisms to exchange exactly that type of content.

It just so happens that in this case it is advertisements. So another way of thinking about it is can we use the existing CDN mechanisms like Saturn, for example, to serve advertisements just like they serve other content?

And that comes with a nice property that we don't need to make anything special for making only advertisements available because this is just car files we're talking about.

And the other aspect that I see is I don't think it is just the responsibility of the indexers to make this data replicated. It could be or perhaps it could be far more efficient or perhaps just works out much better if we build something on the provider side, built out protocols for them to then publish copies of their advertisements elsewhere because it's all signed anyway, verifiable.

And then convey the information to the indexers about where they can go get it. So that's another angle.

So we talk about this mirroring and of course at the first stage it's much more usable or useful to the indexers themselves, but this is something that goes beyond indexers in my opinion and includes providers themselves.

So it's not just, right, from providers perspective, it is in my benefit for my content to be discoverable. And I think we can leverage that to get providers to do a little bit more in terms of just distributing this information better such that it can be ingested faster and so on.

What do you think Andrew? I think that actually wraps up a couple of the topics of discussion we wanted to talk about. So yeah, those are all very good points. Let's talk about a couple of things that you mentioned there.

So being able to provide a mirror as a way of publishing advertisements. So yeah, a content advertising publisher, they could either create a mirror or they could push content into a public mirror that offloads the need to serve all of that data themselves

through the normal advertisement protocol. They could say, hey, here's a mirror with all of the car files in it and then use S3 or some sort of a CDN to serve those files directly.

Or maybe it's because all those car files are content addressable as well, the advertisements are content addressable, those can be put on any sort of a shared content addressable storage system or distribution system.

And I did mention that part of the strength of using a mirror is that it concentrates the data. All this data has been spread out among different publishers so having a mirror concentrates it in one place and makes it quickly accessible.

So if you do something like say put all of the advertisements or you know whether they're in car files or whatever, and in something like IPFS, well then you sort of redistributed them and now you're back to pulling individual car files from disparate locations

and you sort of lose the advantage of the mirror. The other thing that was mentioned, that's related to publishing to a mirror as an alternate way of publishing is using a mirror as a cache for the advertisement data.

So one of the interesting things about this is if you think of all the index or installations as most of them are generally caught up.

You know, maybe, you know, at least within some reasonable amount. Maybe they're a day or a few days behind. Maybe a week or two behind at most. If you were to have a mirror that serves as a cache for the more recent data, you don't have to keep everything in there just keep the last few weeks of published content in a mirror, in a public mirror, and then all of the indexers can use that mirror to see if

the content is already there before they reach out to the individual publishers to try to pull that content out.
So if that mirror is fast, it doesn't have to necessarily be huge, because it's only storing recently published data. So that's another way that a mirror can be very beneficial.

And if you provided mirrors as caches if Protocol Labs say provided content advertisers mirrors as caches, anybody's indexer could pull from those caches first and that could save a lot of work.

It could take a lot of the burden of serving data from off of the publishers and provide an efficient way of getting the data that most indexers would be asking for.

So that's that is also another another possible way we could use it and that would be something that probably someone like PL will be interested in providing, but it could also be provided by individual publishers and a publisher could use that as a cache

to pull the data, hey don't don't pull this data directly from my advertisement service system. If it's an S3 go here or it's in a CDN or pull the these car files for those ads from some other location that's that's less expensive and faster to serve them

from. And then you get all the compressed data, and you get all of the advantages of having everything on one, one place to quickly read it from.

So there's a lot of things we could do with the mirrors. Those are a few of them. But we want, but these are the questions we have to start answering and I don't know that we're going to have all the answers today or, or even even come up with all of the possible ways where we might want to use them, but I want to be able to start the discussions

of how do we start using this what do people need. We have a mirror right now which is fairly well populated.

We can start giving that to people who want to bring their indexers up to speed. Also as people move to double hashed indexing, there's probably going to want to be a lot of re indexing done to convert over to the new the new double hashed indexers, and in

the conversion process re indexing is probably going to be a popular choice. So, in order to lessen a lot of the pain of that. Could we share our, our, our mirror somehow.

That would be, I think, figuring out how to do that would would actually be very beneficial to not only PL but the anyone who's running these indexers of course.

Do we, so we can probably collect a number of ideas and we can discuss them in whatever form is appropriate for that.

We're ready to something that resonated with me. Yes, is perhaps distinction between hot and cold data.

So, I put myself in the shoes of somebody that is perhaps considering exposing one of these right. Why should I do this, and how much is the total cost to me, like this, these are the things that I would be interested in right.

So, this distinction between hot and cold data could be useful to those ideas audience as well such that it provides some sort of upper cap in terms of the total amount of information they store and they basically would periodically or actively remove advertisements that are older than x duration.

Then the idea is that the way that the mirror would then evolve is it is much, much faster and much, much easier to find most recent advertisements, which are probably pointing on pointing to data that is more important, not necessarily probably.

And then advertisements that are older are are then harder to find to have less replicas, you probably have to go to providers directly.

That's what I think. What are your thoughts on that sort of interaction, but not all advertisements are equal, but some are more equal than others.

I think that that is exactly what we're looking to do as far as a cash and.

Now, who decides the, the policy for cash expiration.

So it can be based on time, it can be based on how frequently something is used maybe something is very old, but it's, it's asked for it, a lot of it.

Actually take it back the indexers would would be able to, you know, at that point it's really about getting this into the indexers so it really only matters as far as how frequent or sorry how fresh the information is not how frequent it is, is accessed or query.

So I so as far as the caching that I have hot data, I think that's exactly what we want.

You can keep data that's been published recently, say a day, week, whatever is a reasonable retention strategy.

The way it's in a, in a mirror that it's easily accessible takes most of the burden off the publishers, by having a small portion of the data in in the mirror because most of the indexers are caught up enough where they're going to be creating the same

recently published data. As far as cold data is concerned, you could even have a, you could have a mirrors that are staged in for different time periods, or you could just say hey nothing if it's not in the hot data mirror, then you go back to the original publishers and that's where

you fetch your cold data from.

And if you have a mirrors that have hot, warm and cool data, you could do it any way you wanted to, and they could be, they could be set up appropriately for the amount of traffic that you anticipate them getting so the hot cache would would have the highest

bandwidth and things like that but maybe it wouldn't be very big, whereas something that was for older data would be slower, but less expensive to store more data.

I'm not sure if the, like having concrete senses of scales of these things does help us be a little bit more intentional in what we're aiming for, because, like in an optimistic world we've got what may be order of 100 full replicas of full indexers.

Like this is like we're not expecting a world with thousands or 10s of thousands of indexers like you want an indexer in your data center, and then you're going to have caches that that are going back to those full indexer replicas as the thing that disseminates past that.

So if you're only have you know order of 100 indexers.

None of these caches are going to be that hot. There's not that many indexers to like to do that, but I think you're right that we've got to access patterns. One is the mostly caught up indexers syncing with each other on new content that's going to keep happening.

And so as a publisher publishes a new content. What's the right dissemination, so that it's not all hundred indexers go back to that publisher, but that they share amongst themselves.

And then the other one is a new indexer comes online and wants to get all of history to replay it or otherwise to copy some other indexer.

And so that then that's much rarer, probably, but but is much more data.

And so that's the question of do we keep a full car mirror of all of the active history or the other thing you could say is if there's multiple of these mirrored archives that we're using for the staying in sync thing.

You might have them find some way to slice because you expect that that full history thing is a pretty big long term like like that'll take a week. I think right now, right, is our expectation.

And so, okay, great with mirrors, maybe we can make that take hours or a day, that would be better than a week.

But, but that I'm going to multiple data centers and multiple mirrors for different chunks of it is not going to be the difference.

And so you could imagine all of these mirrors have the the current one, but use some sort of stable hash to spread out that overall history of the full active set so that a new entity gets, you know, a fraction of it from the different mirrors.

If they need to replay everything that you're not so much looking for like, you know, a strategy of, you know, like hot or used or time you want to have diversity so that you're, you're lowering cost while still having some mirrored copy rather than going back to all the providers for anyone new starting up in those unlikely occasions.

Yeah, that actually. That is a reasonable way of reasonable strategy and we don't have to have any particular set of data in a mirror, it's either.

If I, there's either something in a mirror, or it's not and I can look in whatever mirrors are available to me.

One thing I do want to avoid as an indexer. Indexer is going to not want to have to sit and pull multiple mirrors for every advertisement, and that's.

So if we're going to be pulling advertisements from a mirror going to think okay is it in this one is it in this one isn't in this one that's going to kill the performance.

Whereas, if we can identify an entire advertisement chain is in a mirror.
And then whatever advertisements from that chain will pull only from that one mirror or assume that they should be in there.

So, that's something that we want to consider is, is not.

èI'm trying to identify if a chain or some portion of a chain is in the mirror as opposed to for each advertisement trying and mirrors.

èSo the access pattern that we see for indexing generally the publishers are announcing for the

most part, you know, we're seeing one to 10 to 20 advertisements at a time that we're pulling in.

èSo it's not particularly long chains. So it's, but it's still, you know, when we have a chain of them,

even having 20 advertisements having to look in different, different mirrors for each one is going

to sort of kill the whole reason to use a mirror. ÈYeah, that's.

èI think going on providers is seems reasonable. We're already doing that for the shards within a

instance, or a full replica of an indexer is that it has different shards that are partitioned on
provider. But you could imagine in the same way that we're doing that, that the different indexer instances, so we run one, you know, other entities run their full instance of an indexer.

Those also can be partitioned in that same space so that you go to the mirror provided by that
mirror provided by that replica for a given provider on your sinks. And so it's essentially

it's that shard then that, that that instance overall is sharing to the rest of the indexers.

But you try one, you see if they've got the stuff you need for the providers in that shard, and otherwise you go back to the original providers. It means that there's some correlated set of providers that are going to get hit if a shard goes down. But I think that's probably

always true. And right, like if the mirror that's responsible for some set of content
falls off, all the other indexers are going to need to go back to the provider for that as that heals, but that's fine. Yeah. And sharding by providers that actually works
with what I was saying before about looking for a whole ad chain, because the ad chain is at the time we process, by the time we process it, we've, we've isolated the ad chain to a

single provider. So by sharding the mirror by provider, we've already been able to essentially

correlate with a, or associate a, a mirror, a mirror shard with a provider, which they're,

which then associated with the whole chain. So that all works actually quite well. If what I said was clear, I'm not, I'm not entirely sure, but, but yeah, processing the ad chain, the chain processing is all, is for a single provider. So the, the sharding by provider is perfect. And yeah, you'll have to heal if, if a shard goes down. Yeah. You'll

have to heal that shard, but you'd have to go, you, you, you know, if the provider, you know, you've got to process that chain by going back to the original provider anyway, even if a piece of it was down. So
if a link in that chain was bad, then you'd have to, at that point, go back to the original provider to pick up that link. There's another angle in mirroring that I can see, which is
the information we are mirroring. They're not all, I mean, by, by information, I mean, individual

fields in the advertisement, they're not all having the same sort of importance. So what do I mean by

that? For example, a provider's address and a metadata and extended families associated to an

advertisement is probably a far more important piece of information to keep up to date compared

to the list of multi-hatches that they advertise. Why? Because the, the, the, the, the, the, the,

the, the, the, the, the, the, the, the, the, the, the, the, the, the, the, the, the, the, the, the, that stops or makes the connection to actually fetch to CIDs, right? So if I have two CIDs

of total but my address has changed, if the address gets updated, you know, last, it means my CIDs,

regardless of how many of them indexers know about, would become undiscoverable, unaccessible,

unretrievable apologies until the indexer reaches the advertisement that indeed changed some metadata or introduced extended providers.

Right. What are your thoughts on this uneven importance of information advertisements?
Oh, thank you for asking. That's actually I couldn't have paid a better show in the audience to do so.
Um, the way we do the indexing specifically accounts for that by reading the late by getting the latest advertisement, not from the mirror, but from the provider, because remember, we think the chain from the provider, and then we think each advertisement from the mirror, if the mirror data is there.

So what that means is we get the latest that's available from the from the publisher of that advertisement data and and then we use that latest address latest.

All of the latest information, ephemeral information, addresses, extended providers, etc. And then as we pull in all the information from older ads in the chain, we, we have already updated our indexer with the latest ephemeral information. So all of the older versions of that get ignored.

So we don't, in fact, even need any of that to be in the mirror in its current form. We save the advertisement in the car file that is mirrored.

But that's really as more of a historical record and a way of recomputing signatures or validating signatures on this data. We don't actually use the advertisement itself in the car file as is. So yeah, this ephemeral data is all is always retrieved from the publisher itself.

One of the things that we might want to do when using mirroring, though, is it might be important to get whatever the latest advertisement in that mirror is

and then use that information from the latest advertisement if you don't have something newer from the publisher. The mirror does currently keep a head file. A head file says

what the most recent car file is for any single publisher. So whoever publishes an ad chain,

we were also recording the mirror what the head, what the latest ad in that whole chain is. So that way, if somebody didn't have a publisher,

they could read from the mirror, whatever the latest in it was in the mirror, and then get that ephemeral data from that latest advertisement in the chain.

So we do have ways of dealing with that already. Indexer already deals with it by getting that from the publisher. You can, and you can deal with it in a mirror by looking at the head file, which we didn't talk about, but we do maintain a head file.

Andrew, what about a case where extended providers are added somewhere in between, right? So imagine I added two extra protocols by which data could be retrieved.

And then I carry on with the rest of the advertisements. And the advertisement that added this extended providers was at the chain level, which means it applies to all previous and future advertisements, right? It won't be included.

Does the thing that you described cover that case too? Let's see. I'm trying to think if we cover extended provider information specifically with, if it's added in it. So if we take what the latest is, so I'm going to say that I don't think, as we're reading through, if the latest information is not the complete set, then no, we don't cover that.

So that may be something I guess where I'm getting at is there might be a need here for ingesting advertisements from head to tail and perhaps in parallel head to tail and tail to head where we are ingesting different things, right?

The head to tail one could be ingesting retrieval information and address updates and that just gets the advertisement, obviously tries the mirror first and then reverse back to provider, but goes head to tail in order to keep the latest state of how the data could be retrieved and whether it could be retrieved up to date because that's fundamental to the whole process.

And then you can imagine another process in parallel that goes from tail to head or even head to tail again that its job is to just ingest CIDs and associate that information to this value key thing. What do you think?

So on the head to tail traversal, you don't actually need to traverse, you only need the latest in the case of having most up to date set of information.

So, you know, I think that's a good point. I think that's a good point.

I think that's a good point.

But otherwise, there really isn't a traversal from head to tail. It should just, for any ephemeral data, the ephemeral data should just be read whatever is on the latest advertisement.

As long as we know the latest advertisement, we can just read the, we can simply read the advertisement from that car file from the mirror or from the, or whatever the latest one from the publisher is either way.

And that's the ephemeral data that now the advertisement, sorry, the latest version of that data that's advertised and we just use that.

So we don't have to traverse for that stuff. That's my, I don't, and I don't think we ever should have to traverse. If we ever have to traverse head to tail to get something to get the most recent data, then I think we're doing it wrong.

That should always be brought up to the head of the, of the advertisement or extracted from the chain that's been processed and put somewhere in the mirror that we can instantly read the latest version of.

I think the setup with extended providers is that we have this like override extended providers flag that's not always set, but is the basically says this is the extended provider that now should be set for anything where it's not explicitly set directly for that advertisement.

I believe in practice, the index provider sets that every time it starts up also at the same time that it's changing like and re reconfirming what its current multi adders and things like that are.

So we, we would get it. You know, fairly close to the chain and in that sense it is the most recent time you've seen that extended records be published it's just that it's not necessarily in every advertisement so it's not necessarily in the most recent advertisement.

But as soon as you see that that set as you walk backwards, that's the most recent one that overrides any previous ones that were set so you only have to find one of them.

So it's, it's a bit of a traversal still, but we do republish it in practice, pretty regularly. So, we could probably get away with not doing much of a traversal, and we could also save that most recent one again with a car mirror or something, rather than having to walk back and unknown amount of time to find that most recent one.

Yeah, I think that's a really good point. I think that for the mirror what I would prefer to do because even though you shouldn't have to walk back far we don't know how far that is.

I'd rather just extract the latest version if possible.

Is that, is that something that's possible as we're writing the files.

I update the latest. So yeah, I think that would be the easiest thing. Let's just extract it just like we write the head file, here's the most recent one. As soon as we encounter extended provider information, we'll just write the most recent, and we'll update the most recent copy.

See, I didn't realize that wasn't in all of the ads, so if it's not, that'll be, I will, I will make an issue for that right now and, and then we'll, we'll export that in the mirror. That way we can not have to traverse back when reading from the mirror.

That's reasonable. The only other thing to think about there is I think the extended provider record is not directly signed itself. So, do we just trust from our mirror read that that is indeed a correct thing, or we're going to have to follow the chain and back to verify that indeed this is the most recent one.

So the other thing that you could imagine doing is that we could be adding a pointer to the most recent extended records, SID or something in every head, so that you've got the head object and that signed thing also includes it. Maybe that's worth doing.

We've got the ability to iterate on some of that part of the protocol, and maybe that is something we do want to do in retrospect.

I think we always need to have traceability back so you can rederive the signature. So if I have the latest extended providers information, it should also just have a CID of the advertisement that it came from. So that way you could go back and load that original advertisement, either from a car file or even go back to the publisher and then verify the signature on that.

So you compare the data, make sure that's the correct data, and then verify the advertisement signature. So yeah, we should always be able to have traceability that allows you to recover a signature or verify a signature.

So I will add that, but that doesn't actually affect the indexer as it is, because the indexer is always processing the chain from the provider. It doesn't get the ad chain itself from the mirror, although it could.

That tends to not be any faster and in some cases actually slower to read each individual car file to get the chain. So it's better to get the chain from the publisher directly. That's very, very quick because that has a very small amount of data.

And that way, we always have the latest extended provider information and we don't have to walk back along car files in a mirror. So I think that this isn't a high priority because it doesn't affect our indexers, but if somebody is strictly using a mirror, then

they probably want to get that data so that they can use it immediately as opposed to wait until their indexers caught up enough to see the latest.

So I suppose that brings us to one more thing. I don't know if we want to talk about this here, but there are some enhancements to the mirroring itself that were suggested.

Talking about keeping chain metadata and that could include things like latest addresses, latest extended provider information.

There are some big downsides to having metadata for a chain, mostly that every single time you get a new link on that chain and new advertisement, you have to update this metadata and that metadata itself can grow very large and most indexers may not actually

even need that unless you're ingesting the chain. So how do you segment that metadata?
So it becomes a complicated issue. We can talk about some of those things, but one of the most useful parts of having this metadata is just being able to skip empty advertisements.

Another proposed way of doing that is with a simple replacement of empty car files, you replace them with a specialized spanning file. This would be a specialized car file that replaces the first, traversing from head to tail, it will replace the first advertisement that has no data

and then have a CID that links to the next advertisement that has data. That way it would allow the traversal to completely bypass an entire set of car files from the mirror.

Andrew, can I ask a question? Yes. I'm curious. This sounds like a really great advantage, but simultaneously I'm wondering if the reader privacy conflicts with this capability.

Do you lose that ability or does that interfere with once you have an encrypted key value store, the ability to do these things?

So no, reader privacy, it's a very different, it's a completely different thing because reader privacy really refers to asking the index, the client, the retrieval client, asking the indexer for something in a private way by giving it a hashed key lookup.

This is all on the ingestion side of the world. So this is all for handling the data that's coming from the publishers before anything's encrypted.

So before we do any double hashing out, that is. Well, does it then negate the benefits of reader privacy? Because now there's a unrestricted or unencrypted?

No, it doesn't because we don't know who's reading this. This is just who's ingesting this. We can see that, oh, here's all of the, well, it depends.

I guess it depends how you look at reader privacy. If reader privacy, if you strictly mean we don't know what a client is trying to read, then no, this doesn't affect it.

This does, you know, having, let's say a public car mirror does provide a means to see what everybody is publishing, but you can always get that from the publisher anyway, if the publisher is public.

So this doesn't affect that at all. And then we can, as we ingest these things, we double hash it. And then the reader privacy is still sound because we don't know what the readers are actually asking for because they submit a hash.

I guess there's a blanket of sort of security that we have been talking about on the reader privacy side, and that is, sure, you can contact every single provider and get the chain of advertisements and entries and double hash every single CID there.

And then right there you have the master table for deciphering all reader privacy stuff. And the blanket of security that I'm talking about, and it's probably a thin one, was that these providers are distributed.

It will take you a long time to crawl the whole thing. So what we are doing here is that we are making it just more expensive to make sense of this double hashed lookup.

But then on the other hand, we are making it cheaper to get all the advertisements.

So technically we are making that blanket thinner, but nevertheless the cost still is present that if you wanted to make that master table thinner.

LLC.govؤ�a ów�m lápSlyly mñEqí�»ÁŠŠŠŃŰŽŽŽŽŽŽ�ŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽŽ

So does a mirror, yeah, does it does a mirror really cheap make that process a lot easier and cheaper. It makes it faster but doesn't make it cheaper because you have to store the same amount of data to have this gigantic lookup, or you have to search the same amount of data if you don't create a pre created table.

So, yes, you could run your own indexer installation and, and this and via a reader, the anonymizer and you know deprivatize or.

Awesome, Andrew, thank you so much for joining us remotely for a colorful discussion into advertisement mirroring and what it means for index providers as well as providers themselves. Are there any parting thoughts from you before we wrap it up.

No, I just look forward to continuing discussions on various forums and throughout the community, and hopefully this presentation has been a good enough bootstrap into the into some of the ideas behind the discussions we need to have to figure out how to make this most usable.

Thank you so much.
