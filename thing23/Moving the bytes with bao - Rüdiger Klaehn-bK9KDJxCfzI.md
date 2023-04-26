
# Moving the bytes with bao - Rüdiger Klaehn

<https://youtube.com/watch?v=bK9KDJxCfzI>

![image for Moving the bytes with bao - Rüdiger Klaehn](./Moving the bytes with bao - Rüdiger Klaehn-bK9KDJxCfzI.jpg)

## Content

Okay, yeah, let's just get right on it. So I'm Rüdiger from Number Zero, and I'm going

to talk about BAU. It's kind of a mix of hashing algorithm and a data transfer protocol that

is related to the Blake 3 hashing algorithm, and I'm going to try to explain why this is so awesome. So let's just get on it. First of all, there was a decision at Arrow

to kind of rethink things from the ground up, and well, we tried to keep things extremely

simple. The reason for that is that we are a very small team, and we want to put things
into production as quickly as possible, so we cannot afford a lot of complexity. And

this is kind of our guideline to make things really as simple as you can. So like, if you take one thing away, it just doesn't work anymore. And okay, so the outline is first

basically what primitives does the new Arrow offer? Then what do we want out of this? How

does BAU enable this? And then some applications where this works very well, and some other

applications where it doesn't work very well. And last but not least, a discussion about whether this can still be considered IPFS or not, because as you'll see, it is quite a departure to some things. But let's just see what it does. Okay, so what primitives
do we have? So the project was started very in the beginning of this year, and it's already

used in DeltaJet, and so there is not much in terms of primitives. So let's see, what

do we got? We got blobs. So blobs can be arbitrary size. You can have a blob that is one byte,

you can have a blob that is one terabyte, it doesn't matter. If it fits on your hard disk, it can be an Arrow blob. So there's no limit of four megabytes or whatever. And

we use the file store pattern by default, I mean, people that use Kubo are probably familiar with that. It means that the file itself stays in the hand of the user, so if
you have a large data set, you might want to work with the data set and still provide it to other people, and then it's quite useful to have the data available at the same time as sharing it. Like if you have a big machine learning data set and you want to play with it, but you also want to share it, then it's not good if you have to have it twice on your disk. And this is the default for Arrow, we don't do anything else. Then we have a concept

called collections, that's kind of the smallest thing we could come up with to combine multiple

blobs. And what's a collection? A collection is a blob, it's clear. We only have blobs,
so collections are just blobs. A collection can be thought of as a sequence of links with

some metadata. There's some discussion about whether we need that, but currently there's some metadata. And a collection, just like a blob, can be as big as you want. So if you
want a collection with one billion links, it's no problem, you can do it. It's like 32 billion bytes, it's no big deal. And typically links go to blobs. You could have links to

collections, but so far we don't do that. So far we just have the two-level hierarchy
a collection contains blobs and that's it. And like Hannah mentioned, no DAG, no problem.

And we use, this might also be a bit controversial, we use the same hash algorithm for all links in a collection. And currently the only hash algorithm that we support is Flake 3. So we

get to why we do that. So what do we want with these two primitives? We basically just

want one thing, which is verified streaming. Meaning we want to send content over the wire

only if it was requested due to a hash, and we never want to send something which does not match the hash. And we want incremental verification, meaning that you should not

have to download a huge amount of stuff. You should notice very early if somebody sends
you wrong data, either due to a bit flip on the wire or due to malice or whatever. Should

be able to notice that immediately. And we want to validate on send and on receive. So
validation is good, let's do it everywhere. So this is a typical collection. This is one

data set I've been playing with a lot of times. And it's a typical machine learning data set.

It has some very small files, there's 100 bytes, and some very large files, 12 gigs.

And we don't need to split this up into a tree or anything, we just have the collection and the blobs. I mean, there are still trees, you'll see, but they are kind of an implementation
detail. Okay, so now let's take a look at what we want to do with this collection. So

the data transfer protocol. So first of all, it is a request response protocol. So all

this ceremony about who is it from, who is it for, and so on, it's all taken care of

because it's request response. It is specifically not a discovery protocol. So I actually go

to go to great length to make sure that nobody can abuse it as a discovery protocol. It's just about, you know, the other side has the data and you want it and you want to say, I want this data, give it to me. There will be a discovery protocol, it will look similar, but this is not a discovery protocol. And again, verified streaming, and on send and

receive. So what, what are the possible things that people might want to do? So the first
thing, of course, you have a hash and you want the data. That's what bitswap does. So
you have a hash and you want all the data that is behind that hash. And that would of
course be a bit limited if you have a giant piece of data. So the next thing we want to
support is the ability to say, I want some ranges of this data, like the, you know, HTTP
range request, where you say, I have a big asset and I specify which part of the data I want. And that's one thing we support. And we support not just one range, but multiple

ranges. So whatever complexity of the part of the data you want, you can specify it.
Okay. Then once you go to the level of collections, the most obvious thing you might want to do
is you want the entire collection. So in these diagrams, blue is always what you already have and green is what you want. And this is the simplest case. You have a hash and

you want everything below this hash. But it turns out that this would be very limited
if this was all we could do. So I got a few other scenarios. So this is resume. Basically
you have made a download and you were interrupted in the middle. And then you basically got
the collection itself. You've got a bunch of files, but you were in the middle of the second file, you were interrupted. So you want exactly the rest and nothing more. And
you need to be able to specify, I want like the second part of the big file, number two and the last file. So that's one thing the thing needs to be able to support, but it gets more complex. So this is repair. Imagine you have a big data set, but for some reason
you have some breakage, a bit flips, or you deleted some files or whatever. And you want

to be able to specify, give me exactly the blocks that repair this data set to make it
whole again, to make it conform to the hash again. And then you might get some very complex like ranges. And this is another scenario. This is multi-party data transfer. You have

a big data set, you want it, and you have multiple peers that announce that they have
it. And then you want to kind of shift the load on the different peers. So you say, you do first half, you do the other half, and then you might get some sort of stripe pattern or whatever. I mean, how exactly this looks like is up to the higher level algorithm,
but this is basically what you might end up with. And so these are the scenarios that

we need to support. So how does a request look like? A request contains a root hash,

of course, it's content addressed. And then it contains ranges for each element. So the

collection, as I mentioned, is just a blob. So we treat the collection as first element.
And for the collection, just like for any other blob, you can specify ranges. So you
can say, imagine you have a collection which contains a billion links, you might not want to download the entire collection. So for the collection itself, you can also specify ranges. And then everything basically in the order in which the links appear in the collection,

you specify then ranges for each element. And this is a little bit compressed. There
is a range encoding, which basically compresses multiple ranges. So you end up with smaller

numbers. And then there's another encoding, which encodes multiple ranges. But well, if
you are interested in deep meta-tails, then ask me in the hallway or look in the repo.
But let's just say it's very compact. And now, once you get to the response, so what

is the response? The response is just the data that you've requested in a certain order.

And the natural order is, of course, the collection first, and then all the elements in the collection
in the order in which they appear in the collection. And well, OK, so let's see what the response

is. It's just the concatenate response for each non-empty item. So if you have an empty

item, you don't need to send anything. The requester knows what he requested. So you

can only make sense of the response if you are the requester and know the request. That's
important, but you can do it because it's a request response protocol. You can assume in a request response protocol that the requester knows what he has requested.

And now, OK, you could say, just send the bytes, but that would be pretty weak because that would not be verified streaming. That would mean you would have to download everything and then hash it once it is on your disk and then find out, oops, I downloaded the wrong
thing. And so that's exactly when BAU comes in. So what is BAU? BAU is basically an extension

of the Blake 3 hashing algorithm. The Blake 3 hashing algorithm is relatively new and
relatively fast hashing algorithm. There are some controversial properties, but I'm not
going to go into them. But the main thing is it is a cryptographic hash function. And

it is using an internal ephemeral Merkle tree. This happens to be a binary Merkle tree that

is kind of left balanced. And it has the smallest addressable element is 1 kilobyte chunks.

And now this sounds pretty abstract, so I got some pretty pictures. This is a Blake

3 tree. So below these white rectangles are the content. And then basically whenever we

have a chunk, so here we have two chunks, and we compute the hash for each chunk, make
a hash pair out of it. And then the hash of these two hashes is the hash of the entire

tree. And this is how Blake 3 works internally. So for a single piece, it's very simple. You just hash the thing and that's your result. But as soon as the tree gets bigger, you build this internal tree. I guess for another audience, I would have to spend a lot of time explaining
this, but here I think it's pretty common, this kind of thing. So you end up with some

kind of left packed binary tree. And as you can see, you have only two things in this
tree. You only have chunks and hash pairs. So hashes always appear in pairs, and these
hashes are 32 bytes, so you always have these 64 byte hash pairs. And so this is how the

tree looks when you have a larger number of chunks. As you can see, the thing above is

the final hash. This is what you get if you just do Blake 3 hash. And all the intermediate

nodes are ephemeral. They are just used during the calculation of the final hash, but they're
not stored anywhere. So you've got a bunch of data, you do this nice tree, but then you
throw away the key and just keep the result. That's what Blake 3 is. And I think Blake 3 is supported by Kubo, but only in this mode where you can just hash the whole thing. But
if you do that, you're kind of missing out because BAU, what is BAU? BAU is exactly the

same as Blake 3, except... So it is Blake 3. You get the same hashes, but you persist

the tree. And that means that you can save some calculations, so you don't have to build

the tree every time you need to do something with the data. And there's two ways to use
BAU. One is basically you store this tree in a single separate file in a certain order,

and this is referred to as an outboard, or you kind of mix the hashes with the data.

And we are using the outboard version because we want to leave the original data unchanged. You want to be able to use your data while you share it. And so here we see this is basically

Blake 3, except that you keep the tree. And as you can see, the outboard file is just
a list of hashes, of hash pairs to be precise, with the size prefix. That's all there is
to it. There's nothing else. No metadata, nothing. And the outboard obviously is much,

much, much smaller than the data itself. This is not the scale. And in what order is the

stuff stored? Well, you basically store it in exactly the order in which you would need

it to verify it. So the first thing you store is the two hashes immediately below the root,

because these are the hashes that you would have to hash to check that it actually matches the root. And then, and so on, and so on. And this is called the depth-first left-to-right
preorder traversal. And this is exactly how the outboard is laid out. So the outboard

is a single file. It is not some database, not some giant complex thing. It's just a single file where the offset can be easily calculated based on where you are in the tree.
Okay, so now we got this great tree. How does this enable us to do verified streaming? So

we don't want to do verified streaming of the entire thing. We also want to be able to do verified streaming of ranges, small and large ranges. So let's take a look. So

on the provider side, you get the request, and you figure out which tree you need. That's

pretty clear. And then you basically have to look at the tree and traverse only the

relevant part of the tree. So everything in the tree that is not relevant for the query, you can just ignore. Which also means that if you don't have that data, it's no big deal.
It's only a big deal if you don't have any of the data that was requested. So you can share data which you have partially downloaded. And the traversal, again, is in preorder.

And then all you need to do is you basically take data out of the output file, take data out of the data file, mix it, send it over the wire. That's all you need to do. It's as fast as you could possibly imagine. So this is how it looks. Here we got a tree.
It's not a perfect binary tree, but it's kind of left-packed. That's how they always look.

And down there, we got the ranges that we want to request. So the smallest unit in BAU

is a chunk. Chunk is one kilobyte. So the first thing we do is round up the ranges to
chunks. So we get one chunk on the left and two chunks on the right. And the next thing,

we need to figure out which part of the tree actually is relevant for this query. Just throw away everything that is not relevant. And then we just need to traverse this in

the right order and send it over the wire. That's all we need to do. And this is the order, obviously, first to root, because that's what the remote needs first to validate. And
then we basically make a path from the root to the leaves. And this is the order in which

we need to send the data. There's not much to it, basically.

There's one thing we also validate when we send the data. The reason for that is that

this is data that is in the user directory. And the user might have changed it. So we don't want to lie. We don't want to send wrong data over the wire. So we validate on send. And if the user has changed it, I mean, we tell people, don't change your data while you share it. But people will do it anyway. So if you verify on send, we will notice.

And now, this is kind of interesting, what happens on the requester side. There is no

metadata. There is no header. Now comes the hash pair. Now comes the chunk or anything
like that. But the requester knows exactly what to expect, because the requester knows
the shape of the tree. So the requester knows which ranges it has requested. And the requester

knows the shape of the tree. So it does the exact same thing. It does the exact same tree traversal. So first of all, it reads the size. Then it knows the shape of the tree. And then

it does the exact same thing as what is done on the sender side. And the tree iterator

kind of tells you what to expect. OK, the tree iterator says, now comes a pair. Then you read 64 bytes, treat them as a pair, validate them, and so on. And the tree iterator tells
you now comes a chunk. Then you read a chunk, validate it, and do whatever you want with

it, basically. And you will detect corruption after one item. So at most, after one kilobyte,

you will detect if somebody is lying to you. OK, in summary, so the provider traverses

the tree, reads from data and output, validates, and writes to socket, whatever. We use QUIC,
but it is not a part of this talk which exactly the transport is. And the receiver does basically

the same thing, except the receiver side doesn't have the data yet. It traverses the tree to

know what to expect. Then reads from the socket and validates. And then it uses the data in
whatever store or process or stream to the video or whatever. OK, performance. So Blake3 is a very fast hash function. But that's not the main thing.

The main thing is how this all is optimized in terms of data layout. So no matter how
big your data is, you're only dealing with two files. You're dealing with a very small file which is called an outport and a possibly very large file which is your actual data.

And in these two files, you have a very sequential access pattern. If you request the whole thing,
it will just read the files from the beginning to the end, both the outport and the data.
And if you have a range request, it will only seek forward. It will never seek backward.

And it will only seek forward once for each gap, basically. And for the outport, it will

also only seek forward. And in the worst case, it will seek forward the number of levels in the tree. So also no big deal.

And this is as close as I mean, this was very important back when we were when we had spinning this, but even solid taste state this like if you have a predictable access pattern, because they also have caches and so on. And they like if if you behave in a certain way, because then they can prefetch stuff for you. So this is really in terms of IO. This is as close as to optimum as you can get. And currently, our performance limit is quick and encryption. It's the

actual transfer itself. I mean, the computation itself is almost free, basically. And we can

validate on scent because it's so cheap. And there's another talk called measuring on the
fast track. I think it's happening right now. So if you want, you can watch the replay to see the performance numbers. OK, now let's talk about the overhead. So in the best case,

you have a giant file and request it all. The overhead is one two and a fifty six of

the data size due to some things we've done. And so for one terabyte file, you would have

a four gigabyte of overhead. And this overhead consists entirely of the hashes. So there's
nothing else. No, no, like headers or whatever, just hashes and data. That's all this. And

the hashes are one two hundred fifty six. Now, let's talk about the worst case. The worst case is you have a giant file and somebody requests a single chunk. In that case, you
have to kind of build a path from the root to this chunk. And that means the number of

parent and a node. So the number of hash pairs you have is locked to the number of chunks.

And so to put a number on it, it's like two kilobytes. So this would be interesting if somebody has a large file, one terabyte file and announces it on the network. And you just

want to probe whether it's actually true. Like you ask for a random sample of the file
to see if this party actually has the data or not, or if you want a tiny bit of the data,

I don't know, random access, whatever. And in both cases, the overhead is not very large.

Totally acceptable. But to get to this point, we actually had to modify the bio algorithm

a little bit. So we are doing a thing called chunk groups. So the original bio algorithm

has the tree built for every chunk. So average for every chunk, you have a hash and in total

you have two hashes per chunk. So you have 64 bytes for each kilobyte, which is one sixteenth,

which is a bit much. So if you have a one terabyte file, you would have a 64 gigabyte output, which would mean that you could not keep it in memory. And we want to keep it in memory. So we did something, we implement something that actually the original author

also thought about. We only store higher levels of the tree. So let's take a look how this

works. You have this tree and now I've numbered the levels. So you've got level zero are the

leaves, one leaf for each two chunks and number one, number two. And now you can basically

treat the lower levels as ephemeral and the higher levels as persistent. And if you do
this and this, as you can see, your outboard gets smaller and you can afford then to keep

your outputs in memory. I mean, there is of course also a downside because you have to, if you want to do something within one of these kind of chunk groups, you have to recompute
the hashes. But as I mentioned, Blake3 is very fast. So recomputing the hashes only
has to be done for queries that are so small that they are below one chunk, below one chunk

group size. And it's very fast. So don't worry about it. Okay. Applications. So what can

you do with this? One thing we are doing already is the database sync on mobile platforms.

So basically we have also a talk called Data Chat and Arrow. The application is that you

have a database on a phone of a chat app and you want to sync the database to a new device.

That's one thing that's already in production. And the next thing that I really like is machine learning datasets. They really have a mixture of small and very large files. And for them,

this works very well. And large file storage would also be an application and game assets,

as mentioned in the talk before, and directory trees. We can also do very large directory
trees because collections can contain an arbitrary number of links. And we are mapping deep directory

structures to a single collection. So there are no collections in collections. You just have one big collection, which is the entire directory structure. And you can also share
disk images. If you have a giant disk image and you want to get it on a different machine, this is one of the fastest ways you can do it. And here I'm basically, I got a demo of

adding this Lama 13B dataset to Arrow. This is not network. This is just building of the

outboard. And as you can see, it's pretty fast. It's more than one gigabyte per second.

And it's just very smooth. It doesn't even cost much CPU. I mean, your machine won't

slow to crawl with this because it's just not doing that much.

Future plans. So wait a second. There should be a section about where it doesn't work well.
So, future plans are, one thing you can do is if you have a collection or blob and you

append data, you can reuse most of the three. So you can think about, let's say, a log file,

log file which is append only, and you want to share the log file. An application would

be basically you have already transferred most of the log and then you compute the new hash for new log and basically transfer only the delta. This can work for blobs and for

collections. And then one thing that is a little bit further in the future is arbitrary write. So you have a file and you write anywhere in the file. You recompute the root hash and
then you basically send this over the wire and basically only need to transfer the delta.

So imagine you have a database, a live database, which is a single big file, and this gets modified all over the place and then you can sync that to another place with very low overhead.

And so this is dbimages and you could even think about having a complete mutable file
system that you sync on another device. So you could sync your def, STA, whatever. But

single writer is something we are going to keep. We are not going to tackle multi-writer. That will be a layer on top of this. Okay, limitations. There it is. So where does this not work very well? So if you have a

deep and highly dynamic DAG, this is not going to work well for you. I mean, there's nothing
fundamentally that makes this slower than anything of the existing stuff, but we encourage

people to have flat hierarchies because it's just so much better for transfer. And so ribbon

chunking, for example, we don't support. Our chunk size is always one chunk, is one kilobyte.

And probably trees, there could be some ways to make them work, but it's going to be a
little bit difficult. And so you can put collections in collections. You can create a hierarchy

as deep as you want, but currently we don't have a garbage collection that is aware of this. So if you have a garbage collection that is not aware of it, it would just throw away all your DAG except for the first level. And that would not be good, obviously. But

I'm going to make the case that actually this carrying that much about these very complex
DAGs is not as common as you might think. So here's the Linux kernel. I just took the Linux kernel as a car file and did some analysis on it. So the total size of branches is about

four megabyte and the total size of all leaves is 1.2 gigabyte. So the branches are just

a tiny fraction of the leaves, meaning that if you have a way to transfer the leaves in
a quick way and put the rest in a single collection, you're not going to lose that much unless the data set is extremely dynamic. Here's another example, Wikipedia. I didn't take
the whole Wikipedia, but I took a small subset, just the math part, just so I could compute some stats. Branch size is three, whatever, 32, 33 megabytes. And leaf size is again a

factor of 100 more. So all this DAG stuff only happens in this 1%, which is the branches.

So I would argue that for many, many use cases, just forget about all this stuff. Put your
leaves, make sure you can transfer your leaves quickly and put all the DAG stuff just in
a single collection. And here's the data set where it doesn't work. This is something from Filecoin and there you have a small test data set from Filecoin. And there you have the

vast majority of the data is in branches. So this would be something which we just cannot do currently. OK. So now, it's too late, but the question is, is this still IPFS? So if

you take the view that IPFS is something that has all these things or that has the majority of these things, then this is definitely not IPFS. No way. We have equivalents to most

of these things, but we don't have, I mean, this is not what we are doing. But if you

take this wider view, IPFS is everything where content addressing is baked in at the very,
very low level and everything works with content addressing. Then most definitely what we're

doing is IPFS. Because there's not a single byte going over the wire that is not in some
way related to a hash that came before. You do a single bit flip anywhere and it will
immediately blow up. So content addressing, every single byte that goes over the wire
is content addressed. And we got incremental verification at a very, very fine-grained
level, even more fine-grained than UnixFS, by the way. Because in UnixFS, you will download
a directory block which can be up to 200K in size before you notice that it's wrong.
And in our case, it's on chunk or chunk group level. So it's much, very, very fine-grained
incremental verification. And we don't even trust the file system because it's in the

hand of the user. So if the user modifies a file, we have to check whether it's actually
still the same. So we even validate the stuff that we read from the file system. And that's
why I would argue that it is very, very much in the spirit of IPFS, even though it doesn't
have some of the primitives. And that's it.

So questions? Or do we have time for questions? Hi. We worked on some BAS stuff last year, so I'm really, really excited to see this going into prod. The thing where you throw out the lower levels is smart. We were about to change the chunk size, which is bad, obviously. So my question is for support for mixing this

with like RABN content-defined chunking. Have you guys put any thought into the different
options? Like you could decide to throw Blake 3 out. You could RABN chunk and then BAU.

You could RABN chunk the inboard BAU format. There's a lot of options and all of them are kind of bad. Are you guys just not going to do content-defined chunking? Yeah. So like I said, this is in the big topic of DAG support. I think we have some ideas

how to do it. So I mean, transferring the individual RABN chunk pieces would still be
very fast with this. The thing is that we are a small team and we are really going to

look for applications where we have revenue potential. Let's put it that way. And so if

there's something where we say, if we can make this little piece work, we're going to have revenue. We're going to do it probably. But we're not going to do something just in
case somebody might need it at some point in the future. So that's all there is to it. How exactly we're going to do it, I can talk to you later. It's a bit more complex. Okay. So thanks a lot.

