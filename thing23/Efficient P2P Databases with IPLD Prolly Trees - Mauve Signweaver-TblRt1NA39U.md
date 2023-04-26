
# Efficient P2P Databases with IPLD Prolly Trees - Mauve Signweaver

<https://youtube.com/watch?v=TblRt1NA39U>

![image for Efficient P2P Databases with IPLD Prolly Trees - Mauve Signweaver](./Efficient P2P Databases with IPLD Prolly Trees - Mauve Signweaver-TblRt1NA39U.jpg)

## Content

Hi folks, thanks for coming out. My name is Mauve, my pronouns are they it, and today

I'm going to be talking about peer-to-peer databases and how you can build them on top of IPLD Prolly trees. Now, before we get into peer-to-peer databases, let's talk about how

regular databases work and how they can serve data so fast. At the core, most database engines

consist of a query layer that sits on top of collections of data, and indexes over that

data which speed up searches. So, let's talk about the indexing bit. What does it mean

to search through data without an index? To start, if you want to search for data related
to a given query, you'd need to search through all of your data in order to know if you satisfied
that query. What's more, if you want to perform a sort on your data at the same time, you'll
need to do that in tandem with filtering and buffer the order of the results, either to disk or in memory as you go. This is fine for small datasets if your CPU and disk are
fast enough, but as your data grows, so would the latencies for searches. Database indexes

are separate collections that sit alongside your main data, but instead of having the full data that's sorted by VID, it's just one or two properties that are sorted by value.

What's convenient is that you can have all the data sorted ahead of time and can quickly seek to what you need and skip anything you don't when you perform a query. This is also
very useful for streaming results in. Instead of waiting for a full query to finish, you

can start processing data as your application gets it and as soon as it's available. So

databases typically use B plus trees for storing indexes. You can think of them as key value
stores where the keys are bytes and are sorted from lowest to highest. B plus trees also
have the property of being efficient for seeking into a sorted list and to then sequentially

read ranges of key value pairs from that. Here's a rough picture of what the structure
of a B plus tree looks like. Data is sorted and stored sequentially in leaf nodes and

there's intermediate nodes that are created that point to the leaf nodes along with the starting key within that leaf. You can then replace individual leaves or intermediate
nodes up until the root without needing to update the rest of the tree. You can also
quickly search through the tree to find the leaf node that contains the start of whatever range you're seeking. The way it works is you start at the root, you find the closest
leaf to the key you're searching for, then you traverse down and repeat until you reach

an actual leaf and from there you can start doing a sequential read and follow the pointers to the next leaf with the rest of the range you're seeking. One of the downsides of B

trees is that the order that you insert or delete keys can change the shape of the leaf
nodes and how everything links together. For example, if you insert into a tree that had a key deleted before or if you insert in a key that got deleted after, you can have different
boundaries between leaf nodes. This makes them less suitable for distributed systems where peers are authoring and replicating changes in different orders. An alternative

is to make use of another data structure, Proli trees. They're like B plus trees in
that you have ordered key value pairs within leaves stitched by intermediate nodes. However,

instead of using pointers in memory and chunking keys as they're written, each leaf and each
intermediate node is content addressed, in this case using IPLD. As well, when keys are

being inserted into the structure, boundaries between leaf nodes are calculated based on
the content address of keys rather than on insertion order. This means that regardless

of the order that keys are inserted, each peer would come to the exact same structure for their tree at the byte level. Here's what it looks like at the schema level. The root

of a Proli tree has information about how the tree was chunked so that that tree can

be easily updated or merged with other trees without having to hard code those chunking
settings for the entire application. Each tree node then has a list of keys and a list

of values that go with those keys. It also has an is leaf Boolean that lets the application
know whether this is a leaf node, which contains actual values, or if the values are links
to further tree nodes. So, here's a gist of how you construct a Proli tree or how you

can go about updating a portion of it. What's useful is that it's a bottom-up approach and
you can build the tree as you ingest data. So, first, you start by beginning to add keys

to a leaf node. So, as you're doing this, you're going to be checking for chunk boundaries
for each key value pair. And once you've discovered a boundary, you create a parent node if one

doesn't exist and a sibling node. You add all remaining keys to the sibling and you

add the current node and the sibling node to the parent. Then you repeat this process

for the sibling and after you've processed all the keys, you go to the parent nodes and

process them and keep going up until you have a new root node for all of your data. So, with this, you'll have a balanced tree where you'll have chunk indexes where you expect

them sorry, chunk boundaries where you expect them and you'll have sequential keys to read
in your application. So, the boundaries between leaves can be calculated based on a chunking

threshold. This is an integer that's used to determine the probability that a new chunk
should be started. You take the hash of the given key value pair and if it's less than the threshold, then you know you need to put all subsequent keys into a new chunk. This

is reminiscent of the hash-cache algorithm used in things like Bitcoin. The lower your
threshold value is, the less likely a boundary will occur and the larger your tree nodes
will get. So, from here you can start worrying about the details of trees and focus more

on the keyspace. Here's an example of how you can segregate the keyspace for a collection of posts. You can have a top-level prefix for everything related to posts. Then you
can have a longer prefix for all the individual documents in your database that are posts.

And finally, you can have separate prefixes for each index over those posts. With this,
you can do sequential reads on documents and different indexes and avoid overlap between
different collections within a single dataset. For example, here's what an index with a few

posts could look like. We take the created at and tags property out of each post and

we make a new index key which points to the post's ID. Notice how everything is sorted
by the timestamp. So you can seek to a start time and filter out irrelevant tags without
fetching the full post. In this example, we have three posts, but what if you have a million
and only want the most recent 50? Well, you can seek to today, get the first 50 posts

that match your query and ignore the rest of the dag entirely, skipping all of that extra loading. That's because with Prolly trees, the network is your database. As you

query the key space, you'll be loading parts of the dag using either block exchange or
graph sync or whatever other replication protocol you want. And instead of replicating everything

and then making it usable, you can start loading data and just the data that needs to be on

the page now. You can also load more data in the background to speed up subsequent searches
and to be available while offline. When merging datasets, you can skip any equivalent branches
and stitch in new branches without having to fully traverse the values. Combined, this
lets application authors focus more on their data and making efficient queries, not worry
as much about initial load times. There's some tradeoffs to consider. However, if you

choose to have larger chunks on average, you'll end up having them updated more often as keys
are added and changed. At the same time, if you have smaller chunks, your tree will be
deeper and you'll potentially need to do more round trips to load it. On the indexing side,

the more indexes you have, the more duplicate data you have. But these indexes are important
for making your queries efficient. Sometimes it could be the difference between scanning across all your data and having poor UX while you wait for that to happen, or having a bunch
of duplicate data and using up extra storage on your device. Generally speaking, you'd
be making indexes for data locally if it's not on the network, so it's usually worth it. Lastly, the question of how to merge data from multiple sources is nuanced, and there's

a lot of ways to do that depends on your application. You might be using CRDTs, you might have a
last-write-win strategy, you might have some other thing I'm not going to bring up now.

Even though we have these useful structures for indexing, there's still a lot to be done at the application layer to make these databases useful. So, hopefully that's given you some
more insight into how this stuff works, and maybe it sparks some ideas on how you can build your own databases too. If you're interested, we've got a spec for how this can be implemented
using IPLD, and we've got a matrix channel where we've been chatting about peer-to-peer databases in general. So, come check out our source, and join us in building efficient
and usable peer-to-peer apps. Thank you so much. With the Parali treaties, every time you're ingesting new data, you're mutating the state

and potentially creating a lot of new objects. Have you thought about when it makes sense
to do that directly versus when it makes sense to have some kind of novelty cache that builds up a bunch of novelty, and then when you reach some size, then do the merge of that novelty

into the deeper trees and indexing and all of that work that is going to happen every
time you have to merge in novelty? Yeah. So, generally what you want to do if you are going to be ingesting a lot of data

is do batch operations. So, if you look at the Go implementation that's linked to in

the spec that you can see on the screen, we actually use batching where before committing

anything to disk and doing any sort of balancing or content addressing, we just start kind
of like sorting all of the keys that will be inserted before we build the tree. And

then we modify whatever nodes need to be modified. And after everything is ready, then we commit

to actually hashing everything, checking the chunk boundaries and rebalancing and all of

that. So, generally if you're dealing with a bunch of data or even if you're inserting a single object which will touch multiple indexes, you're going to want to batch it
before doing it all on the fly. Because, yeah, as you mentioned, if you do everything all
at once, it's going to be major overhead.
One other thing, I mentioned kind of IPLD and how you can do this high-level algorithm

of how it works, but you can actually do a lot more optimization because you don't have
to have objects. If your keys and your values are fixed width in terms of how many bytes

there are, you can have a continuous block of memory and just have some other data structure
being like, oh, hey, here's the boundaries between the chunks, and we're just going to calculate them on the fly without any extra allocation. So all that to say there's a whole bunch of optimization you can do on reads with batching

and memory allocation that can really be used mostly when you're writing custom code for

the specific data you're uploading. And by default, we have batching already, but the

fixed memory address stuff is kind of like a to-do after we do more performance optimization

for larger scale data sets. Was that kind of the gist of what you were getting at?

Thanks. Yeah, I've got a question. I guess, currently in Fireproof, I'm trying to decide

should I make my leaf nodes all be CIDs instead of embedded JSON? I mean, it sounds like what
you just mentioned about the fixed width optimizations may lean me toward CIDs. The thing that leans

me toward JSON is that most of my JSON documents are smaller than a CID. So it's sort of a

not sure kind of situation. Yeah, absolutely.
So I'm not quite sure what your question is, but I want to say thanks for pointing out that there's more to the trade-off than I had realized. Yeah, I think in general, even if you're looking at JSON-ish documents, it might be worth it

to see if you can use C-BOR or something instead, because then you could potentially still get
that fixed byte width for your data without having to go the CID routes. For example,

if you're dealing with numbers, especially, it's just so much easier to encode them as

unsigned bytes, or sorry, unsigned U32s or whatever, instead of going the JSON route,

where now you also lose the lexicographic sort.
To get that optimization you're talking about, you'd want to have a table model maybe then so you know what your columns are? Maybe. It depends. Personally, before I did this IPLD work, I made a database using similar

indexing principles using HyperCore Protocol's Hyper-B library, which is a B plus tree that's

peer-to-peer. And in there, I actually used BSON from the MongoDB ecosystem. And so that's

also schema-less. And you just kind of have to be careful where you're like, yeah, make

sure you use this schema. Maybe do it at the application level. But since the data is encoded

into binary bytes, you kind of get that whole fixed width stuff for free. And especially

with BSON, I find that having dates be a first-class citizen is super useful.

Interesting. Yeah, that makes sense. I mean, it's almost if you're giving knobs to application
developers, you can say, hey, one of the benefits you get from working with a schema is a little bit of performance enhancement. Yeah. That's also why longer term, I was really hoping we could do a more holistic approach
with IPLD, where we could take the IPLD schema for Prolly trees themselves, and then we can

layer it with the specification of here's how you have indexed collections of IPLD documents,
where here's the schema we're applying to the documents, and then here's the indexes
that we're applying over that schema, and then just have kind of like a spec that we can reuse between database implementations. Because on one hand, IPLD is nice for having standardization on how you encode and decode

data and what the fields look like. But the actual guts of a database, I think, aren't

even just the data, but are more like how you find the data, how you authenticate it,

and stuff like that. So I was really hoping we can make a spec at some point, but so far, it's very up in the air. But yeah, schemas or tables. Right. The decode block function gets a lot of churn in my implementation.
Yeah. Well, yeah, like ideally, if we were all using IPLD, then that could do like a

huge amount of the work for us, where it's like, you might be using C-Boar, you might
be using JSON, whatever else. And if you're using IPLD and CIDs, that can just be kind

of done automatically. As well with Prolly trees, since they're using CIDs to link to stuff, you can have kind of an easy way of building a car file of an entire database
and just kind of dumping it and sending it wherever, like on Filecoin.
Right. Yeah. I've got plans to put car files into the static Gatsby site build.

Whoa. Right. So that the database is ready when you load the page. Maybe you have deeper thoughts
on, like I know that just block by block network sync can be excruciatingly slow. And so GraphSync

is a step up from that. Having a closed world where you ship all the car files is a different

way to do it, but then you have to send the whole database. You don't get that kind of partial data paging that you were talking about. What should we be thinking about if

we want to, you know, if that's the cake we want to eat, like what do we got to do?

Yeah, I think one of the things is that it depends on your load a lot. So if you have,

for example, like Prolly tree nodes, which are very fast, then that means you can reduce
the number of network requests you do. And one strategy that I don't have numbers for
this yet, but I think is going to be viable is when you do the initial load of the database,
we fetch maybe the first few layers of the Merkle tree via GraphSync. And then from there,

we can do a block exchange to download the specific leaves we think we need, or maybe
do subsequent GraphSync requests or yeah, GraphSync requests to get multiple nodes from
there. Because like, you don't really need to download that much, like compared to say,

like an IPFS UnixFS DAG, there's a whole bunch of traversal that's done to fetch chunks.

And especially if you're fetching a directory, there's like so many blocks you want to download

compared to the Prolly trees. If they're fat enough, you might just be getting like maybe
one or two levels at the top before you hit leaves. And just like due to the Olog N-ish

complexity of the data.

pf data set, you can have really huge amounts of data without that much block exchange because like again, the big difference here from other peer to peer database approaches is, we're sparse and indexed.

like, whereas OrbitDB, it's an append-only log where you're gonna be processing a whole bunch of data before it's usable. Or like a lot of other things you have to do like a bunch of work up front. That kind of like makes the network a lot more noisy. Whereas here it's like, we do maybe two requests and we're at data and we're already iterating through it. Yeah, I think as well, there is probably gonna be room for custom replication protocols on top of it.
For example, the Hyper-B library I mentioned earlier in the Hyper-Core ecosystem, their data model is also an append-only log. And so they send bit fields to get like chunks of the log. But what they do is they actually have a little side channel where they talk about like, hey, I wanna do a query for this range in the B-tree. And then over that side channel, the peer can tell them, oh, well, if you want that, here's kind of like the ranges. And then instead of requesting all the blocks individually, they can now send a bit field without actually having to traverse the B-tree. So we might have optimizations like that where we're like, oh, we wanna just get this range from you peer. Either send me the Merkle proof right away or tell me what blocks I should request for me to get that. So there's a bunch of things there, but I think it's really exciting. And I think there's a lot of potential for improving UX. Because right now, peer-to-peer apps, their UX is okay.
But especially if you get a lot of data, it just drops so drastically where you're sitting there just waiting for a spinner or waiting for, like after you wait for the initial connection, waiting for the initial sync, it's just not great. Whereas when we have indexing at our core, we have the potential for just like really speedy databases and like really snappy apps. Yeah, so I think that's like my deep thoughts. Yeah, so I think I heard about the plans to also have a Rust implementation. So I'm from the Arrow team and I would like to love to figure out if I can make ProdigyTrees work with Arrow, with the new Arrow, but it would be quite helpful to have Rust implementation so I can just play around with it. So what's the status on that? Yeah, so there is a little bit of a pause. As you all know, Protocol Labs had like a little bit of like restructuring towards the beginning of the year. And so a bunch of the dev grants that were going out were kind of like put on hold while that was being figured out. But actually I think last week, the Rust implementation has kept going. So Simon or Siano is, I don't know how to pronounce
his handle, has actually been resuming on that. And so he's progressing on that and following the spec that we wrote. If you're curious and like figuring out more, there's the Matrix channel, hashtag p2p-dbs at move.moe.
And so we're chatting in there, it's progressing. So far, I think there's the implementation of a Merkle search tree, which has very different performance trade-offs, but ProdigyTrees are like actively being worked on. So definitely we can get that in Iroh if that's something that is exciting for y'all. I'm personally really excited by that prospect. How do you usually think about caching these things? Because there's gonna be a lot more heat toward the top of the tree and you can save a few round trips if you take just a small amount of storage to cache the top of the tree. Yeah, absolutely. So again, I think we could learn from the HyperCore ecosystem here because they've been doing a lot of this stuff with their Hyper-B module. So what they have is an LRU, so like a least recently used cache, where they just have a bunch of the blocks in memory and they can preemptively search a bunch of stuff. So I'm imagining we might have some sort of PubSub if you have a swarm of peers making updates to databases. And then from that PubSub, you might either gossip like several nodes, depending on how big they are, or you might use that as a trigger to start requesting blocks and maybe caching them in memory. So I think that there's a lot of room for optimizations, but honestly, it might depend on the numbers because my gut feeling, and this is again, not qualified with numbers yet, is that even before we get all the fancy caching, we can still get like really good performance out of it. But yeah, definitely in memory LRU
with preemptively fetching, maybe the first couple layers is gonna do a lot without having to cost too much. Thank you. In fact, like, yeah, in fact, like maybe the first two layers, you only store in memory, because those are the ones that are gonna change the most. If there's no other questions, I actually have another thing I wanna plug, which I didn't get into the slides. Yeah, so I'm kind of into this whole virtual augmented reality stuff. And one of the big motivators for me from ProliTrees is to build spatial indexes. So what's cool is indexing spatial data has been something that databases have been doing for ages and referencing to data that's in a spatial index. There's been just a lot of useful work from say Google and Uber and stuff. And one of the things I really wanna do is to have a collaborative spatial index for like metaverse type use cases where we could use something like quad keys to just have a flat map and have some sort of like very loose layer where a community can just place virtual objects on it. Longer term, I also think this could be a useful structure for community mesh networks that use augmented reality where you can onboard onto the mesh and start placing things in physical reality and kind of like spreading them over the physical connections in your reality. So I think this could be like a useful structure for taking a lot of the stuff we have in like centralized databases, but now making it more peer to peer and local first and kind of like more available.
So that's one thing I'm excited about. If that's something others are excited about as well, hit me up. Yeah. Well, upon that, so I guess that means you'd also be able to use it as a vector index and then you could get that sweet AI money. Yeah, probably. I mean, literally anything you can put in a B-tree, you could put in a Polly tree. And the other kind of like key thing here is that Polly trees aren't just for storage and querying, they're for merging as well. So if you have a bunch of these large indexes or data sets like AI or otherwise, you can now seamlessly kind of intertwine them where anywhere where there's similarities, they just kind of slot together and you can skip a bunch of extra processing for sections that don't have overlap in the tree. So I think there's a lot of potential for combining larger data sets and for stuff like data DAOs to deal with like more interesting big data use cases.
But again, learning what we can do in centralized, but now it's also decentralized. Awesome, thanks so much for all the QA stuff. And it was a super informative. I have code to, I'm gonna go increase my branch factor and take advantage of those fat upper nodes, like next thing. So yeah, I guess round of applause for Mo. Thank you.

