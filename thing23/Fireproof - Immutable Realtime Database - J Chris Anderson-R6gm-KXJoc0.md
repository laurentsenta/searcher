
# Fireproof: Immutable Realtime Database - J Chris Anderson

<https://youtube.com/watch?v=R6gm-KXJoc0>

![image for Fireproof: Immutable Realtime Database - J Chris Anderson](./Fireproof - Immutable Realtime Database - J Chris Anderson-R6gm-KXJoc0.jpg)

## Content

Well thanks y'all for coming out to do database stuff with us today. So I'm going to be talking

about Fireproof, which this is the official launch. If there's an official launch of Fireproof
it's right now and I can prove it because I have this super hot hot sauce that all the
Europeans are going to start sweating when they eat. And yeah, I got into this maybe

14, 15 years ago building peer-to-peer databases with Apache CouchDB and have been super excited

about this space since then. And kind of sitting back and letting the infrastructure create
new opportunities to build cool databases. There wasn't a way to build a database cooler

than Apache CouchDB until now. And so with the advent of Prolly Trees, well we were able

to do stuff on the immutable deterministic data backend that we weren't able to do before.

So Fireproof takes advantage of Prolly Trees and it's been really enlightening to hear

the other discussions today from folks who are deep in the database engineering side
of it because I knew there was a lot that I was just touching the tip of the iceberg as far as my understanding of the data structures. So as far as the value that I'm trying to

bring to the table in this community, it's how can we package this stuff to be so easy to use. There's a lot of power in Web 3 and almost all of it has gone toward like let's

do something so totally super new that we never could have done before. And so that's
great and my instinct is to take that capabilities and say let's take that thing that everybody

wants to do and just make it so easy that they can't believe it got this easy. So that's what the goal with Fireproof is, is build a database that React developers can drop in the page and have interactive application features and then get all the data integrity
and replication capabilities that you normally need a backend database to handle. So we talk

about IPFS and peer-to-peer means the data can be anywhere. You can ask the network for
the data. The database is the network. And if that's all true, then the place that you

want your operations to start is as close to that user as possible. So in the browser

or in the mobile device. So that's what Fireproof is about. And the big goal here being to make

it just easier than whatever, what was the previous easiest database? Like whatever it

was was like 10 times harder to use. So that's the idea here. And then there's a bunch of

stuff that we get because of being part of this community and using these data structures
that is like more serious than the previous easiest to use database. So with Merkle proofs,

we can do things like data provenance for AI API calls. Anything that you see signed,

you drop in there. You can sign Merkle proofs. You can put Merkle proofs on the blockchain.
And those are all use cases we were talking about in the last couple hours. So I feel like that sitting in this space in the engineering tradeoffs by being on the browser but using

Merkle proofs and using IPFS and Web3 storage for replication allows us to make a database

that doesn't give away any seriousness by being so easy to use and so close to the end

user. Which, you know, with my experience in Couch, you can do that, but we had to work

a lot harder, I think, than Fireproof has to work in order to make the database useful
for enterprise contexts. Yeah, I want to get into the actual how it works by diving through

the mirror. And so we're going to talk about document lookup and the event feed and how

the indexes are built. So that's all the application layer data management stuff. And then we'll talk about the infrastructure side of it. So how transactions work, how replication works, how encryption works, and how sync works. And about 50% of this stuff isn't written

yet but, you know, the whole thing is discovered, not invented. So it's not like there's a whole

bunch of other ways it could go. So there's two main data structures in Fireproof. We

just heard all about Prolly trees. So that's what's going on over here. And we'll come back to that in a second. But we also use Merkle CRDT clock. And that is a lot like

the data structures that we heard about from Aaron's talk at the beginning. So what the

clock does, and thanks Alan Shaw, who's going to be giving a detailed talk on this clock
library in this room in the afternoon, is it allows you to have update histories that

diverge and merge, and then to ask questions of that update history in an efficient way.

So the way that we use this is every new update is based on a previous update. So when you

write into the clock, you know, your event is always appended to the clock's current.
But when you come back to the clock, you can have it give you everything since, like, say

this update. So if you were here, what would you need to sync to get everything you didn't

already have before? Well, it would include this, you know, as well as everything since
then. So being able to pull those nodes out of the data structure and play them forward

is one of the things the clock offers. And we leverage that not just for, like, the update
cycle, but also to power the indexes. Because when you go to query an index, like, you're not going to want to update the indexes on write, because as we saw, Prolly trees are going to be expensive to churn every single time there's a little change. So instead, we query the index on read, and that gives us natural batching, all the updates that happened since the last time, then get ingested at once into the index. But that means we need to go to the clock and say, what's that batch look like? And so that's what this is.
We'll go, you know, from the last time the index was updated, and then pull in all the relevant events and play them into the index again before we query.

So what is hung off of each of these nodes is a version of one of these. And this is

the Prolly tree JavaScript implementation that Michael Rogers wrote that is part of
the same spec as the Go one that we were just hearing about. So it's going to, I mean, I

think it may be diverging from the spec a little bit, which means we're innovating, and now it's time to go push those changes back up. But because of the way these Prolly

trees work, if I were to put a change in here, you know, it's only going to change these

nodes. And so the operations are relatively lightweight, which means that although we

have a version of the tree hanging off of each one of these events, the delta, the difference
between what's going on in one of these events, is pretty small. So it's not like we're duplicating the tree over and over again. This essentially means that we couldn't have built this at all without Prolly trees. If you tried to build this without Prolly trees, you'd get huge write amplification with, you know, just a ton of disk work for every little thing
you did. But instead, almost all the writes resolve to stuff that's already been written.
And so it makes it efficient on the back end to work with these kind of things. So, yeah,

that's how the storage works. And a little bit of how the event feed works. I'll talk

about, you know, how the indexes are built. It's like an Apache CouchDB style index. So

you write a JavaScript function and you say, I want to index, you know, anything that has

a title, I want to index it by title, and now you can browse all the blog posts alphabetically.

So the indexes are also built out of the same data structure, which was, is a choice that

I didn't have to make, right? Like, the core data structure needs to be a Prolly tree,

and it needs to be working with this clock. But the derived indexes, you could just say,
hey, I'm going to keep those only in the browser and have them be raw indexed DB. You might
get like a small performance gain by doing that, even. But the reason to have the indexes

as well play in the Prolly tree content address game is that when you issue a query into the

index, you know, it's going to hop down the nodes until it finds the range it's working on. And what I can do when I give you those results and what Fireproof does and where
the name Proof comes from is it includes all those CIDs in the result set, and you can use that for all kinds of stuff. Maybe the most straightforward is just an accelerator.

You know, you know, perhaps you have a big data set and a client wants to query it, so they send the query over, and oh, these are the blocks the client needs to have in order to traverse into the tree, even the big tree, and not miss anything. So then you can ship
all those blocks over, you know, like a Graph Sync style operation, and the client is able

to operate seamlessly on the big data set without having to replicate it in advance.

That's most of what there is for the front end. There's also, I suppose, the same way

that the indexes know to bring themselves up to date by consuming all the changes that are relevant since the last time they were updated. There's public APIs, so you can hang your own indexer off of that and hydrate a full-text index from your document set or

machine learning model or anything that needs to play causally through the data and be able to pause and restart and pick up again, you know, or pick up new changes after they've
been applied. So the same feed, which, you know, in that case allows you to incrementally

index stuff and is kind of a database-y feature, is also a very, like, it's probably the priority

zero most important feature, aside from being able to put and get data, for React developers,

because once you have this time to refresh function, and here's what you need to refresh

with, now you can provide React developers with that auto-refresh that repaints the page,
so when you're collaborating with other users, if database changes come in, the page will repaint automatically, or if you don't want to have to thread all those data changes and

know what to refresh as a developer when you make a change, you know, you can put listeners

into your app listening for specific events in the database, make changes in the database, and have the database dispatch those events to your UI, which most React apps end up doing

that anyway, but without the benefit of the database, right? They have to do all this data flow just to get the events to refresh in the UI, and if they were to just put that
through the database on the way, then they get both jobs done at once. So it makes for

a simpler, faster development. So when we get into the storage engine side of it, now,

this is the part that is less visible to the functional requirements and more about what

you can do with it. Everything that the database does writes to an in-memory block store, which

is just a JavaScript map of blocks from CID to block, and that at the end of an operation,

you're going to have updated a handful of blocks. So your per-operation block store
might only have, like, six blocks in it that you just wrote. So what we'll do is we take

those blocks, and we wrap them in a car file, and this gets us, like, entry into all the

new transport stuff that's coming online with Web3 storage and Saturn, because car files

are, they're just a bag of blocks. It's like a tar or a zip, but for IPFS blocks.

And by putting those at our transaction boundary, we get all kinds of neat stuff, right? Like,

it doesn't have to be at our transaction boundary, and there's other times we might repack it
into other sizes, but if my car files are coming off this, you know, let's say you've

got an active database, now you just have a queue of car files, one per transaction coming off of it, and I can, it's more I don't do this, but I could, and it works, upload

those car files to Web3 storage as they happen. And there's certain use cases where you'd want your data to be in the clear, and for that to be your replication strategy. When
you do it that way, you can just delete your local index DUB, and as long as you have the
ID of the root of your PRAWLI tree, then the app works just fine, even if a little bit
slow. So it is kind of nice to replicate in the clear like that, and we'll keep that

an option for databases with encryption disabled, but encryption is enabled by default, and

so that kind of replication doesn't work with encrypted data. You have to think about it a little bit differently. Luckily, when you replicate encrypted data, I think it's faster, because you're pulling down car files instead of blocks. So to talk a little bit

about how it works in the encrypted case, what we do is between the clear blocks that

were written to the in-memory store and the car file, we encrypt the blocks with a symmetric

key, and so the symmetric key, it's not like we're doing some kind of fancy public key
thing there. We're not doing key management. We're just making it so that the blocks that go in that car file and get sent to Web3 storage aren't readable by anybody who happens by, and so you have now, as part of the local data that you need, the head that it takes

to decode the PRAWLI tree, not only do you need the CID of that root, but you also need

the decryption key. So now you've got a tuple of kind of boot-up information that you need
to read the tree. But it also means that what you're replicating up to the cloud is gonna

be something you're fetching back by car instead of by block. So it's gonna give you, in general,

more of like a GraphSync accelerated refetch. There's a lot of other things that we can

do when we expand the car file scope beyond a single transaction. So one use case for

these car files is like, let's say I have a user profile builder page that I give the

user, and they're gonna choose the background color and which image they want and what their
caption is and that kind of stuff. And so that would be an HTML file with a fireproof

in it, and as they're working, saving that information to fireproof, and at the end, they'll have that cookie-sized thing that you need to rehydrate from. So they can save

that to your existing application database, your MySQL or whatever, and then when you

go to build your static site, including all those profile pages, you can say, hey, fireproof,

give me the car file for that entire database. So maybe it's like 1,000 blocks. It's everything

the user wanted on their profile all wrapped up into one content delivery network-friendly
package. And so then you'd have your compact database in one file in your Gatsby static

site build so that when the page fetch first happens, the users are interacting with a

dataset that's been scoped for exactly them. If there have been changes that came, you know, if the user, the composer of the dataset has done work since the publish operation,
you can fetch the diff and hydrate the new data right there in the browser.
So that's a use case for packing the car files or packing more than one transaction in a

car file is you can put the whole database together for an accelerated download. One

of the other things, like maybe let's go back and if these encrypted blocks are the
neon green, if we have the non-encrypted blocks, right, in a car file, there's other useful

stuff we can do because it's on a transaction boundary. So one problem in this world of

building collaborative apps, probably one of the harder problems is key management, especially for shared data. So you can do, there's lots of fancy solutions. I might even
outline like one that's on the horizon for doing key management with shared data because I think it's neat. But what's neater is not having to do stuff. And so the way that the

sync, I distinguish sync from replication in this context. Replication is taking your

data from your application and putting it somewhere persistent like Web3 storage. And sync is working with collaborators in real time on your data. And so if I want to sync with some people on a dataset and fireproof, then what I would do is create a trusted channel

like a WebRTC or something that we're all in and just bladder around these car files
in the clear. There's no reason to encrypt it because we are already, you know, as end users encrypting our own data at rest copy of it. And we already trust the channel to
be secure and who else is in the channel to belong in the channel. So we send them the data inside that channel and then they're able to encrypt it for their own persistence. So just one of the ways that the, you know, my hypothesis starting this project or rather

joining Protocol Labs, you know, a year and a half ago because I wanted to do this project was I bet that when you get. ľI think it's database building time and so that's it turned out to be database building time.

ľI think that's it for the how it works section. Oh yeah, okay, so question about the encryption key.

ľI actually I did I said I did a teaser about maybe something a little bit more nuanced. So what I'm doing right now is the simplest thing that could possibly work,

which is for the entire database. There's just a fixed AES symmetric key that's used to put that in to make the car files and then that means it's safe to ship the car files over the wire to somebody you don't trust.

And that's essentially the only purpose of it and it punts on the key management. It means somebody's going to have to figure out a sane thing to do with these keys.

But in the absence of figuring that out, if all you do is treat it like it's part of the rehydrate cookie, you're still better than if everything was in the clear.

So that's the initial scope, but let's say I wanted to do instead of a live sync to live participants, if I wanted to do an encrypted exchange,

then what we could do and this is with some of the new UCAN like attestation stuff that the team, Web3 storage team has been working on recently,

is each one of these encrypted cars gets its own one time key and then in the UCAN that's associated with it, you'd say, you know, for each person in your group,

you would encrypt the one time key with their public key. And so you're only shipping the diff once, but it's encrypted securely for the participants

and you only use that one time key one time. So it's not, or rather I haven't analyzed it to see if it's already there,

but it's shaping up to be like the perfect forward secrecy stuff that you get from open secure messaging and those types of protocols.

So it would allow, I think, with a little bit of thought for us to build that level of security into the messaging system on top of it.

So yeah, I'll hop back into this. I've got just a couple of example use cases. The big one I mentioned already was add data to any page.

And so the rehydration cookie, as I called it, is just something that you can put in local storage.

Fireproof does this for you by default. You don't have to worry about it, but that's all it is, is putting that somewhere so that when you rehydrate the page,

it gets the right state. And so, you know, depending on your replication settings, then whether or not that Fireproof ever existed on that node or not,

it'll be able to fetch down the data and interact with it. So use cases where putting data into any page is valuable,

something like a Salesforce app or like any other enterprise app where you've built some line of business thing and it's going to be super expensive to change,

but you just need to add a couple more fields or bring in another widget or do just like some lightweight stuff on top of an existing line of business app,

there's just so much institutional inertia sometimes to making changes through the back end.

But if you can make those changes all on the front end and have the data, you know, kind of guarantees and security and integrity that Fireproof offers,

then why would you do it on the back end? So definitely a lot of opportunity in those kind of upgrading those line of business apps.

Anytime you want to do, you know, structured local data on the browser. So A, B testing or bringing up features from behind feature flags for users makes a lot of sense also. Customizable widgets.

And then, you know, kind of going one deeper, like you've got a whole enterprise line of business set of apps for your users.

And now you want to add some kind of auditing layer where you just want to take a secondary copy of the interactions and put them somewhere else

because the original apps are, you know, crufty and you don't even know what they're doing. So you want to do that and figure out what's going on with the original apps.

But really any of that kind of injectable logic. And then outside of, you know, these use cases where it's allowing you to do something maybe you couldn't do before.

And so you can upgrade apps you couldn't upgrade before. There's also just a ton of let's build something and use the right tool for the job that these kind of data structures offer.

A huge one. I mean, we talked a lot about like side chain safety and how you can link to Merkle proofs from blockchains.

But provenance tracking, which is, you know, popularized, I guess, by NFTs.

But if you take those NFT data structures and you use them on the outputs from AI models and not just generated images with text and everything,

then it allows you to get a layer of accountability for AI that's missing right now. And not just accountability, but acceleration.

Because if you know that it's, you know, if you set your temperature to zero, so you have a deterministic result,

then you can keep a rainbow table of this model, this prompt, this output, and then you don't have to re-prompt all the time.

So AI models are just an expensive version of any kind of data science workload. But if you've ever worked with data scientists,

you'll see that they often rerun the whole notebook, even though only some of the data has changed.

And that's fine. You're not going to tell them not to do that, but it sure would be nice if that rerun was instant and free.

So any kind of memoization makes a lot of sense, not just with Fireproof, but with any of these content address databases.

So, yeah, the other really interesting one, when the network is the database, is edge creation of IoT data.

So like one of the use cases I explained to my kids is, what about the, you know, the point of sale at all the Burgerville drive-thrus?

Right. So they have all kinds of analytics that they're keeping about, like, which products are selling which time of day and whatnot.

Why not put that all locally and then use the database as the network to push the predicates down to the edge devices

and get the trends and stuff reported back? People are using Couchbase Mobile for that on wind turbines and stuff,

and content address databases are a better fit. So the last one that's good for the people in this room, and something I want to talk about with the other database creators here,

is how we can do some of the mix and match. So let's say you laid down a 32 gigabyte Filecoin slab, and in the cold data you'd already pre-indexed it,

and some kind of Merkle tree, it doesn't have to be a Prolly tree, I don't know.
Then put a Fireproof UI on top of that cold data, let people remix, reorder the playlist, whatever,

pick their favorite media, tag things, and then save that back to warm storage that doesn't have to duplicate any of the stuff that was in cold storage.

So there's a lot of ability to take cold data, archived data, and Filecoin, and work with it just like it's fast without having to do copies.

And that's going to be the same for all of these databases, but I think Fireproof especially, because it runs locally in the browser.

So, yeah, it's got all the attributes that we like. One of my favorites is fork by default, I think that's going to help end users understand some of the value of owning their own data.

And the other fun part, so I started this project in like February, and it's basically been like 14 hour days hacking out,

because I want to make something that could be the most commercial database in the IPFS community.

So if y'all like these attributes, and you want to join, then what you get for joining is the database dashboard,

which is open source, you could run your own, but it's all integrated. And then you get inside of kind of like the private forums and whatnot.

So it's a dollar a month, and that pays for your first dollar of meter.
And I made a European price just for this conference.

But yeah, the fun part about that is that the dollar a month gets you membership into the forums where people are asking questions

and doing open source contributions and whatnot. But it also, although I don't have any metered services activated yet, when the meter starts to run, you know, the first dollar is paid.

So that allows you to experiment with apps, and probably most apps, like experimental size apps, are going to run for less than a dollar a month.

So it gets you in the door and lets you have fun. So thanks a ton, super excited to have a bunch of Fireproof users.

Very cool.

The actual like payloads, the diffs that go into the Merkle clocks, would you say like you're kind of like a, you're making a CRDT structure

that's basically like a CRDT of the Prolly tree, because the diffs of the Prolly tree are like the deltas that go into the Merkle clock?

You could derive those diffs, all the data is there for it, but it's more like the other use case Aaron talked about where you just put a CID in,

and you know, so the thing that's in the clock is just hanging the whole next Merkle tree off of there.
So I don't do any diffing operations when I'm running. The closest thing to like a diff resolution is that when you come to do a read, any writes that haven't been processed yet that are in,

what we're gonna do is, you know, those writes might be sitting in parallel next to each other I guess,

and so merge all those together until you have one root, and then you read off of that, and that's when we would do the conflict detection and stuff. Awesome, and you said during the last talk, during one of your questions, that it's like JSON based,

so when you make an update, when I do like you know, document.update or whatever, is it a patch similar to what Aaron was talking about, ceramic, is it some other structure that's being like encoded?

This is a super good question, no, it's like, it's just a big old JSON stringifier, it doesn't care.

For every update? What's that? For every update? Yeah, if you, I mean this is the same, in Apache CouchDB and Couchbase, I went through this and decided not to patch,

like it's just, that's not where the problem lives. It might be that I could get a little bit more compact with some of that on the browser, so it might be worth doing optimizations, it also hasn't really been optimization time yet,
but the thing that NoSQL in general optimizes for is making it so the thing that's at the end of the get or the put

is already got all the data locality done, right, so if you start fracturing that, then you lose some of that benefit.

All right, makes sense. Yeah, so what's in the database is each document lives underneath an ID,

and so you look, they live in the poly tree under the ID, and then anything that's indexed lives in the index tree under its index key.
Anytime you do a document update, so Apache CouchDB used multiversion concurrency control

with like optimistic concurrency token that was required,
and that's a really good way to just have all your users use Mongo instead, so what we did in Fireproof was make the concurrency control token optional,

and that means that your update is gonna be uncontrolled,
you're just gonna update the last write wins, but if you want to and you put that optional MVCC token into your update,
then it'll error on you if somebody else has messed with the document since then. Yeah, is there a rap video to go with this launch? That's definitely apropos. Oh, how does it go? Bacon, lettuce, bacon, lettuce, avocado, and tomato,

you don't need servers when you blotch your data with this JavaScript code in your browser,
your users get rowdy, your app gets louder when you got data blat,
when you want to do data blat, that was back from when I was calling it blat, I think it's better calling it Fireproof. That wasn't actually supposed to be a question, but I appreciate the answer.

I use CouchDB. So, you know, we've talked a lot about replication and conflicts,

and in this case calling it sync, but there's cases where you need to push that up into the application layer,
and there's other cases where if you push it up into the application layer, it's unknowable.
And so, you know, what do we do in those cases? We had an app on CouchDB that was for cable plant documentation,

and if we had a conflict, the answer was you send a technician back out into the head end

and see where that cable actually went. So I think comparing it with Git probably isn't a good idea in general,

because normally we know what to do in Git or can figure it out eventually,

but the application user may not. So are there strategies for avoiding that, you know, not having the lock, obviously,

is one way of doing it, going to last write.
Might have worked in CouchDB because they would have had documentation and eventually known that it was wrong, but how do we avoid getting there to begin with?

That's a super good question. I mean, I think maybe what Aaron was saying in the first talk about having tunable, you know,
choices for when you need that strong consistency. So in the real world use case you talked about, like imagining writing that app on Couch,

I could see how everything's great and then wouldn't it be cool if then you had a barrier you could throw,

right, before you rolled truck back away or something, you know?
And so we don't have any notion of that in Fireproof, but aside from having that transaction boundary,

which we could maybe throw additional guarantees on in the future. But I think maybe the more power is going to come out of tools for inspecting the diffs

and understanding the context of conflicts. I mean, on the assumption that as the database vendor I can't make people not write conflicts,
then the best thing I can do is make it easier for them when that happens. And so in Couch we would never throw any data away, but it wasn't like super well documented what to do in those contexts. So, you know, I don't know that there's anything inherently better or fixed about the current situation,

but I do think because of the way that the trees interact and everything being immutable, you're way more likely to be able to kind of recreate that context,

even if maybe you have to do the work to stand, you know. So Fireproof has a snapshot feature. You can give it a historical clock and it'll run from back then. So you could go ask, you know, when you notice the conflict, you could go query the whole database at a snapshot of when that conflict occurred and give you some opportunity to do some auto merging. For the transactions, are you using a car v1 or a v2? V2, I think. Do you use like the index stuff at all at the tail end? I don't need to yet, but the encryption will. So like the encrypted blocks do, I guess, but I have a clear index in my index DB

of from basically from clear CID to encrypted car file.

So you can use those car indexes to rebuild. You have to decrypt it first. Like basically you have an index that tells you what the encrypted blocks are and then you have to use that to rehydrate the CID set of the decrypted blocks. So the blocks and the, like looking at the perspective from the Merkle clock,

would you say like the head is like the encrypted block or is the head the,
like the CID of the plain text block? It's strictly the plain text stuff. So like everything that is semantic about the database lives above that line, right,
and the encryption is just part of the storage engine. So, you know, that's why it makes sense to exchange the clear blocks when you're in a trusted work group. So then like if you receive an encrypted block, it's not until after you decrypt it
and process it do you know if it's actually like a valid block in this context? Yeah, totally. Yeah, the encryption is opaque from the outside. Okay. Which means you need to keep a secondary manifest of the car files you need and all that. All right. If there's no more questions, we should all start calling ourselves Cloudless. I've been market testing it. It works. Thank you.

