
# IPVM: Content Addressed Compute for an Open World - Brooklyn Zelenka

<https://youtube.com/watch?v=jhtEYr3ORfk>

![image for IPVM: Content Addressed Compute for an Open World - Brooklyn Zelenka](./IPVM - Content Addressed Compute for an Open World - Brooklyn Zelenka-jhtEYr3ORfk.jpg)

## Content

All right. Going to get started here. Thanks, everyone, for sticking out to the very, very

end of this track, which continuing the UCAN theme for like two-thirds of the day. We're

going to talk about the interplanetary virtual machine, conscious-addressed compute for an open world. This is work that builds on top of UCAN. And Zeeshan, my colleague, gave a

talk about the implementation of this in a different track earlier today. This is more
about the specs, standards, and roadmap for IPVM. Ellen Perlis has this great quote from

several decades ago, which I think shows just how universal and enduring a lot of these
concepts are, right? The only universal in computing is the fetch-execute cycle, which we're going to be talking a fair bit about here. My name is Brooklyn Zelinka. You can

find me anywhere on the internet as xpeed. I'm the co-founder and CTO at Fission. You
can find us on Discord or the Pro Collabs network, Mastodon. And the past few months

of my life have been lived as a IPVM spec wrangler, herding cats. It's not much, but

it's an honest life, you know? And hopefully today I'm going to answer these things. People

drop into my DMs on a semi-regular basis asking, what's the status of the project? What's going

on? What even is an IPVM? And so hopefully I can answer all of these for you, plus a

few things that we've learned along the way. So this talk is brought to you by the IPVM
Working Group, which is, yes, spearheaded by Fission, but has had significant effort

from DAG House and BatcoYOW, and also some help from Ceramic and Workforge. So it's a

whole community effort. Timeline. So at the last IPFS thing in Reykjavik in July, I gave

a talk basically pulling stuff out of the Fission roadmap, saying like, hey, wouldn't it be interesting if we did some of these things? That spurred a lot of discussion and
people saying, ah, you know, we need to work on the ABI and, you know, fuel metering and,
you know, all of these things. And it was very exciting. And then there were several months of just silence. We thought that we were going to be able to pull things further closer up in our roadmap, but unfortunately we had to wrap some stuff up first. But then
by the end of the year, this actual video of me going wild on the keyboard, wrote a

bunch of specs to kick things off, and then did a couple of proof of concepts. First one
was actually in Bash, and it was surprisingly high fidelity. It worked shockingly well,
considering how little code it was. And then also a proof of concept in Rust. Then early
2023, Zeeshan got started on the Homestar implementation, which is the RS IPVM. So in

the same way as Go IPFS became Kubo, RS IPVM is now Homestar.

Exactly. Specs v.2 went through another loop, aligning with some stuff that Daghouse in

particular are doing. We ran an in-person workshop in Vancouver, and then here we are

in Brussels at the next IPFS thing, and hopefully we'll have even more cats hacking on keyboards

soon. So what is an IPVM? The overall goal that we're aiming for is to have an open standard,

almost like an HTTP equivalent, but for compute. So computation, like data, should be a ubiquitous

commodity. It should be available to everyone, everywhere. If you want to run it yourself, you can. If you want to pay for a hosted service, you should be able to, but you should also

be able to move between services completely transparently. You should be able to depend

on having an execution environment around, right? So in the browser we have Wasm, we have it on the desktop. In servers, we should be able to pass that around and expect that

to be there as part of our environment. It should be fully consistent between clients,

even if they don't have all the same capabilities. So one of them might have a GPU, the other
one might not, but they should be able to pass jobs around between each other to essentially
subcontract each other to do work. And it should be able to be a full replacement for something like AWS Lambda for open protocols and nodes. So that's not to say that Lambda

couldn't participate if they really wanted to. It's totally open, but you should be able to do the same sorts of things that you could do with Lambda.
A lot of the way that people think about compute is from an era when there was a server rack

in the corner and you could say it runs over there, right? On the co-located machine. These days we have really heterogeneous network, right? So we have Filecoin and the FVM, we

have people running jobs right on their phone, and we have cloud and edge networks. So like

Cloudflare, Edgeworkers, et cetera. And we need to be able to run computation transparently

across these different places, right? I should be able to run totally offline, going through
a tunnel, stuff right on my phone. And then if I need more power or access to some capability

that I don't have, send that job off to somebody else. They can run it and return it to me. Or if there's six petabytes of data, I don't have six petabytes of space on my phone. I
need to be able to run that next to the data and have them return a result to me.
And this needs to be all permissionless. So instead of having a registry that says, these are the things that we can do and nothing else, we are designing this so that anyone
can stand up and say, I need to run a CUDA job or really take your pick. And there's

somebody who is doing service providing, they could literally just turn on the spare cycles

on their desktop and say, yeah, I can run these kinds of things. And the network should
be able to connect them to each other without the network having to understand anything
about the specific tasks that are being passed around. So it should just be a discovery layer

at that point, and then both sides should understand how to actually execute and return results to each other. So we've been trying to get to compute at Fission for a while. And when we looked at
it, we wanted to get to compute. To do compute, you need data. And to do data in a production setting, you need to have auth. So we built UCAN. On top of data, for data, we're doing

IPFS. And on top of that, we've built a web-native file system so that we can do permissioned access to data as well. And WebAssembly, which runs on its own standards process and is quickly

becoming a standard basically everywhere. And with all of these combined, you get an IPVM, roughly. When we started looking at the specs for how to actually glue these parts

together, we realized that a lot of it could actually be extracted out. And so you don't have to buy into the entire stack. If you just want the invocations, and we'll talk about what exactly that means in a few minutes, you can just pull that one part out. We found, for example, we wanted a generalized signature multi-format. And so we actually pulled that

out into its own spec rather than just baking it into the core where it actually originally started. So specs to date, at least. UCAN core to power actually transmitting around

access to particular resources. IPLD-WIT, we'll talk about that later. It's an ABI layer.

So Zeeshan did a bunch of work to get that working, and actually in a very smooth manner.

And VARsig, so this is what I was just mentioning, general multi-format for signatures. We would
have called it multisig, but unfortunately the name was already taken, so we've now called it VARsig, and that lives in the Chain Agnostic Standards Alliance. We have UCAN invocation,

which does input addressing, execution, memorization, a bunch of that stuff. And the same spec actually
includes pipelines, which is call graphs, awaits, etc. Those are actually in the same
spec. Today they might get split out into their own separate ones. We have IPVM tasks,

which is around the computations. If you want to say I need a limit on how long this thing

can run or fuel limits on WASM, things like that. You can configure the shell that's going

to run it and workflows, which is your transactions, error handling, defaults, things like that.

There's one more part in here that we haven't built yet that we have a napkin sketch version
of. That's payment channels on UCAN. That's both for actual payment, but also for limiting

access to any consumable resource. And the running joke around is UCAN channels, or you

can channels, so we'll probably have a cute little anime mascot, I assume. And we've had

a few people say, well, are you just trying to use this to block out all of the other

possible compute providers, etc.? It's like, no, no, please get involved. This isn't to
try to capture the market or something. We're trying to make everybody in the ecosystem
better. This isn't one organization. This is a group effort. We're not competing with

each other in the decentralized web space. We're going up against Amazon and Google and
Microsoft. And we are tiny compared to them, so we need to band together. And by cooperating,

we can only benefit from each other. There's just so much space for us all to play in.

So let's actually look at how this is structured. This is UCAN invocation. Ultimately, we want

to pass arguments to WebAssembly or potentially to other things, but today it's all WebAssembly
for now. So we just have those two, great, and they're content addressed, and then we

wrap those in a task, which has some of that config that I was mentioning earlier as well.

There's a difference here between the reference and the actual indication that sometimes needs
a little bit of explaining. Using JavaScript as a rough analogy, I have a closure, so just

an anonymous function that points to an alert in this case, and if I just pass around the

reference to that, I just say message, nothing happens until I actually put the brackets
on it and then I get the alert, shows up. A more classic OCAP description of this would

be if I give somebody, why do we need this instead of just UCAN? If I give somebody my
car keys, that doesn't tell them what I want them to do with it. If I hand the keys and

they immediately get in the car and drive off, that may or may not be an appropriate thing to do. So there's a difference from here's the keys, I'm leaving for the week, versus I need you to actually run this task for me. So these exist at separate layers, you embed a UCAN inside of this structure to authorize that something's going to happen if you need access to a resource. And then this describes what I actually expect you

to do with that. Once it's actually run, we attach it to a receipt, or rather the receipt
points at this task. It includes the pure values that were output, and any effects,

that is to say any further tasks that I want to have on queued be put back into the system.

And then additional metadata, it might be a trace, tags, things like that. Here's the

actual IPLD schema. So at the bottom is what we're calling instruction, used to be called
a closure, and this contains the resource, so the URI, the ability, so these are directly
from UCAN. Any inputs it needs. So for WebAssembly this will just be a single string key, that's

args and then array of the arguments, but could conceivably be anything. And a nonce.

So with deterministic wasm, you can actually leave this blank, which is great, because any time you run that, they're all equivalent. But if you were to say update DNS or send
an email, those may need to be unique invocations, and so they need to have this because we index

everything based on this specific structure. And we'll talk more about that later.

We wrap that in a task, and point back to the instruction in this run field. Invocation

then adds the authorization on top of that. So now it's actually been signed by the invoker,

by the one that's requesting this to actually be run, and then that contains the other two.
And we can have a bunch of these and wrap those up in a workflow. So we'll talk about workflows near the end. When you're building these, how do you pass in the right arguments? So this is a question about ABI. We have in IPLD, it's essentially roughly JSON, plus a couple extra types. And

this is the WebAssembly component model types. This is WIT. They're a little bit different.

And WIT isn't actually done. It's still in flux. It's still changing a little bit. It's
changing slower. And we've talked to the folks at the Bytecode Alliance. It sounds like maybe
end of this year, this will be actually done. Right now, the spec is empty in their repo,

but they've done writing in other places. So we're just going to keep it up to date with this stuff. But just, you know, caveats. Some of the underlying details here may change.

When we looked at this, we're like, well, we want to reuse as much as possible existing stuff happening in the Bytecode Alliance, not having to write stuff ourselves so we can reuse all of that tooling. But these are very different types. In IPLD, we have two

numeric types. And in WIT, there's ten. So they're very different. But if you compile

your module with the component toolchain, it creates type definitions for you. And so

Zeeshan went ahead and created a totally transparent translation layer, both on the way in and

on the way out. So we get data back as WIT, and then before we put the results back into

IPFS, we convert them back into IPLD. So the advantage of this is you don't have to think

about, except for when you're compiling your WASM, you don't have to think about that layer at all. You just hand it essentially what looks like, I mean, I've inlined a few things

here, but essentially what looks like JSON or CBOR or however you want to have that formatted.
And it'll do the translation for you under the hood. Let's talk about Dataflow. Dataflow is pretty great. I think it's too bad that we don't

get to work with it more often. The guy who invented it tried to, or he went to the lawyers

at IBM and said, hey, I just invented this thing, should I patent it? And they came back to him and said, well, this seems kind of more just like how nature works, so we can't actually patent it. So it's not that surprising that it shows up all over the place, but really

important for how we do things in IPVM because it's very distributed, and especially because

there's an increasing amount of data going onto these networks. We have a lot of data today, but as we're computing, we're going to be dumping more of that into this data store, including receipts that come out of this system, out of the computation. So there's
additional metadata in addition to the data that's actually being operated on and updated.

So we need to be able to pass messages to where that data lives and have it execute there and return us results, at least just the thin receipts as well. In order to do
that, we need to be able to describe everything that should run over there, rather than waiting for a single thing and going back and forth. In order to do that, we also need to be able to transfer authority. This is part of where UCAN comes in. So here's Alice. She has a

bunch of capabilities and she can delegate all of those to Bob, and Bob can delegate

just some of this to Carol, who's going to do some service providing for him. This can
also go in other directions as well. So this could even be, say, Alice kicking off a job,

and then the other four here collaborating to do which parts of the jobs that they can,

and essentially subcontracting out to others the bits that they can't do because they maybe
are missing some hardware or missing a private key because they can't decrypt some data, etc. This breaks down roughly like this. This is a simple example. So when you pass a bunch

of invocations to a workflow, the order that you pass them doesn't matter. We just analyze
the dependency graph and produce the graph from that. So we have a bunch of awaits in

here. So if you look at this one, everything depends on the top. That data then flows down

into both of these email sends, and then at the end the bottom one awaits those other

two. We have a lot of flexibility now in how that actually gets run. So the scheduler, the distributed scheduler has a lot of control over what happens from here. So we can break these up if it

makes sense to do this based on how much load they're handling, if they have certain capabilities,

service discovery, all of this stuff. So in this case Alice is doing these first two, Bob does the other two, and the awaits actually go across the wire. So you could run that entire sequence locally if you have the ability to do it, but if it
needs to be broken up across multiple providers, that happens automatically in the system.

And we've taken some degree of freedom away from the developer saying, no, you can't just put them in order unless you specify the dependencies between things. The upshot is that we can make really efficient use of the network as a whole.
Those awaits are also in the schema. So they look like this. It's a key value pair of awaits

and then the branch that you care about, so either the okay or the error side. Or you can take either. and it'll include the branch in the result. That points out an instruction that it's awaiting.

So it's await okay, this CID. That will resolve into a receipt, which will finish the awaits,

which then means that another instruction doesn't have awaits anymore, so it's all been inline,

so that can execute now, which will produce more receipts. And we have this cycle of resolving, producing more receipts, and that's how we do the actual working through the call graph.

On IPFS, we do content addressing. So I have some content, like a nice Belgian waffle,

and I just hash my waffle, and I get a key value pair. Here we have receipts.

But I don't know what the output of it was, so I need to actually index it on the request for the job.

So instead of hashing the receipts, because we don't know what that is,
we hash the description of the job instead. So this is called input addressing.
Because of this, it actually has to live in a different DHT, so we're just composing this out of libpdp.

It was actually, like, libpdp is amazing, so we spun up Academlia, used these as keys, and it works really well.

You could potentially retrofit the content addressing into this new DHT, because one way of thinking about

content as opposed to computation is it's like a function with no arguments. It's just like, well, just give me the thing, it's just the identity function. You could do that. I don't know if that's actually required. It's more of just a fun thought experiment.

Because now we have the ability to look up results and to see if someone else, anyone else on the network,
has run this before, when we're calling certain functions, certain WASM modules,

we can look up and skip steps as needed. So as we go from each component down this chain,

we can tap it and asynchronously do the WIT to IPLD conversion and dump it into IPFS,

so that it's actually happening in a separate process as we continue to execute the WASM.
So when you have composed WebAssembly modules, they actually get stuck together, and we haven't implemented this part yet, but you put a thin shim in between that then basically redirects,

copies the data and redirects it out into the separate process. The advantage is then if I ask for this process again, or the process crashed and I'm restarting,

I don't have to run that again. I can just grab that out of the DHT and continue on computing.

Earlier today, Zeeshan gave a demo of exactly this. So took a photo of a cat,

and the first two steps of these two workflows are the same. So it was crop the image and rotate it 90 degrees.

And I think he also crashed the process and restarted as well. So he ran this top flow first,

and then blurred the image, and then after that does crop rotate and grayscale,

but because the crop rotate steps have already been done, it skips them completely and just does the grayscale.
So you get this massive performance improvement. Because this goes over the network,
this could also be on another machine, so you might not even be aware that there's been a crop rotate done before.
You just notice that things got really fast all of a sudden.

When you're scaling up computation to work on multiple processes,
and we have very, very high parallelism potentially in IPVM, what you want is to have this linear...

If you don't have dependencies between data, you can get this linear improvement. As you add more computation resources, you can go faster.

The commonly cited limit to this is Omdahl's Law, which says there's some coordination,

you get less benefit as you go, there's only so much you can parallelize a task.
In reality, what often happens is the universal scaling law instead. Because you have incoherence and data contention, things like this, actually adding more parallelism can slow things down. The good news is on the thing that we think is going to be by far the most used in IPVM,

which is the deterministic subset of WASM, you don't have incoherence or data contention.

You don't have to wait for anybody to finish. If they're taking too long, you can just run another copy of it yourself if you think you can get it done faster. You can pull receipts that somebody else has run, even race them, and go faster.

You can do this thing where two of the three steps on the image are shared, and you can just skip right over it.

Something that on the flight over here, Tim was chatting about, that's kind of like an interesting accidental thing that's fallen out of the system, is that we get reverse lookup for free, because we've essentially built a reverse lookup DHT.
I can go from a SID to its computed metadata. I say the resource is the LLM WASM.

Its argument is a SID. I take that and say, well, what did you compute as metadata out the other side?
Was this moderation classifier or an image classifier to say, here's a bunch of tags that you should add to it?

Or do we want to check if a token that you can is valid?
We can just go into this DHT and say, hey, how many receipts do I have for that?

Okay, cool, there's 10 people that have already checked this for me. I have a pretty good degree of confidence that this is actually going to be a valid token. I don't have to go and pull all of them down and check the entire thing.

Which is something I've heard a lot of people actually talking about,
is this classification, how would we actually look up the metadata?
This is one approach to it. There's probably generalizations to it as well. But it's just something that we get for free, which is kind of nice. Let's talk about safety. People often, especially if they have a distributed systems background, are worried about partial failure.

We have workflows, there's lots of different things happening. What happens if part of it fails halfway through? We've actually given this a fair bit of thought, mostly coming from thinking about software transactional memory.

This great paper from Microsoft Research called Ambrosia, it coins this term virtual resiliency, which allows you to write failure-oblivious code,
i.e. code that doesn't know that it could have partial failure in it, run in a failure-resistant manner,
by limiting to certain cases how you structure your computation.

The basic upshot of it is if you limit yourself to only having a mutation at the end, you can do as many queries and just raw computation in the middle as you want,

you get essentially software transactional memory. You get transactions for free out of the system, which means that you can use all of the techniques from the database community.
And the developer no longer has to worry about the possibility of failure. The whole thing will get committed or not, just like a database transaction, but for arbitrary computation.

I lied a little bit a moment ago when I said that it's queries and mutations, that's a simplification.
The three properties that we actually care about are determinism, idempotency, and mutation.

I'm going to keep using query and mutation just to keep it simple, because it's a half-hour talk.

We have a call graph that looks like this. I've got a bunch of queries and then some computation, then another query, then some computation, and then at the end is a mutation. We can do this entire thing atomically, and that will work. We can rerun big chunks of this until the mutation at the end. If we have something that looks more like this, where we have mutation layered throughout,
we can't have this be atomic anymore. The plan for this is to say, hey, you should really add things or structure things

if you have a single mutation. If you need to have several, that's fine. You just have to pass a force flag to it and say, I understand, that's okay. There might be a partial failure in that case. The overall layout of this is we have queries and we have computation,
and those can cycle back and forth as much as they want,
but we have to treat mutation in a very special way. I'm skipping over a little bit of detail here, but that's the basic idea.

The other thing is when we have these call graphs, we can't just do a top sort on it and say, yeah, here's roughly how they're laid out. We need to push the actual call of that mutation to the very end. It might not even have a dependency, but we're going to leave that as long as possible.

A related thing is doing SID resolution.

There's a question when you involve things like slashing, who's at fault when something can't be run? We have a CID, we pass that to the process. It checks on the network, hey, can I actually resolve this thing? If the network says, yeah, I can totally do that. I'm not going to give you the whole thing right now, but yeah, I have that available. It converts it to a runtime type that is not expressible in IPLD called a content handle or cha

and passes that to the task. This happens under the hood. Programmer doesn't need to know about it, but tasks don't accept CIDs. It doesn't know about them. It only accepts these handles that have already been checked by the runtime.

Some stuff that we haven't gotten to, but that's sort of in the nearest term roadmap,

is optimistic verifications. We already have receipts. We need to start comparing receipts to say, you know, I've had so many confirmations of the same value.

I got three receipts. One of them differs. I need to go back and rerun that one step or check it against a referee of some kind.

We would really like to experiment with zero knowledge proofs for this stuff as well.
So one of the ZKP WASM systems would be really great.

We really wanted to have IPFS run working before we got here. We kind of hacked it together, but doing a deeper integration would be nice. So this is definitely very much on the roadmap still as well. And decentralized WASM repositories. So instead of having to always write your own WASM every single time,
putting this into a file system and routing that at a name, so it might be on NNS or on DNS link or somewhere,

and saying, yeah, just look at my repo and it's at this path, gives you essentially package management.

And then you can start composing workflows out of well understood, already known task types.

So please join us. Here is the link to the community, which is a GitHub org.
We do monthly calls on Luma. So there's the link for the Luma as well. And in the uncomf, we have two sessions tomorrow in the morning and in the afternoon.

So the Rust template and Homestar with Zeeshan and then everybody in 2.30 as well.

Thank you. Any questions?

What are some of your early thoughts on the security of the input based DHD? Because you're storing outputs to an input CID.

So how do you know who to trust? In that sense, are you assuming that you're only going to be accepting receipts from people you trust?

Yeah. So there may be more than one receipt, so you can set a minimum number of confirmations that you need.

So it's like, I need 100 confirmations. You can scope it down. Actually, the way it works today is it's scoped down to a subnet as well. So it's already in a trusted environment. We're going to open that up basically as soon as we can. We're just trying to get this system up and running first. And a reputation score is the short answer in the longer term as well.

When there's actual long running tasks that take a proper amount of time, like more than scaling pictures or something,

are there ideas already around how a coordination between worker nodes would happen so that they don't start the same task at the same time when it flows in?

Yeah, absolutely. So for things that are happening in Wasm, if there's some degree of parallelism, that's actually not necessarily a bad thing.

The process that kicks it off, it's not that you say, hey, who wants to pick this up and they just all run with it?

It's all mediated by UCAN. So you actually have a point to point saying, hey, actually, I've agreed. Yeah, you run this and you run this and you run this. So you control your level of fan out. This is great. I have a bunch of questions, so I'll ask a few and then pause for others.
For receipts, it seems right now the receipt is both addressing a competition that has terminated and addressing a competition that may or may not have started or that might be ongoing.

Is that correct? Receipts are things that have terminated. So then why can't you address a receipt with just a CID? Why do you need the handle that is not the output of the competition?

Is that just to enable the lookup so that you can find the receipt? Yeah, exactly. It's to say, I have this job and has somebody run it before? Is there an output for it? Because I don't know what the output is. So it might be useful to have a separate type there, which is the handle to the computation, whether or not it has terminated.

And that's the thing that you should place at the mutable invocation reference, because some long-running thing could signal that and you can find the handle to the thing that may or may not be computing.

And it might be a place to place other things like what nodes might be computing on this thing at a particular moment in time.

And then I had another question around IPLD-WIT. Would it make sense to just make an IPLD codec that reads directly from WIT so you can just store WIT as is without having to convert back and forth?

Yeah, absolutely. Yeah, I totally think we could do that. I'll pause for now. Good suggestions, though. Thanks. How do you think about work that is not fungible? If I have a job that wants to do something like read some weather data in Atlanta and you go, oh, well, I scheduled in Boston, that may not solve your problem.

Yeah, totally. So there's a bunch of cases like this, right? One example is email server for a certain email address or decrypt this data.

So I actually didn't talk about decryption stuff at all. There's a layer about privacy as well. So decrypt this data and operate on it, right? Yeah, there's lots of non-fungible resources. Yeah, right? There's a huge number of use cases for this. That's done with, and we didn't really talk about this either, affinities. So to say certain nodes have an affinity to run certain kinds of jobs and they have to prove that they have those things.

And other things in that realm of affinities could even be like, I have a GPU or not, right?

So it's very common. We just haven't baked it in yet because it's just WASM today. Do you also support anti-affinities if I want this to be on a different host? Yep. It's not implemented yet, but that's the plan. I had a question as to why the resource is a URI and not just a CAD because then it doesn't make the invocation fully deterministic if it's a resource that might be mutable.

Yeah, so some resources are mutable, right? So we want to enable things like read from DNS link, operate on it with deterministic WASM, right back into DNS link as an atomic transaction.

So I need to then say the thing that I'm resolving is the DNS URI. It might be useful to create a different type there for things that are fully specified all the way down to immutable artifacts and things that may be mutable.

Because then it becomes really easy. Certain things become really nice and easy when you know that everything that you're pointing to is fully figured out as opposed to you might have mutable ref.

In the same way that you have these mutations that might occur if you have the ability to just distinguish a call graph that is entirely deterministic from one that isn't.

Yeah, so we do that. By looking at the resource type, like literally the schema, and the ability, we can tell in advance the safety level of that bit of computation.

So actually this is also a two of three, right? You can't have all three of these.
If it's deterministic and idempotent, it must be pure, for example.

So you're in this totally safe, I can do whatever I want level.
We've worked it out because again it's a two of three or you can even have one of them. And I think there's like eight or nine levels that map roughly to consistency levels, basically.

And then that changes how the scheduler can interact with it. Yeah, got it. So then it's the program writing the invocation has to make sure that the description is correctly matching the resource type.

And is it easy to enforce? I can imagine many cases where lying about that can get really bad for the computer that is trying to run the thing.

Yeah, so an example would be, this actually used to happen, right?

Having mutations happen on an HTTP GET, that breaks the contract, right?

We don't have a strong solution for that other than a reputation system, they shouldn't do that.

And then on the traces for receipts, which I think is great to have the ability to have that, that might want to be in some kind of handle instead,

because you might want to be able to see the trace as it's occurring. So think of like a CI model where you have a job and it's processing and you want to see the outputs as it's happening before it's terminated.

Would you do those as a DHT or would you have them as a...

Etc. ŠAŠan įσųŴ and say gossip, so. Yeah, I would probably just use the DHT for pointers, like mutable references only, and then you then put everything else as mutable objects in an appending log. So I would do it kind of like a get graph where you keep rolling up, and you just change the latest state of the head in the DHT or something like that. And you could, once you have identified who has this content, you can obviate the call to the DHT entirely, because you know who are the parties that are running the computation, and you can just ask them. For example. What about compute over data? Meaning, specifically, do you have anything like affinity to the input data? So like, hey, I have a data set. Someone wants to do a computation over that specific data set. So therefore, let's make sure we target the code. Because the nice thing here is that IPVM lets you basically ship code really easily to different endpoints. So is there a plan to model the actual affinity to the data itself? Yeah, absolutely. So essentially, what the scheduler will do, it's not this sophisticated today, but it looks at the description of the thing that's being run and its inputs, asks, hey, who has this data?

Is this 9 terabytes of data, for example? Then, OK, I'm going to try to schedule that with you if you can provide that to me. And it will win the bid for that job, basically. Thank you. As a follow up to what you just said, this communication to find out which node should run what and so on, is this like a PubSub protocol? Or how are these nodes talking to each other? Yeah, today, GossipSub, EpiSub eventually, I assume. Yeah. And you can also do pre-negotiation for some of this stuff. So one thing that I didn't mention in the payments layer, or that was really just any sort of consumable resource layer, is you could say, I prefer to run, I have a bunch of credits with this provider, and I want them to run a cron job for me every week. So you can do that pre-negotiation, and then it'll just kick off the process every week. All right. Thank you.

