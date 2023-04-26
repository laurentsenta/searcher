
# Anchors Away (consistency in an IPFS world) - Aaron Goldman

<https://youtube.com/watch?v=S5-mBa0Yol8>

![image for Anchors Away (consistency in an IPFS world) - Aaron Goldman](./Anchors Away (consistency in an IPFS world) - Aaron Goldman-S5-mBa0Yol8.jpg)

## Content

The goal of Ceramic is we are trying to build a database where you can get nice, fast, web-to-like

read performance, but still have shared composable data. So lots of people can contribute data
to the datasets, lots of people can consume the data from the datasets, but your app doesn't
get screwed because somebody else's app did something funny with the way their data is working. You can actually pull the datasets and compose them in infrastructure that you control so that you can have that reliability. Whether that counts as web 3 or not, we can

decide. So let's talk about immutable data. To me, immutable data looks like this. You

have some value. This value could be small, the number 42, this value could be large,

a cache of the current state of Wikipedia. But the value just is the value. All 42s are

equal to 42s somewhere else. I never mutate my 42 and break your application which was
relying on the fact that your 42 continued to work. But it's not always convenient to

work directly with immutable data because sometimes we want to have things that can
actually be communicated or retrieved. So that brings us to what I call semi-immutable
data. If I give you a CID, a lot of people will say, ah, you're retrieving a CID, this

is immutable data, there's only one value that that CID could be. This is, of course,
a lie. There's two values when you try to retrieve a CID. If I go to your system and say, give me this CID, your response can be, here is the value that has that hash, or I

don't know. And a server can go from I don't know to knowing the value for that hash, and

you can delete it and go from knowing the value of that hash back to I don't know. So I call this a semi-immutable state. There's exactly two states for that hash. Either I
can give you the data or I don't know. So now we've got improvements. I can give you the CID of Wikipedia rather than all of Wikipedia, and now I can transfer it in a text message
and you can go look that up offline somewhere else without me having to send all of that
data. But a lot of our applications want even more flexibility than this. You know, a lot

of artists, they make a song and they want to share it on their website, but they don't actually want to name their song by the SHA-256 of the MP3 data of their song. So we end up

wanting sequential data. So I have some particular name that I've bound to the data, and at

some point in the future, I say, hey, I want my tag that used to mean red to now mean yellow.

So I can announce that to the world and people who are following my tag, it goes from red
to yellow. And then at some point later, I want to update that thing again and it goes to green. And we've all experienced this. When I tell someone that I have installed Firefox on my computer, I don't mean a particular moment in time. It is my expectation that

that Firefox is going to be continuously updating and continuously moving to new versions of
that data. This is what we expect from our data. I don't want a snapshot of Wikipedia
forever. I want something where as people make edits to Wikipedia, I can continue getting
that new data, but still have authenticated ways to know this is really Wikipedia and
this is the new version of that. And that works fine as long as there's only one updater.

We don't live in a world with only one mutator. We live in a world where data can diverge.

So let's say I have some social media account, you know, I'm doing a blue sky like thing,
and I've got a phone and a tablet and a laptop, and each of those are capable of tweeting into my decentralized Twitter. What happens if both my laptop and my phone go offline,

make a change to the profile and then come back online? Eventually, I'm going to have to merge that data back together. And if there's a merge conflict, we're going to have to come
up with some way to make that consistent. And in databases, there is a whole set of

things that you can do in CRDTs in this fork and merge model. And there's a whole bunch

of things that you can only get if you have a strict sequential model. So databases have

talked about consistency for a long time. If you look up strict linearizable consistency,

you can see hundreds of different definitions, hundreds of different consistency models.
But basically, they come down to if you want some kind of mutual exclusion on a resource,

you're going to need to have this sequential behavior. And if you don't need that, you
can do things with this lattice-like behavior. So how do we build systems where some of the
edits we want to do, in fact, most of the edits that we want to do, may not need this kind of strict serializability, but some edits do? So the second thing we come into is we

have to have this concept of time. If I want to do a happens before and say, all right,

if my laptop revokes the keys of my phone, and my phone revokes the keys of my laptop,

whichever one of those events happened first, you took away the permission from the other device. And whichever one of those events happened second is irrelevant because you already took away the permission of that device. The phone that's already been revoked can't revoke the laptop. So we need to put everything in order. And this brings us to this weird question of, like, well, what does order mean? And what does simultaneous mean? So if I have

these events where event two has the hash of event one and event three has the hash

of the event two, I can know that three happens after two happens after one. And in this diagram,

I don't have a happens before relationship between, say, A and two. So it's fair for
me to say A and two are simultaneous. They both happened before one. They both happened

after one. They both happened before three. And that's when you start realizing that
distributed systems are really not intuitive. Because event A is simultaneous with event

two. And event B is simultaneous with event two. But event A and event B are not simultaneous

with each other. B is after A. So our simultaneous concept isn't transitive. Which is weird to

me. Like, I learned these laws of logic. I was like, law of identity, these things are

simultaneous, you'd think they would be. So how do we solve these problems of turning

a lattice into sequential data so that I can actually get this straightened out? So we

go to recent distributed systems creator Aristotle, who taught us that time is the measurable

unit of movement concerning before and after. It turns out that if you look at the fundamental

nature, particularly in a distributed system, causality is a very real thing. If I have

one file, and it contains the CID of another file, I know that the file that contains the

CID is younger, and the file that's identified by the CID is older. If I am caused by something,

I must be after it. Cogno ergo sum. So I can get time by building this graph, and building

this graph using hashes. So now I can have this guarantee where I say I want to recover

my sequential behavior. So I have event one, and then I have event two, and I can accept

event two, and now when the blue event shows up and says, hey, I would like to turn from

red to blue, that is going to fail, because someone has already turned from red to yellow.

And when I show up with the green event, it can do the merge and say, hey, I've realized that turning red to blue never actually happened, so I'm going to rebase that, and instead of

adding blueness to my red, I'm going to add blueness to my yellow, and I'm going to get a green. So I can sort of rebase that thing in, because I've detected, oh, there was this
event that got thrown out, because it failed that compare and swap. If I was doing this in memory on one node, I can simply use the CAS instruction that exists inside of a CPU

and say, hey, read this memory address. If the memory address hasn't changed, update
it. If the memory address has changed, it fails, and now I have to do something else to try and create a new event on top of what would have happened. So we have this long history of using CAS in the local system to enable multiple threads to do consistent updating

of value. And in our distributed systems, we have the
cap theorem. The cap theorem is usually presented as under partition, you're going to have to
choose between consistency and availability. If I split-brain my system, and I allow each

side to continue making progress independently, that's very available, because the split-brain
didn't stop me from making progress, but I become inconsistent because I've gone in two different directions. If I say, no, when I go split-brain, stop the world, we're not
changing anything until we get back to one brain, now I can stay consistent, I didn't
go into that inconsistent state, but at the same time, I have become unavailable because
I can't make changes, I stopped the world until I came back together. I argue that this is not actually the right way to think about the cap theorem. It's better
to think about it as availability comes before consistency. The way that I build a consistent

database is to say, I'm going to hold all of the rights until I know that I have reached
a consistent state. I get availability by saying, you can go ahead and read stuff that
hasn't become consistent yet, but that can be a choice that I leave to the user. If I have two APIs, one that says, read the consistent state, and one that says, read uncommitted,
there's nothing that stops me from letting people continue to make changes and continue to do things in that CRDT-like merge and join world, and then at some point later, we say,

okay, now we have reached consistency. Everything up until this point in time is final, and

we have this common prefix that the whole world agrees on, and everything after this point in time is in fuzzy eventual consistency CRDT land, and eventually that stuff will

be merged, and we'll say, now I can append to the consistent tail. And that will enable
us to build systems that are available because we can build things now, and if you need to do consistent things, you just have to wait a little bit longer for consistency and really make this a choice for the application developer rather than a choice for the database. If
I want to switch back and forth between a consistent model and an available model, maybe the right way to do that isn't rewrite your entire application for a consistent database

or rewrite your entire application for a eventual consistency database. You know, if my answer
is switch from Postgres to Cassandra or switch from Cassandra to Postgres, that's pretty
heavy weight. Maybe it should just be a query type. And we can implement this by just saying,
look, we're holding things for consistency and we're making things available, and the application has to make an intelligent decision about that. And this gives us the ability
to operate on time. If you have a globally distributed system, I can make a consistent
write on memory on a single node using the CAS instruction in nanoseconds. I can get

durability where I say I know that I have a copy of this data, and if my node fails, some other node has a copy of the data. I can get that fast. I just have to get it to any other computer. If I want majority, now I start needing something like 51%. This is

where you get your Paxos and your Raft. These are much slower than getting durability because
I don't have to give it to any arbitrary computer who's going to remember my event. I have to get it to the specific computers that are the consensus group for my thing. And if I
want finality, now I need two thirds plus one and probably a three phase commit. I have
to do a Byzantine fault tolerant consensus algorithm, and those are really slow. It's going to take you something like three round trips to the consensus group to get that.
These can take a really long time, and you're just not ever going to get really fast transactions
out of that. I think seven seconds is about as fast as we're going to get for things like

global scale blockchains, dropping blocks. So the thing that I'm introducing here is

to talk about the ceramic anchor service. This is our CAS for distributed systems. How

do we make it so that we know we can do a compare and swap and turn those lattice-like structures into this is an exact set of events that happened in an exact total ordering,

and we can all agree on this is the common prefix of our system. And it's a relatively

straightforward idea. I need something that is my change over time, as Aristotle said

it. If I have a server that is publishing a new timestamp event every few seconds, and

it's just like, here's an event, it's got a hash in it. Here's an event, it's got a hash in it. Here's an event, it's got a hash in it. Here's an event, it's got a hash in it. Now, when I go to make a commit, I can just pull the most recent time event, put

the hash of that time event into my thing, and then submit the hash of my commit so that

it shows up in the next time event. And this doesn't necessarily make the time events big because I can use a Merkle tree and say, okay, I'm collecting hashes into groups and then
collecting those into groups and collecting those into groups. So even if you had millions or billions of hashes that were coming into this thing, you could have tree shaped things where you've got a thousand servers that are collecting events, and then they send theirs to the next layer that collects them right before it publishes. So the main point is you have some path using
hashes to prove that your commit happens after one of these anchors and before another one

of these anchors. Now your event must have happened in this time window. It can't have
been earlier and it can't have been later. We can use that to put events in order with

the exception that some events are going to end up in this split brain way. And for those

you just need some deterministic rule. So I can say I'm going to put these things in
order by where they appear in the Merkle tree, or I could say I'm going to put them in order by what is the hash. So if I was trying to order this particular set of events, I could
do one, two, A, B, three, or one, A, B, two, three. And those are equally valid. There

are some advantages to keeping the streaks together. I probably don't want my events to go one, A, two, B, three, because then I'm going to have split these events and I'm much

more likely to have invalidated B, which was built right off of A. At least this way, if

A was valid, I get that whole chain. All right. So where are we going to get a source of time?

We could use any source of time we wanted. You could have a server run by three box that just issues these things every few seconds, but we want things that are untrusted. But
fortunately for us, people have spent a lot of time thinking, how do we build a large-scale,
consistent system where it's difficult to mess with when things happen? These are the large blockchains. So the obvious places to look for a source of time is something like a Bitcoin

or an Ethereum. Mostly we're leaning on Ethereum because the blocks fall much faster. If I
know that this event happened between two Bitcoin blocks, that's just a lot more time
than I know this event happened between two Ethereum blocks. I don't know the exact numbers, but it's like roughly 10 minutes between Bitcoin blocks and roughly 10 seconds between Ethereum
blocks. If I have to wait for my things to become consistent, I would rather wait 10
seconds than 10 minutes, all else being equal. And since the Ethereum blocks give us this continuous chain, now I can go back and have my CAS service, collect everything into that

Merkle root, put that Merkle root into a transaction that appears on the Ethereum blockchain. And

now that acts as my source of time. And all of the events in the entire ceramic network

are now in a Merkle tree that is anchored to a Ethereum block and can be put into one

of these total orders. And then it's just a matter of playing all of the events in order.
So if my events are all transactions that are doing some JSON diff, I can just keep applying

the diffs in order. If I have something that went split-brain, I can say, well, this one
was first, so it wins the compare and swap and it gets the diff applied. And this one

showed up second, it loses the compare and swap. I'm not going to apply that diff, but

you're still going to be able to detect, hey, this thing is on a dead branch. And if you

wanted to do something like have a merge algorithm that tries to rebase that data back in, you
could build that. And we're going to probably have more tools in the future to make that
easier. Our algorithm is relatively simple. We think first writer wins. We are going to

take whoever has the lowest block height. And if you tie on black height, then we're
just going to do a random. Whichever event has the lowest hash is going to be the one that wins. And that's going to start getting things applied to the end of it. And that will invalidate some other chain. But if I've made a whole bunch of changes in a row and my first block wins, because everything else is built off of that first block, I can take that whole chain in. And because the replay is entirely deterministic, every commit is

is every transaction in that chain is either going to emit or is going to abort. So you

can use this to build an arbitrary replicated state machine and build up whatever current
state you need and know that that data is going to be consistent. So this is going to

enable you to build things like names. So we have three IDs, our DID that is built on

top of this. When you're building identities, you're always going to have this choice of
do I want this thing to be local and available or exclusive or chosen? If I just want to

use a DID key, I can generate that locally, but I don't get to pick my name. If I want
something like an email address from Gmail, I can pick what name I want, but I'm going
to need permission from Gmail to get something that is a name within their namespace. Or
I can just do tags. My parents named me Aaron. They didn't have to ask anyone's permission to...

name me Aaron. Very local, very available. You get to choose what you want. Unfortunately, there's lots of other people with the same name, and it's very difficult to Google for me. So, questions. Do people have interests where... I find

that I talk to lots of people, and it's very easy to make the mistake of trying

to do everything as local first software, and saying if you can't do it in a CRDT, and you can't do it in a duals consistency, you shouldn't do it at all. And I argue that that's wrong. There's a lot of relatively basic things that people want, where you want consistency. I came over on an airplane here. I wanted a
guaranteed seat on the airplane. If I showed up and they had said, hey, we know that you've had this reservation for this flight for months, but somebody else reserved that seat one second before you, only their laptop was closed, and they didn't open it till this morning, and it turns out you never actually had the seat on the airplane. Like, no, I want finality that tells me you have mutual

exclusion rights. You bought this ticket. You bought this seat. You get on the airplane. But at the same time, the vast majority of what our software is doing doesn't need that strict linearizability consistency. Most of our software can

actually do things in much fuzzier, much more eventually consistent ways. So I think we need to be building on databases where consistency is a thing that you ask for when you need it, or better yet, you give data constraints to
your database, and it uses consistency when it knows that you need it in order
to satisfy your constraints. You can look up confluence invariance if you want to see a lot of literature on how SQL databases can figure out when they do and don't need consistency. And let's build databases together where we have

strong consistency and we have pretty good availability when we don't need that strong consistency by thinking about consistency as a thing that happens after some amount of time, rather than a thing that happens by choosing the right database. Thank you. So you touched on it at the end there about not wanting your flight to get

unbooked on you. Yes. But in general, what do we got to do to, like, on the user

facing metaphors side, like, we get, we know what it's like to have our pull requests not merged yet, right? But like, do applications need to bring those
metaphors to users or they just need to be, have good judgment about when to go strongly consistent? So it's some of each. There are some times where you know you

need strong answers and sometimes you don't. If I am revoking a key, I'm pretty

sure that that user wants a real strong consistency event for, I made it so that

this person is no longer allowed to see my photos, and then I added a bunch of photos to this album. You really want those events in the right order. But for

a lot of things, I think you are going to want to show that to the user. So I had a
Palm Pilot in the 90s, and whenever I created an event, it was a white

background with black text. And when I plugged it into my laptop and I synced that event, it flipped to a black background with white text. And it just
had visual indications to me for, this is an event that lives locally on your Palm
Pilot, or this is an event that has already been synced to the desktop and you know you're not going to lose and is out there. I think a calendar app could pretty easily be like, hey, you have a pending reservation for this room, that's
in gray text, and at some point later, whether that's 20 seconds later because
you're waiting for a block to become final, or whether that's hours later because you're disconnected from the internet and you just can't get to the consensus group. Those things can appear as, hey, you have a fuzzy, not final thing

that you have done, and you can see your changes and react with it and have a good real-time experience, you know. I would much rather edit a Google Doc and have some of my text indicate to me, this is locally on your machine and not synced yet, than have it say, oh, you're not online, let me lock that document

for you so you can make no changes to your slides while you're on this plane for six hours going to a conference. Like, no, actually, it would have been okay for Google Docs to let me make changes to these slides while I was on the airplane and sync them later, as long as it communicated to me, you've got data
that exists only on your laptop. So I do think that you want to have these metaphors at least possible to expose all the way up to the user and give them guarantees of you have a candidate new truth versus you have a final new truth.

And I would probably avoid going too deep. For developers, I probably want to
give them APIs where they can say, this data is local, or this data is durable, I at least got it off your machine, but it hasn't been guaranteed consistent yet. Or this data has reached majority. As long as no node in the network lies,
you can keep going. So that's going to enable you to get more performance if you just assume the other nodes in the network aren't lying, you probably want to do optimistic concurrency control on majority, because most things will reach finality eventually. But you don't want to tell the user, hey, you have this
ticket on this airplane until you know that things have really reached finality. So the right API to show to a developer may have more layers here. I think the right API to show to a user is a very strict, this thing is final, or this thing is not final, and pending changes and conflicts, and we'll see what happens. So using Ethereum or Bitcoin or, you know, an equivalent like system, those

providing probabilistic finality, what happens in those environments where
like, there's a reorg on the chain or blocks become orphaned that you've then
anchored your time commits to and you end up in a split brain situation where like, some events have been anchored to like one part of the chain, and other events have been anchored to the other part of the chain, like eventually, you know, that needs to be resolved. How's that work out in the ceramic world? So if you had data that was anchored to blocks that eventually got unwound,
those events didn't happen. So they are going to be in a state like this blue

event. And it's just going to be like, okay, this thing didn't get anchored.
If your application has application specific logic to try and detect, I have
an unanchored event, you could try and fix that up. We need better APIs at

exposing that and letting you fix things up right now. And then you would have
to create some green event here and be like, okay, this is event three, I'm going to collect all of my events that got pruned and re merge them together.
All right. I gotta admit, I really enjoyed watching Loki, because it's like,
haha, you're explaining to the society the timelines diverge, and then some of them get pruned off, and now they're gone. And I'm glad that Marvel is making
entire TV shows to explain consistency models better to people. A very open question. But if on your airplane, you would have had the time to add more slides, what would you have done? If I had more time, I would have talked more about ways in which you can get those mergings to be more automatic. So right now, most of the documents that
are in Ceramic are a pile of JSON patch. And the transactions are just apply
the patch, apply the patch, apply the patch, here's your JSON object, that's your current state. You might want to have much richer reactions here, where
you have something like run this particular piece of JavaScript or other

VM byte codes that is going to read the state and mutate things and write new states and do richer replicated state machines. There's a lot of things that can be done with CRDTs, where if I don't have this signed event 3, maybe I

could say, oh, I'm going to look at all of the pruned realities, and if I have deterministic rules for merging them, maybe I can merge them on the client and say, yeah, I've got a bunch of signed mutations, but you went
down one route that says I made a tweet from my phone, and you went down another route that says I made a tweet from my tablet, and those are all different tweets that happened at different times. A client should be able to do that merge and essentially create this green event 3, even

though that event hasn't been made and signed by the actual owner of that

data stream. So to put an event in a data stream, someone has to sign it, but you could have virtual events that are made by just the client. So you could sign it, but you could have virtual events that are made by just taking all of the tips that have been pruned and say, yeah, I'm going to

run CRDT merge on any of the data that is capable of CRDT merge, rather

than just the I have to get everything into one at a time right now. I
think Ceramic is a little too aggressive of everything needs to be in this linearizable, ordered set of events. Everything that is in the

candidate tip is basically just a candidate tip. There is no sense of I'm going to pull information in from candidate tips that never actually got merged into the chain in order to build a richer state. There will be a difference between committing the entire state of the object, which is what you're doing now, or then describe an action to
change the object and then commit that. Most of the ceramic streams that exist right now, the thing that is in each of the events is essentially a
transaction. It says run this piece of JSON patch on the current state, and
it will turn whatever JSON you had as the previous state into the JSON that's the new state. So the thing that is being stored is the diffs, and you just replay all the diffs to get the current state of the object. You just as easily could make something that worked more like a register that was last writer wins. So for example, if I was making the current state of
Wikipedia over IPFS stream, I wouldn't put all of the diffs into a giant

JSON patch. I would just be like, my rule here is here's the CID for the
current head of Wikipedia over IPFS, and when I had a new change to it, I
would just create a new event that says, hey, here's the new CID, and would just essentially hang a Merkle tree off of each event, and then you wouldn't replay from the beginning. You just say, what is the latest state? So it
makes sense to use this either in the I've got a whole pile of diffs, or in the I do a roll up, and every time I have a new event, my new event is here's the root of the new state. And with advanced data layouts, I think it's going to be relatively cheap to have. Just look at the front thing that leads to some Merkle tree that's got potentially an entire file system. I kind of want to make a stream type that's web-native file system, so that it's really easy to have the mutations be happening as a ceramic stream, and have all the data be in web-native file system, and get all of that fusion goodness. Yeah, that example of making a web-native file system stream is totally

one of those things where we can take advantage of the fact that we all know how to parse each other's Merkle trees, and do useful stuff to remake these technologies later. My question, oh, and I guess maybe a comment on the consistency model there. The one Fireproof does is every update is like, let's say you update document

XYZ, and somebody updates document ABC. Concurrently, you're going to end up with two clock routes, two clock heads, and the read on a clock described by

two clock heads is just a totally normal thing to do. So it reads from that state, it merges dynamically, and the next update will put those clock heads together into one. And what do you do if they conflict? If they're both on the same key? Yeah. At this point, I hang on to it and let you deal with it later. Okay, so it's not last writer wins. It's, ah, you have a conflict, do something. Yeah, so it's the CouchDB model. I mean, Word for CouchDB works for Git. Most developers that I know are much happier that Git says, hey, two developers changed this file, how would you like to merge it? Rather than just being like, hmm, developer B's changes
will stay, developer A's changes are gone. Bye. Consistent now. Okay, so then the question I had is, let's say we, well, okay, you talked

about the consistency monitoring thing, like, like, if you're an app, you want to tell the user that it's finalized or whatever, how, how do we build that

sort of monitoring, like, in the IDFS immutable distributed way? Or is that

become like more of a service specific thing? So I think that is always going to be fairly tied in with who is signing the
time events. So if I have something that is, if I can boil my consensus group

down to a signature, so let's say I had the dumbest possible time server, a
single server that you give events to, and it just signs the event with its key. And I'm trusting that it's not signing conflicting things, but it's actually going exactly in order. Now I could throw that entire pile of objects

into IPFS and simply by checking the signatures that are on the timestamp

events, I can say, yes, I've got all these things and I've got them all in order and I can figure out what that dependency graph is. So I don't actually need to talk to the time server to do that. It's a little bit more complicated if you're doing something like Ethereum, because the question that you're always asking is, is this Ethereum block in the thing that is
the current Ethereum head? And because Ethereum could always reorg on you, you have to go to the Ethereum network and be like, hey, Ethereum network, what is

the current head of the network? And more specifically, is this particular
transaction that appears here, because I've got some timestamp event that references an Ethereum transaction, that that transaction is in fact in the

chain. And at some point you're going to have to go check that, because that is the source of truth. Or you can decide that you're more trusting than that and hit the anchor service and just say, hey, anchor service, did this particular
event get anchored? And it can tell you yes or no. So it's a question of how much trust do you want to give it? Maybe the right answer is send 9,999 out of

10,000 requests to the anchor service, and then 1 in 10,000 requests actually go to the chain and check it. Because over time, if it's lying to you, eventually you're going to catch it and be like, haha, you're a lying CAS service. You said this was anchored, but it wasn't. I'm telling all my friends not to use your CAS service anymore. So that kind of gets into what my last question was going to be, which is like at the most consistent, they turn the knob up all the way. I guess what kind of anomalies would Jepson find? And it sounds like maybe what we'd hit is we're

only as good as the write-ahead log or as the Ethereum chain would choose to
use. Yeah, so Jepson is trying to test, am I actually doing the strict

serializability thing? And if I'm doing something less than that, the guarantee
we're giving here is sort of common prefix. So to be strict about it, you say, okay, anything that's not final, we're going to say. That doesn't count. Jepson, don't look at those. That's sketchy land. We don't want that. If you counted stuff that is in this blue space as this is a failed
transaction, then you would potentially do very well with that. Now, despite the

fact that the model says you could totally get the guarantee from Jepson that this was strongly consistent for anything that has been named final, I am

confident that our current implementation would not get the gold star from Jepson. They would be like, oh, you lost some particular write that you thought
you had, particularly because we are a distributed network. So if I have an

event and I anchor that event and that event exists on my server and no one
else in the ceramic network decides to mirror it, and then I go out of business and my events all vanished because nobody else cared and nobody else
mirrored it. All of that stuff just did the semi-mutable thing and went from
this state to I don't know. There was some data here. Here's what the hash of

the data was. If you had it, I could tell you you were right. Good luck.
Clearly, Jepson would say this was a violation because I had a bunch of events that were final but then got mutated in the semi-mutable way from

here's the final state of that data to I don't know. Right. I mean, that's also a little bit like Jepson saying you did a bad job because you turned off your S3 bucket or something.

Right. But I think we are a little more subject to that than most databases

because while the general idea behind Ceramic is all of the information you care about you can bring to your server and you can cache and you can provide availability for the stuff that you care about being available. If you don't do that I can totally imagine people being like, hey I want to read weather data from someone's app who's got 10 years worth of weather data for the whole world. You're only going to sync in the data points that you read once. You're not going to read the entire history of weather for the world. So you might find

that you have systems that are surprised when all of the sudden your data source
went away because you are relying on somebody's server that you didn't realize existed. So we have maybe like five more minutes left. If you were an

airline building a booking system using this, if they came to you and

they wanted to start building that today, would you suggest that they run their own timestamp server because then that puts them on par with
the guarantees they're used to or would you suggest that they go in the Wild West and use a public blockchain? I think for an airline server they would want

things to be a little bit better installed.

guarantee of running their own timestamp server because they really don't want

þings to revert on them. If there's going to be a reorg they want it to be their reorg? Yeah, basically, but the airline is in some sense a less

interesting case because it's not as contested of a resource. If you had

several different organizations that were sharing something, so let's say you
had a bunch of airlines that were trying to coordinate on who gets to use which gate at which time or who gets to use the runway at which time, we don't want there to be any lines on the runway. We want the planes to not pull out until they know that they've got a spot on the runway. You could coordinate the scarce
resource of the runway using a system like this and then you're probably going
to want to have a timestamp that is a consensus group between either a bunch of the airlines or some public source of time. Okay, so in that case you might run
a private blockchain because then when it reorgs at least it's your reorg? Right. And again, you can get real finality if you have a closed set. If you

know there are exactly 27 airlines that fly out of this airport and we're using
2F plus, we're using 2 thirds plus one of them to consider something final,
you're not going to get reorgs. It's only in a permissionless blockchain where
anyone can join that you can't ever get real finality because what does it mean
to have two-thirds plus one of everyone who's a Bitcoin miner right now? Do you

know that there's not some secret giant mining pool that none of us have ever heard of that has just been created because, surprise, we want one day to cause a bunch of reorgs and they've been mining blocks on their own chain for a year and are gonna undo the entire laxity plan? We can't know that that
scenario doesn't exist in a public blockchain. Whereas in a permissioned blockchain where you say, look, these are the nodes, we have two-thirds plus one of them, this is final, you could have real finality. And anchoring obviously gives
you no better finality than the finality of the thing you anchor to, you know? If
you really strongly securely anchored yourself to a flimsy door, like, okay, no

matter how well you anchor to something you're not gonna get more temporal guarantee than the thing you've anchored yourself to. Alright, thanks Harry.

