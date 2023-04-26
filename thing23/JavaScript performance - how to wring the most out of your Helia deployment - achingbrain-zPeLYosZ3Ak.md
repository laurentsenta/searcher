
# JavaScript performance - how to wring the most out of your Helia deployment - achingbrain

<https://youtube.com/watch?v=zPeLYosZ3Ak>

![image for JavaScript performance - how to wring the most out of your Helia deployment - achingbrain](./JavaScript performance - how to wring the most out of your Helia deployment - achingbrain-zPeLYosZ3Ak.jpg)

## Content

HELIO & PERFORMANCE OK, Helio & Performance How to wring the most out of your deployment This is going to be a fairly demo-like talk
If it gets dry, there's all these little sweets on the table, you just throw them at me or something
Keep it interesting, you know OK, so it's me again You'll see I've moved from being the JSI Professor maintainer to being the Helio maintainer
Things move fast JavaScript is slow, right? It's super slow I mean, it can be, if you use it badly There are things it's really bad at, like CPU intensive tasks, awful, or worse

Don't do it, if you can help it Async, also terrible Async is not free
We have to remember this This is a picture of the browser event loop I'm not going to go into it in very much detail, there are loads of really good resources out there

The really important thing to notice is when you're calling browser APIs and you see it says promises

And there's a little line, a tiny little line, oh by the way I'm going to add something to the microtask queue
So every time you invoke a promise, an item gets added to the microtask queue

This is incredibly expensive When you're doing stuff in a very hot loop You really don't want to do it unless you absolutely have to And I think we, like promises have a lovely API, just like await a thing, that's great
But then actually it can be really really bad So how bad, so a quick demo of how bad it is Where's my thing gone, there it is, ok, ah it's again
So what have we got, we've got itpipe
So it uses a lot in JS, everything It basically is for making pipelines of operations It all just takes an async iterable and just turns it into a list

So these modules have recently been refactored quite a lot So I say it takes an async iterable, it takes an async iterable or it takes an iterable If it takes an async iterable it returns a promise of the things And if it takes an iterable it returns the things So now these things are actually promise aware Which is pretty handy So we've got a synchronous pipeline here So all we're doing is, we're having a list of numbers, five numbers
We're just going to iterate over the source, iterate over this list, increment them and yield them

The output of the first function is passed into the next function And so on and so on, and at the end we just collect them all together So we've got a list of five numbers and we've got five transforms
Five transform functions and then a sync at the end where we just collect them all That's the sync one, and then the async version is exactly the same Exactly the same, the only difference is we've just stuck the async keyword on
Which we do a lot, oh yeah async, no problem So you've got the async keyword on there, we await the constant of the list Note that the list input is still a synchronous list, it's just an array of numbers
So we do the do all the way and then just collect it at the end and then await the result
We don't have to await the result here because the whole thing is synchronous

Node Perf1

So we run the benchmark, bang, and you can see the difference So the number of operations per second is 132,000 operations per second in the synchronous pipeline

And then 81,000 operations in the async pipeline So it's an order of magnitude slower And all we did was put the async keyword on things and await So async is not free, definitely not free
Storage, storage matters How am I doing for time by the way? 12 minutes, ok right I'm going to have to start speaking really quickly Ok, ok, well it'll be nice if it gets back on track Anyway, storage matters CPU is boring in JavaScript, but I.O. kills everything
Like it doesn't, a lot of the time it doesn't matter if your algorithm is the fastest or not Because every time you hit the network or the file system or whatever it just blows it all out of the water

An enormous bottleneck, so you have to think really carefully about the block stores that you use in your application

This is something that actually bit us a while ago One of the users opened this bug that the S3 performance was really slow
And there's this tiny little, tiny little thing in the Amazon documentation It says your application can achieve at least 3,500 operations

Or whatever, per second per partition prefix
And a partition prefix is basically a forward slash in the bucket path

What it doesn't say there is that if you don't have any forward slashes in your bucket path then it's one partition

Which means that your entire bucket can achieve at the maximum 3,500 operations per second

Which is awful if you're scaling Really bad! And then also, if you create 10 prefixes you can do 55,000 reads per second
Which obviously if you see that it was 5,500 and suddenly it's 55,000 with 10 prefixes

What's 5,500 times 10? It's like, oh okay, right, that's a fairly linear thing I wish you told us about that in bigger writing So now the S3 block store has learned from this and applies a default sharding strategy

Of just taking the last two letters of the path, reversing them and sticking them on the front

You can shard, obviously there is a limit per prefix

So if this prefix isn't long enough for your application You can override these settings when you create your block store You should measure everything before making decisions
Other data stores are available So we have these two modules, block store core and data store core
And they include implementations of a whole bunch of different types of block store and data store And they're not always stores, they don't always keep the information Things like mount data store I think is very interesting Perhaps something that has massively unrealized potential for performance All your data and blocks, in particular the data store, they're all accessed by keys

And the keys have a very predictable structure
So that means that you can say with absolute certainty that certain classes of data are stored with certain keys

So you could treat, for example, peer store data as ephemeral

Because the network changes a lot, you join, you leave If your peer ID changes, suddenly you have different peers that are close to you So maybe you don't actually care that much about the peer store data So if you had a mount data store and you just used a memory data store for the peers prefix

Then all of your peer data would just live in memory And then when you shut the node down and start it up again, you haven't persisted disks But you also haven't, aside from the async tax that I just went through
You haven't hit the disk at any point or hit a network or a database or anything to actually store this information

So it could, like if you see a lot of peer churn, this kind of thing could make your application massively faster

The same way with some blocks, if you want to ensure that some blocks are very fast to fetch and others you want to store cheaply

You could put them on S3 or you could put them on Glacier Storage or whatever Go nuts, like this is a toolkit, you can build this Tid data store is another one, so the way this one works is it wraps a whole bunch of different data stores

And if you write, it will write to all of them and if you read, it will read from whichever one is fastest
So maybe you would have, you know, like Web3 Storage for example
Races a bunch of gateways against each other to fetch a block You could use something like this, and there's the abstraction for you And then like, you know, they would just pull it from whichever one is quicker Maybe you could create your own, like the hot content block store Like, I know this CID is going to be massive, it's going to blow up, it's my new album, it's incredible
So I'm going to write a data store that stores that CID in memory Like the blocks that make that CID up, stores them in memory, so they're super fast to retrieve
And everything else can go to the, you know, the slow storage
It's up to you Content reading, so content reading is really slow in general Like there's just no way around it, because it involves network requests So if you're searching for content, you have to hit the network And hitting the network is slow, regardless of the language So one thing you can do is you can configure delegates So these are different implementations of content reading
And like JSLickP2P will try them all So some of them are fast, they're streaming IPNI, of which there are some talks about this week You should totally go and check them out It's novel because it has a streaming API So the ones that don't have a streaming API They return a JSON object that has a list of providers for a given CID
But there's the whole list And once you get to the end of the list, there are no more And the list is wrapped in a JSON object Which means that the slowest provider to arrive You get it at the same time as the fastest provider to arrive So generally you want to go for the ones that have streaming APIs So IPNI has a streaming API, whether or not it's streaming on the back end, I don't know But it has the capability for this The non-streaming ones reframe And then the old school delegated content reading Which just hits the KubeRPC API
Pinion garbage collection So we care about this This is more like an internal thing about how Helia works
But in the olden times, in JSRPFS We switched from using a DAG So the list of pins that you had on a given node Used to be stored in a DAG And you used to have to manipulate that DAG Which involved hashing things and all that kind of stuff Which is CPU intensive And if you remember from this very first slide CPU is awful, don't do it! So what this one does instead Is it stores all the pins in the datastore And the datastore is very fast, particularly in JS And so what this graph is showing is As the number of pins that you have The time it takes to add a pin goes up and up and up That massive spike is caused by the DAG that we had Going to another level So we'd filled one level of the DAG And then we started a new one And then suddenly the number of calculations that you have to do To recalculate the root CID and that kind of thing Massively goes up Which is why you see that enormous jump there And then the red line at the bottom Which you can barely even see is the datastore The datastore performance Which is massively better And this is the time it takes to add a pin So you've got the old JS implementation on the top You've got Go in the middle, so that was Kubo And then the new one New one Is the red line at the bottom Like massively faster And this is the number of pins that you're storing And the time it takes to store the next pin So this is 100,000 pins in the datastore And it's kind of flat Which is what you want out of a refactor like this So it's much more scalable And that was just the I'm going to go back one So that was just the That was putting pins into the datastore But it doesn't tackle garbage collection Which historically has always been super slow Until, until now So Helia runs reference counting For its pins So it's a very simple idea That's the PR that it landed in Super simple idea You It has a benchmark suite as well Which is what I'm going to talk about next But yeah, it's a super simple idea Every time you pin a DAG You walk the DAG To ensure that all the blocks are present Every time you encounter a block You keep a count of the number of times You've seen that block in all of the DAGs You increment it And then when you unpin When you unpin a DAG You walk the DAG again And you decrement that Every time you Every time you Every time you encounter one And then when it comes to running garbage collection You just look at the counts And you're like, does anything have a count? Okay, you're safe If not, you're gone Which just makes it much quicker Because then you're You're basically deleting the blocks As fast as you can pull the pin counts Out of the database Which is a lot faster than Walking a massive DAG So what would previously happen is You would walk every DAG of every pin And you would say, right, these blocks are safe And then you would walk every block And say, is it in that set of blocks? If not, then delete it Which is enormously expensive Whereas if you're just pulling stuff out of the datastore It's much quicker So here's what happens So this is the number of blocks That have been pinned along the bottom And the number of milliseconds it takes To do garbage collection And so cubo is the red And helia is the blue And you can see that helia is much faster And as it gets further and further away It gets bigger and bigger I mean, this is what you want Totally what you want Got some other useful stats out of it So this is how you When you're unpinning things With helia it's super quick Because you're just doing some database stuff You're not really doing a lot else Cubo does a lot more work When you're adding blocks to the block store I mean, adding pins This is the good one, right? Adding pins is just flat Much faster, much, much faster And also this is what happens when you Instead of just blindly copying The algorithms that cubo has works for cubo And it works for Go And it doesn't work for JS Because JS is terrible at CPU Whereas this approach doesn't use CPU And so consequently it's much faster So you write algorithms That are sympathetic to the environment That you're deploying them in Not because of the algorithms Last section, DAGs and bit swaps So bit swap is slow, it's really slow I mean, Ian's talk in the keynote
He alluded to If you have sensible data structures It's not necessarily slow Which is completely true So what is a DAG? I'm just going to go over what a DAG is I was going to use the DAG explorer But it's really sad at the moment It doesn't work, which is a shame Because it's really lovely If someone would like to PR effects to this Maybe with helia, that would be cool Anyway, so I robbed this from some IPRD documentation So what is a DAG? It's a graph It's a directed acyclic graph Which has an interesting property In the way we do them At least you don't know what's If you have to load another layer of the DAG You don't know what's there Until you've loaded it Which takes a long time Which is slow And that's what we don't want So what that means, essentially The interesting fact is Every time you go down a level You're incurring a performance penalty So maybe don't do that So again, here's some benchmarks These ones are quite interesting There's a pull request It's not fully formed yet But please do check it out Anyway, so What I did here was I changed I was changing the This is our bench line, sorry This is the kubo defaults Which jsipfs inherited And you can see the The size of the DAG along the bottom And the time it takes to transfer Between two nodes And they're all kind of like Yeah, it's interesting So Helio is actually somewhere up here Which means there's some low hanging fruit I'm sure in bitswap And what I did here is I increased the block size So instead of having 256 kibibytes It's now 1 mibibyte And you can see the numbers on the left So the top here is 150,000 milliseconds And then the top goes down to 100,000 So there's significant speedup Just by making the block size bigger Now the interesting One of the interesting things about this Is Filecoin uses 1 megabyte block sizes And there's already I mean look how fast kubo So kubo is here The kubo2kubo To transfer 1 gig in 256 kibibytes And now it's all the way down here So like these things really matter And you should totally benchmark The kind of data structures that you're creating As just some stats So Helio with 256 kibibytes Goes from 10 seconds to 7 To transfer 100 megs Kubo goes from 8 seconds to 2 seconds Which is, you know, massive So when people say that Bitswap is slow Maybe it's not Maybe it's the data structure that's wrong These things can definitely be optimized That's it So the important thing is to think about stuff Don't take anything for granted We need to measure things And think about our data structures And no nice abstraction is free Definitely not in JavaScript We should definitely make things better and faster Thank you

That one needed more emojis I meant to ask after the last talk But is there a certain channel that you hang out in Where people can find you on Slack or anything like that? You can just add me anywhere So I've been trying to be more on the IPJS channel Cool But yeah, you can just add me anywhere Good, good

