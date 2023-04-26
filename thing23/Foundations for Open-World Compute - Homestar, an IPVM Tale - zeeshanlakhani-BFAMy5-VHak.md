
# Foundations for Open-World Compute: Homestar, an IPVM Tale - zeeshanlakhani

<https://youtube.com/watch?v=BFAMy5-VHak>

![image for Foundations for Open-World Compute: Homestar, an IPVM Tale - zeeshanlakhani](./Foundations for Open-World Compute - Homestar, an IPVM Tale - zeeshanlakhani-BFAMy5-VHak.jpg)

## Content

So yeah, in this talk, we're going to talk about the foundations for open world compute,

Homestar, which is our implementation of IPVM, as like Kubo is for IPFS.

So we'll talk about that and what that means and what are the issues with open world compute.
So I'm Zeeshan Lekhani, I work at Fission as a staff applied researcher and engineer.
I also started a thing with a couple of friends of mine called Papers We Love. Anyone heard of that? Yeah? Oh, cool, good to hear. Oh, nice. Yeah, so we've been running conferences and trying to get that community together for

a good amount of years now. And I also am attempting some sort of part-time PhD at CMU in programming languages, specifically
a thing called polarized subtyping and functional languages. If you're interested in that stuff, we can talk offline. And that's my daughter, who is an awesome, awesome kid. And she's always happy. So that's awesome. So the framing on this, we'll talk about what IPVM is at a glance for people who may not
know. And we'll talk about some of the early things there. Actually the way we wanted to do these talks was Brooke, Brooklyn, who works at Fission CTO, was going to give the community talk first. And then I was going to come in with the implementation. Everything did not work out this way. She is giving a talk later today about IPVM as a whole. We'll cover some similar things, and she'll also talk about more of the community standards and specifications. I'll talk a lot about workflows. Some of our research in building a workflow system to work with IPFS and do computes.

So I'll talk about that. I don't know if people are familiar with a lot of the microservice kind of world of workflow compute that's been happening. We'll talk about WASM and WIT, which are WebAssembly interface types. So with all my friends, that's pretty good. What it means to try to do an open world compute system. And I think we're just starting out in the implementation of IPVM with Homestar.
But I hope throughout the rest of this conference and the Young Conference, we'll get a really good sense and see how do people want to work with plugins in a system like this. How can we run compute anywhere in a decentralized way?
So as mentioned, and I said Brooke is speaking later on this today, and we're also going
to do two talks at the Young Conference. I forget which day. I think it's tomorrow. She'll be talking about more stuff there, and I'll do more of a code rundown about some

Rust stuff we're doing at Fission and how that relates to Homestar, our implementation.
But Brooke has given a lot of talks on the interplanetary ritual machine.
There's a lot of great talks already there that really cover a lot of the ground of what these things are. Some of the quotes that come out of her talks that I think really gets at the large scale,
the large distributed scale that IPVM is, is nothing less than connecting all of the

world's users and services. Pretty easy. Just a little bit of engineering, I think. The HCP storage and compute equivalent, open, interoperable, and everywhere, must be substantially

better than Web 2.0. So when I started at Fission and we started working on the implementation in late January for this, these quotes, particularly the first two, I was like, oh, I have a lot of work

ahead of me. A lot to be done, a lot to think about. And of course, this is all tied into UCANS. I will talk a little bit about that today as part of the capabilities. Brooke also has a talk on UCANS today at the other track.
I won't go into depth, but I'm guessing people are pretty familiar with UCANS. We can talk more about that offline as well. What is an IPVM or IPVM?
This is the beginnings of the specification for it. Content address execution to content address data and IPFS.
That's the small goal of the large goal of the open world. These high-level attributes are still in place. We have WASM and IPFS, even though things that we'll be running or that we want to interop
with won't just be WASM. We have declarative invocation, which is actually part of the UCAN invocation spec. Captured results or receipts, as we'll call them. We'll talk about that. A distributed scheduler. Things like matchmaking, how to determine who of nodes of machines should run a certain
kind of compute. We just had a talk by Bacalao. They have certain kinds of things they want to run with data science workloads and, as
we saw, stable diffusion. Not everybody's going to be able to run that. We want to be able to distribute the work or the data to the work where nodes can actually
do that, can actually enact what needs to be done within a workflow.

Memoization and adaptive optimization. We'll actually demo some of the incremental nature that we get by running compute and IPFS. Manage effects, which I'll talk about very little in this talk, but kind of where we're really going. I think a big difference, as we did put a blog post out recently comparing to Bacalao
and other groups working on IPFS and compute, and the one big difference, I think, that at least comes out in my mind as we're starting out on this is thinking a lot about how people

program, how do they work with effects, where determinism, item potency, all these things come into play. And I'll talk about that. Also ambient computing, HTTP compute, and a lot of stretch goals. So IPVM is open world compute for IPFS. Whereas Brooke has called it the long fabled execution layer. And I guess when she gave the talk about this last summer, it was more of a fable, and now
it's becoming more real. So that's a good sign. As I mentioned, Homestar is an IPVM implementation. It's written in Rust, and we've been working on that, and I'll link it at the end. But also we'll have the ability to execute WASM on the browser. But a lot of the tools obviously we're using is WASM, IPFS. The system is heavily built around WASM time and their work and the bytecode alliance,
IPLD, and UCAN, as I mentioned. So when we think about compute, at least when I started working on this stuff, thinking

about workflows, I guess in the Web 2.0 space, workflow computing has gotten very popular.

So has anyone heard of Uber's Cadence? Workflow engine? It's a different crowd. It's the used AWS all services crowd. So a lot of the big companies, Datadog, DoorDash, AWS, has their own stuff.

But Uber did a thing called Cadence. Another company called Temporal came out of Uber with another workflow engine, which Datadog
uses Temporal today, plus a lot of other companies. But all of these companies, all of these companies came out of projects at Microsoft called Durable Functions, Durable Tasks, and Durable Entities. You see Temporal there. They had this number of how many people, how many executions they've been running for different jobs. That number kept going up, so I didn't wait anymore when I took the photo of it. So I don't know if they're just lying, but that was pretty good. And then you have Cadence, as I mentioned, fault-tolerant stateful code platforms. So these things are pretty common in the microservice world today. You give them, you write a series of jobs in YAML or use tooling, typically in clients
like Go, because microservices in Go go hand in hand in a lot of cases.
And then you can define patterns. So in Durable Functions, as it has above, you have sequential chains, you have aggregates, you have things that can work in parallel and then become sequential. So these concepts of workflow compute are pretty normal.
And they are all the rage these days in a lot of the big scale space. But one of the terms that really comes out of these things and all these companies that
got spawned from Microsoft have come to talk about is this term called virtual resiliency,
which came out of this Ambrosia paper, also in and around related to Durable Functions. But virtual resiliency is analogous to what we call virtual memory. And the big thing, and you'll see this term a lot in the workflow documentation, is failure

oblivious code. Has anyone ever heard of this concept? No? Cool. So new stuff. Great. So failure oblivious code is really trying to work with distributed compute at a layer where the user doesn't have to get all the failures and have to deal with all the mess. Something else is executing and understanding how things should work and deals with things like replayability, incremental changes.

So I would say the Microsoft paper, and in case, a really great paper by John and Goldstein and co., really gets at how to deal with things like non-determinism originating from outside

of a resilient component. And so that paper really captures essentially how do you deal with work?
How do you deal with it where it doesn't have to flow all the way to the user? And so a big goal for us, and again, differences with some of the other groups working on IPFS
compute, I think is this. We are really trying to gauge at an abstraction to deal with virtual resiliency and failure
oblivious code. And Brooke will talk about that at the high level as well in her talk. The issue with a lot of these companies like Cadence and Temporal and people using these workflow systems is they will have talks and documentation, long sets of documentation

to digest about what is idempotency. Can actually someone tell me what idempotency means? Go ahead. Yeah, essentially. It's about replayability in particular for that. That's really good. Can anyone tell me what determinism means? Right. Are there differences between idempotency and determinism?

Yeah, what are they? Right. That's right. Okay, good. Very good. Very good. And so when you think about these things, we understand these things at a pretty good

level. We're not saying, hey, all you people working on different services and microservices running these workflows, you should all know what idempotency means and what determinism means. What are the differences? And please, oh, please, make sure your code is deterministic so we can replay it easily. And if it's not, we'll try our best. And these things are really hard to think about. These are classic distributed system problems. And yet these workflow engines that people are using every day, the only thing they really kind of try to aim at the problem is documentation and some other specific things. So one example that was told to me is somebody who works at, I won't name the company for this one, I should say that, but who worked at a certain company where they were trying to create a workflow. And so a big use case for workflows is all different kind of RPC calls right across microservices. And so you would think in a workflow, you want to get this replayability. You have one step, call one RPC service. You have another step, call another. The problem you'll see is engineers who actually just copy around templates and say, well,
I have code that has like 50 RPC executions in one step. Which happens, well, what happens when one of those RPC executions fail? Well, the whole thing will replay again, over and over and over again, because one service essentially killed your other 49, right? So getting these things right, how should people do steps in a workflow? How should we engage compute? There's a lot of like devX experience and ideas to think about that make this pretty hard. So also in Temporal, they talk about, you know, the word honest is on you, right? They really try to force idempotency, but they can't restrict you. So there's a lot of discussion there to be like, how do we enforce these kinds of things?
And then of course, they also said application developers are responsible for developing worker programs. It is on you to do this work. And they also say in their docs, activity code, which is actually their kind of key for doing like scheduled tasks, can be non-deterministic. We recommend that it be idempotent. Again, this idea is that we can recommend all these things, but we know that programmers will do whatever the hell they want. So how do you try to enforce these things? You have type systems, you have all these other kinds of things. And it's one of the things that we've been building out IPVM. We're really trying to think about how can we enforce certain properties so people can get certain kinds of guarantees when they run compute. And that, to me, is the real hard problem as you start thinking about interop. All right, so the modes and kind of working with IPVM and what we're thinking about. I talk a lot about replayability. This is something that really does come in IPFS for free with the incremental nature
of it with using content addressing. I'll show actually an example of that. We want to think about things. We want to have properties around things like updating DNS records where you have workflows
that are essentially atomic transactions or something like software transactional memory.
People familiar with STM at all? Philip wrote Haskell. That's too easy. So Haskell probably is one of the bigger languages that actually has an implementation of software transactional memory. And the idea is, again, in software thinking about transactions in this way where if you have to roll something back, you roll something back. One of the new kind of hot programming languages of today is Verse, which is coming out of
Epic, which has the dream team of all the great programming language researchers, including Simon Peyton Jones working on it. And they're basically looking at a verse for a multiverse compute language that will be at its core all based on software transactional memory to do multiple, multiple concurrent
modules running all at once and kind of building that into their core for multiverse programming. So when we think about large scale compute on IPFS, how do people enact these kinds of

workflows and make sure they get the guarantees that transactions give them? Manage effects. Brooks talked a lot about this. This comes also a lot from the functional programming world. And we're still just working on what this is going to look like. But again, these concepts, idempotency, determinism, oh, now we have to have a side effect. Now we have to make a call over the network. How do we move that around within an execution flow? How do we do that? I'm giving you more questions than answers. We have ideas on how this is going to work. But how to get people to understand that certain things that can be deterministic, you will

get the results that you expect. Versus if you add some side effects, we can try to manage them. But you might get an email that happens twice. Because we can maybe only give you at least one semantics. And so I don't know how programmers are putting together all these tasks and workflows together. How much they're really thinking about how these things interplay.
And then, of course, capabilities are a big mode of things. And capabilities, we've looked at it in kind of two different circles. You have what we've been using for UCAN, which is around delegation and authorization, which is a big part of IPVM. But also capabilities of different kinds of nodes running different kinds of tasks. Do you run WASM deterministic tasks? Do you run WASM with WASI to do more side effecting kind of WASM code that you want to do using something from the host? Do you run Docker jobs? Do you run these kinds of jobs? How do we know who should run a certain kind of workflow based on various steps of data within a workflow? Right? So based on tasks. So now, essentially, what we're working on with IPVM are ways to bootstrap all this information.
You come up online. You have your capabilities of what you run. What kind of nodes you are. What your GPU is. You can't just run stable diffusion if you don't have enough GPU. So these things are very important to determine who should run a workflow. And that's where we come in with affinities. So there are certain kinds of clusters, certain kinds of nodes that have affinities. And that's work that we're determining. How do you bootstrap this and have the ability to say, no, you should run this, or we're going to run and delegate parts of these things. So really what this is all going through, all these things. We talk about workflows. We talk about tasks. We're going to talk a little bit about WASM now. When I was writing this talk, I was listening to this Yes song a ton, which is Wondrous
Stories, which is really, really good. And really what comes into play for all this is the goal, the dream that all the Java application programmers had back in the day, which is write once, run anywhere. I still think it is the dream today. And that is a big reason we have gone into WASM really heavily. And so as part of the work, and actually code that's already in place, we've been really focused on WASM with interface types. It's kind of a big community standard of the Bytecode Alliance. We're all in. This does cause some chaos with things like not following some of the bits that companies like Wasm or others might do. But if you're going to have open world computing, you need to have some standards. And so we're focusing really heavily on WIP-Bindgen and WASM tools and these interface types.

And are people familiar with the component model? It's gone through a lot of change. A little bit? Cool. So basically from their words, but you want to define a portable, low to run time efficient
binary format, cross language composition, support ritualizability, static analyzability,

and ability to work with language and not stay with things defined by WASI. It's unique value proposition. This is essentially keeping to WASM's idea of sandboxing, isolation, and computes.
And defining, you know, this is actually the good and bad, right? They've been working on this component model very incrementally. You know, a lot of things, as I've been working in WASM for the first time, as I've been telling a lot of people, it's really great, but it's still pretty far away from like everyday users getting used to it, right? And I'll show, as I get to the demo here in a minute, I'll show you like some WASM code and how it's not so easy to kind of work with. But it obviously gives us a lot of things that we want to use. And so a big thing that's part of HomeStar today and the implementation is we really set
out, well, we have two kinds of things we're all in on. We're all in on WASM component types or interface types, and we're all in on IPLD. So we should have a way to go back and forth between them, right? So today HomeStar implements an interpreter that actually, if you think about it this way, what you actually really get is way more types, right, on the WIT end.

And we can convert all these things back and forth, right? So actually we're getting a more interesting type system for free by having the ability to go in both ways. And that's a really cool concept. We'll just show in code in the conference tomorrow. Another big part of this, as the demo is going to come up right now, is promise pipelining. So this comes out of the e-writes Mark Miller work. This is kind of stuff that Brooks touted and touted for a long time. But how do we show, how do we kind of line up things in the execution graph where instructions, which are the actual things that run within a task, are essentially promised and we can actually create a DAG essentially of how we're going to run those things. And so this is actually a DAG that we produced today out of the code, for example. This is a very sequential one. As you can see, we have these three different batches that run. And each SID is referenced as a task, essentially. It's tied together as this one has to come after that one and after that one, right? There's a causality that is in place. But you can also have parallel ones that all may be having the same input SID from patch zero. And now those can run in parallel. So we have a scheduler that can actually run those things in parallel, while other ones have to have different dependencies. And then we get receipts out of this, right? So with receipts, we have receipts at the different levels. And actually, as we create the graph, we can go back from the bottom of the graph to actually
say, well, I already have the receipt for that third one. I don't have to run the whole execution. But if I have a third one, I don't have to run the execution. And this is a very, very simple thing. And it's a very, very simple thing to show. But it's a very, very simple thing to show. And so I'm going to show you some examples of how we can show this. So here's a graph that I've created. It's a graph that I've created. And I've got a SID of SID. And I've got a SID of SID. If I have to keep going back through, I'll go back through. Okay? And then you see that if I have to go all the way to the top. And then essentially just kind of re-showing the case. What we do with receipts actually is we actually have a local cache using GossipSub. It's a peer with members in our mesh, send the data to everybody. And then we also put it through the P2P for other nodes that are not part of the mesh to have access to it later when they have to go get it. Obviously, they pay a higher cost for that. But essentially, we have this distributed cache that we can use. Okay. So not too much time left. Let me now break for the demo of some of this work.
All right. So we have, I think, like everybody doing demos, we have cats.
Right? Is that like a thing? So we have this app that Brian Ginsberg on our Fission team helped me do the front end for this. He's really great. Thank you, Brian. Okay. So we are going to run up a little flow here. So I'm going to run up a Homestar node right now.

Okay. It is up and running. I'm going to refresh this. We have a WebSocket connection. Cool. And just to show you, like, these are running, you know, somewhat legit, but somewhat fake demo to workflows. So we have two workflows we're going to run that does some image processing here.
We're going to do one. Actually, I have this. Let me do this. That doesn't work on that. Okay. So we have different we have these promises which are being awaited. So you see these await okays. And people can look at the spec more in detail here. But these await okays are essentially saying I'm waiting for another task to complete within this workflow before I do any work.

And this is essentially sequential. And we're going to do crop, rotate, and blur on one side.
And we'll have a second workflow that's going to reuse crop, rotate, but then do something different.
Okay. It will do gray scaling. All right. Cool. So, hey, we're going to start running some stuff. This cat is going to get processed. And boom, we have a crop. We have a rotate. And see before the other one runs, I'm going to oh, my node crashed. Right? And I'm not going to start VS code. Oh, I got to blur. Okay. Let me rerun this. I'll do this a little faster so we can really see incremental compute at its play.

Demo gods, right? Do that one more time. Run up. Okay. Nix, don't run Nix. All right. For James. All right. So we'll run up that one more time.
Waiting for Rust to compile. There we go. Okay. So just to show it again, see if I can beat the timer on this. So we're doing our images, crop, rotate.

I think I killed it in time. Yeah. Okay. Cool. So I'm going to wait just to show you what's going on here, too. We have these nice little icons here that we ran these two jobs. We're waiting for now the client to say, hey, I'm not getting a pong from you anymore.
So we're going to fail out. It's doing that here shortly. It's going to tell me that blur is not going to work. There you go. Boom. I get an X, right? I don't know where you are right now. Okay. Let's rerun it. Server's back up. Whenever Rust tells me it is. There we go. Okay. Cool. And we're peered. Okay. Great. And so I'll go back here. So we want to say, hey, can we rerun this thing again? Oh, it's much faster for those. And then we're just going to compute blur, right? As we see over there. Okay. Now, workflow two already uses two of the same computations that we already have.

Those happen really fast. Now we're going to grayscale. Boom. Okay. Demo works, right? So, you know, the key in this is that, like, in this code, and again, I'll go through the code in the young conference a little bit more.

This is like legitimately running a scheduler, legitimately running things in parallel, creating a graph, creating a DAG.

We're using, you know, the Tokyo runtime to do a lot of work here as well for doing how we schedule parallel things.

And so, you know, maybe it's not the coolest demo in terms of just, like, what we're doing with the images.

But really working across compute and thinking about how to really get developers to get the guarantees and decide what they really want to do with code is really where we're going with stuff, right?

And I think, again, highlighting the differences from a lot of the other groups working in this space.
Yeah. Demo. Good. Okay. Cool. All right. Actually, I'll go back to that for a second. So in the world part of this, and I think the part I really wanted to get at as we're a community here, is, you know, in the system of running steps, running compute, doing this open world, interoperable, you know, form of computing, how do we do a plug-in system that really works where you can bring in a bunch of people and say, you know, we're going to do this.

You know, it really works where you can bring in your Docker task, and maybe you have certain, you know, we can say, like, Docker tasks are all, like, side-effecting, but they might not touch that much of the host.

They might not touch that much of a network. How can you tell us, hey, I have a certain kind of compute or a binary that I run, whatever the case might be, and I want to make it somewhat restrictive, or I have certain properties that I want you to do?

Do you annotate it? You know, I've gone back and forth with Brooke on these things. Do people come up with annotations for a task, but what happens is a big issue that the Ambrosia paper talks about, and people I know at Microsoft have talked about when they work with durable entities, is they would have an annotation system, and then the people would write the code inside of that, inside of whatever's being annotated.

They say it's monotonic, or they say it's deterministic, but the actual code isn't. You know, somebody told me once, a developer was telling them, oh, I get a user record from the database.
That's deterministic. What about the timestamp? That's not, right? And so, because the user record is just a view, right? And so how do we really come up with the right ways to handle those foundations? I think that's the really hard part, the real hard computer science part that really comes into play with all this stuff.

So just to kind of wrap up, we have a lot of components. I put a lot of work into our Figma for doing this. I wanted to show it. And things that we're working on. So we're thinking about workflows. We're working through, like, what we're looking at the runtime. I've talked about bootstrapping, discovery.

I can use the highlighter part. Isn't that cool? Discovery, scheduling, parallel versus sequential execution with promise pipelining, registration capabilities,

all these kind of things, matchmaking affinities, receipt storage. And the stuff that's, like, not transparent is stuff that we're really close to already finishing today.

And so obviously stuff that we're still working on. I talk about this plug-in substrate, right? That, to me, is the real interesting part as a community. We really come together. Because we don't want everybody forking a Homestar node and writing their own piece of this, right?
Even though we'll support WASM and some other things as first class. We want to be able to have something that just fits in with the substrate.
We talk about the WASM part specifically. Interpreter, SID ahead-of-time validation, WASI components for doing some stuff internally, browser support.

We want to obviously execute WASM in the browser. And then, you know, Brooke will talk more about managed effects. These are a lot of the ones that we're thinking about building in-house as part of the kind of first class system for Homestar.

And, yeah, the end is only the beginning. You know, I hope the next few days at the conference, people, as we show some of the code and go through stuff,
that, A, IPVM implementation of it is really happening. And that there are a lot of really great open questions that we should have some powwows on. So, awesome. Thank you. Thank you, Zeeshan.

Amazing presentation. We have time for one question. There's only one question. Somebody else can go. I just had a quick question. If you go back to the slide with the architecture diagrams. Yeah, let me go to... I think it's 24, I think. Oh, just like the last... Yeah. Okay. So, how do you define the run time of actually running those things that is kind of outside of the VM, in a sense? Like, where do you draw the boundary in the run time? Because there are, for example, things around the network and so on that might be outside of the VM that you sort of plug in.
Yeah. How do you draw those boundaries? To me, it's more about the run time essentially is what the node interacts with.

So, for me, it's like... Well, I would say even the run time is something that... When you think about discoverability, it's also networking-based. So, the run time is what essentially is also understanding of how to bootstrap, how to get on the network, those kinds of things.

So, the networking part of it is obviously a separate piece, but the run times have to know about that interaction point.

So, I put it in the bucket for part of the run time. That's how I decided to find it. Yeah, discoverability. One thing is we already have a pretty good story for how to build the bootstrapping layers.
But hard parts of evaluation, when you think about... As we start thinking about more and more kinds of workflows, you really start getting into... Okay, you have enough GPU to run a certain kind of compute.
Now I'm going to give you the workflow. You can run it. Maybe the data is also more local to you. So, here you go. Run it. And then maybe at the point of you running it, you're like, I got really busy, and I don't have enough CPU or GPU anymore.
So, now I have to move that workflow somewhere else. So, it's a classic routing problem. I used to work at Comcast where we did this all in network with semantics on the packets.
But I think one thing I really could think about is how to move around workflows and still get the guarantees.

I mean, you hope with workflow systems, like when you look at temporal and cadence that people are using at big scale companies and stuff that AWS does, it's all tied to SLAs.

So, if you want people to have a compute system, they're probably also going to bring their SLAs to you.
And that's a problem. Performance matters. Movement of data and movement of code matters. If you can cache more things, that matters. And so, I think that's a big – for me, when I think of workflow engines, I think of at that scale that the big companies go.

And that's what we have to build for. Yeah, I'm around, so you can find me. Cool. Thank you.

