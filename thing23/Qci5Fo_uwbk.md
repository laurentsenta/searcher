
# Repco - Exchanging community media and metadata over IPLD - Franz Heinzmann

<https://youtube.com/watch?v=Qci5Fo_uwbk>

![image for Repco - Exchanging community media and metadata over IPLD - Franz Heinzmann](/thing23/Qci5Fo_uwbk.jpg)

## Content

Hi everyone, I'm Franz, I work with the small workaround collective in Freiburg, Germany

called Arso Collective and today I'll talk about a project we've been working on in the past month called RepGo. The project emerged from some quite long going discussion between different community media

outlets throughout Europe. So what is community media? When we talk about community media we mostly mean small media outlets that are not commercial

media outlets and that are not public broadcasters, state affiliated media.

Community radio stations, independent journalistic publishers, DIY video sites, archives of social

movements, all these smallish outlets that you find throughout the internet where people publish media, create media, often rely on volunteers or on little funding, very DIY

focused often, grassroots based and democratically organized usually.

This kind of publishing has some particular challenges, especially in the technical part

of it. They often have little resources, so no big funds to invest into big solutions of their

own and also they often, out of different reasons, don't want to rely too much on big

tech companies like YouTube and the other big media platforms because they often want

to keep their independence and not rely too much on external infrastructure out of experience

that were made. So this project in concrete started about a year ago at a conference in Austria where

a few outlets came together. The cultural broadcasting archive in Austria is the biggest of them and also Freie Radios

Net in Germany, which are both platforms that emerge out of the community radio movements

and that collect media from different outlets. And these platforms and a few others like Media CCC from the Chaos Computer Club, some

people were there and also some people from Spain from a radio platform, they had this
long-standing desire to get out of their bubbles and not have these separate platforms because

that makes it really hard to compete in a way with big tech platforms because that's what you do in the end if you want to reach people and if you want to build up audiences.

All these platforms are based on quite old tech stacks usually, some use WordPress, some
use custom, very old PHP code. There's some more modern tools like some podcasting tools, one is called CastoPod that use ActivityPub

for a federation, also Peertube. But so far there is no tool that allows them to collaborate on a shared repository of their

media and to transfer audiences. One of the features that was imagined in the beginning is to have on the existing platforms

embed widgets in the sidebars of something where you'd reference new content from the network and you'd have similar content to something that is displayed there to be able

to exchange audiences and to engage in better ways with people across these existing platforms.

On the technical level we have very different tools that are used, we have different standards,
like the most common likely is RSS that is used to syndicate content.
RSS works quite well and for a very long time but RSS also is a very limited specification

and most of these platforms when they use RSS they add custom things on top and there is not many standards. With ActivityPub actually it's kind of similar even if it's a much more modern project that mostly when you talk about ActivityPub people mean Mastodon, which is the biggest project,

but there are others like Peertube or CastoPod that have custom extensions to the ActivityPub

protocol to add more media style metadata and so on.

So out of all this the Repco project was created with the idea to aggregate content from these

different outlets to replicate repositories between nodes, which also serves as a way

to back up things. The challenge for these smallish outlets often is that once funding runs out or once people

start to do other things we have these repositories of content in the internet like these old Web 2.0 or Web 1.0 sites, but they are not forever usually.

They drop out of the internet after 5 years or 10 years and then things actually get lost this way. We want to aggregate the content, we want to preserve the content and to replicate it
between these nodes. And then on top we can create shared indexes to offer better ways to browse content through
different platforms and a future thing also is to connect services that add more metadata

or value to these platforms like automatic transcription of audio and video or translation.

The results of this, the initial plan should be embedded back into these platforms because

they all didn't really, out of many reasons of course, want to put all their weight into
some new tool that would solve the problems they have but instead expand the existing

offers part by part. So basically we in a way want to bridge Web 1.0 or 2.0 apps into a new format that allows

to seamlessly exchange the data. And for this we, after some evaluation, decided to build on tools that are a big topic in

this forum here. We decided if we do this we want the data that we ingest once it's in the new system

to be authenticated, to be self-verifying and to be able to replicate and exchange it

in a trustless way. So the basic workflow that we came up with is we want to ingest the existing content,

store this in the original format first of all, assign it content IDs everywhere and then map it onto a shared data format to then be able to query it in a common or in the

same way no matter if the data comes from RSS or comes from ActivityPub or whatever
other data sources likely will be developed in the future. One problem that we, or one challenge that we thought about quite a lot early on is how to deal with IDs. Content IDs are nice but they are immutable and they only can refer to a single revision.

We needed of course also mutable identifiers because things change and these existing platforms
might have updates. What we came up with, which I think is quite common and works out quite well, is not to
have a new single ID for each entry but to have entity URIs and there can be multiple
of them because sometimes the content that we ingest already came through a couple of
different services on the way and then there might be multiple identifiers.

We basically store all of them and whenever we import something new and when we find an
existing match we assign the same internal ID to this content.

How does the system work? I'll just go through a couple of examples. We have the existing data sources which might be RSS, ActivityPub, also a WordPress API
is something we built an adapter for because quite a few of these platforms use WordPress and WordPress offers a REST API. RDF also, we don't have something for that yet but this is on the roadmap, especially
once you move more towards public broadcasters and bigger media outlets there are some established

metadata standards in RDF. First we fetch the content and then basically like a caching layer we store the content

exactly as retrieved. This allows us in the next step to map this original content onto a shared data format.

This actually was one of the longer running things in this process to arrive at this shared
data format. This certainly will develop further because we store the original content we can iterate
on that because we can run a remapping step that would just go through all the stored source records and recreate the mapped records on top.

This already proved quite valuable to be able to iterate without contacting the original
APIs again and again which can be quite slow. Some of them have rate limits and so on. Now we have the set of source records, we have the set of mapped records that have the
custom types here, we have content items and media assets.

And then we do something that is I think becoming something of a common pattern in many of these

projects building on IPLD and IPFS tech these days. We store these records and we call it commits where we sign the list of content IDs included

in a commit and add a UConn proof to validate the capability.

This basically makes the system multi-writer because each repository, the basic organization

of the content is repositories and repositories are identified by a key pair basically.

We use decentralized identifiers but only support the key pair method.

Maybe we'll also, we're not sure about if we actually need the DID abstraction because we only ever use key pairs so far. But anyway, we have a key pair that identifies the repository and then through UConns we

can create additional capabilities that allow other key pairs to publish into that repository.

And this basically allows to have different writers, so one key pair per device that is

allowed to write into the repository. And yeah, so the commits are just a list of content IDs plus a signature plus the UConn

proof. And in the end, this repository then is basically an append-only log of commits.

And after each commit, the repository has a new head which is the content ID of the

latest commit. And this is a very simple structure that makes it very straightforward to replicate this

content from one node to the other, which is what we do.
Oh, there's one slide in between. So replication comes in a second. What then happens at each node is that we index all this content, no matter from what
repository. And this is basically a two-step process. The first step is the lower data model as we call it, where we have the entities, the
extensions, commits, repos, the UConn proofs. This is basically independent of the data that is actually in there. And then we have the domain data model part, which is the shared data model that was or

is being developed in this project. This is in a way specific. We're thinking about adding like extensions points to this. This would basically be the step to make this whole project be usable for other things as
well. We did not do that initially to keep the abstraction low and to first get our product ready, but
this would be quite straightforward likely to add. And this is currently then indexed in a SQL database, which through some tooling gives
us automated GraphQL queries. This is all public read-only data, which makes it quite simple to query it from frontends

then. So basically on each node, there's two APIs to talk with.

The first is the replication API, which is a very simple HTTP API where the repo is treated

as an append-only log and you can just say, give me all new content since the last commit

that I know of, and then the node replies with the cost stream of all the commits, the
proofs, and the content within. And on the receiving end, everything is validated, the signatures, the proofs, and so on.

So there is no trust involved in the transport. And the other API that we have is a GraphQL API that translates to SQL queries that is

used in frontends and the embed widgets that I talked about that allow to, on an existing

platform, have this sidebar widget or so on where you display new content from the system.

All this works very well. However, and this is like the crossover to the data transfer part that we've been thinking about a lot recently, is we have this split in these APIs. We have the replication API that is very simple but is verified and authenticated end-to-end.

And then we have the querying API, which is very expressive and quite simple to use.

You can have this nested GraphQL queries with filters and selectors and so on, but this is all unverified. You post a GraphQL query, you get back JSON, and that's it.

And this is a gap that doesn't matter too much for some use cases.
However, once we get to bigger repos and also once we want to move more logic to the edge

or to the client, so currently the nodes are more imagined as like classic server software
that runs somewhere, ingests content, talks with other bigger nodes, and then clients
and frontends use the query API. We'd like to bridge this gap more, to be able to do better caching and to push validation
and authentication to the edge or to the client.
So what we're thinking at the moment and what I think is interesting also in all this data transfer talk and discussion is we're thinking about having the query API only ever return

content IDs and then have the data transfer protocol resolve these content IDs.
I think this is something that will also come up in other contexts, because for example
in IRO there was a lot of talk, we want to only ever just exchange lists of blocks and

hashes and nothing else. I think it's a quite interesting point to think of the query layer as something separate
from the data transfer layer and have the query layer be app-specific. So there are also, even with dark queries, there is some expressiveness to it, but also

often you will deviate from the strict selector model that you can do with IPLD selectors.

You want app-specific queries, or like full-text search for example, that doesn't align to
graph queries at all usually. So what we're thinking is having a query layer that is separate from the data transfer layer, but that returns only the content IDs of the content within, plus potentially some metadata,

like snippets for full-text search or something like that, and then use a data transfer layer

to exchange the actual content. And then the query layer will likely be quite app-specific and very different between different

apps, however that data transfer layer could then also be something where different apps can collaborate on the most efficient strategies. And once you take out the query part a bit more, it becomes a much simpler layer.

And this was one of the things we're thinking about, not yet working on, but thinking about
how to move this further in the future. Because then if you get the results of the query, you can also decide locally, or look
up locally, what part of the results do I already have cached or stored locally, and
then only fetch the rest. And then also this opens the door to fetch from multiple parties the results.

So this is the data transfer things we're thinking about in this project.
We have also some other future ideas. We want to have persistent subscriptions on the repository so that via also a simple HTTP

API you can subscribe to a repository and get notified of changes, which makes it very
easy to integrate other services on top, like transcription or translation services that
would listen on incoming things, create the jobs, and push back the results into the repo.

And for these jobs, we're also quite actively following what is being discussed in the UCAN
invocation drafts. I think there is a talk in the next days about IPVM. This is something we're very curious about.
This is it. I want to show you a very, not a demo, but just a quick impression of how things look
like at the moment. This is the demo instance that's running at the moment. We have different nodes running. This one indexes content from this cultural broadcasting archive in Austria.

This is another one that indexes content from the Freie Radios net, the German radio network,

community radio network. And then we have a shared instance that replicates the different nodes together.
The front end is still a work in progress. But basically you have this browsing interface where all the content floats in.

You can listen to audio and have some filters. This is all still a work in progress.

But the basic system works and once things flow together from the different nodes, it's all authenticated and validated.

Of course, because most of the content comes from external services, there is an initial point of trust basically,

where you have to trust the node that does the ingest part. And the idea there is to make this software very simple to run so that it can run socially close to the source.

So not necessarily technically, but there are social trust relations towards the ingest nodes from the original creators.

And then from there on it can all flow in the authenticated and validated, verified ways.

Yeah, that's it. Thank you. If you have any questions.
So on the query with proofs thing, I'm just trying to imagine how that works in my head. Are you able to use a generic full-text index

and then find the relevant CIDs and send those back? Or do you need to have an IPLD-aware index?

Yeah, this is basically the idea. We'd have a regular, any full-text search engine.

There's some quite interesting Rust projects in that regard also, if you don't want to use the big and resource-intensive Elasticsearch,

which is kind of the standard in many places. And then those would index the content.

You'd likely have to do some schema mapping, again, to do that in a nice way.
And then for each document, not necessarily the CID, but likely the mutable ID.

And then through the existing database, map that to the CIDs. And then return the list of CIDs, plus likely some metadata structure that would include something like snippets or scores.

And then you could locally just fetch all the SIDs and then store them through the data or replication API.

Just two things. One is this project is so awesome. Before I was doing this stuff, I had about a 10-year career in the nonprofit sector

and intimately familiar with the disappearance of data in small organizations with low tech competencies.

So it's pretty cool. And then the other thing is just the pattern you talked about with the query protocol.

It's interesting. Were you at IPFS thing a year ago? We had a name that was proposed, the manifest pattern,

which is like rather than do a data transfer up front, do a query to get a list of CIDs and then go from there.

There's some challenges around verifiability, but it's definitely a good approach.

And it also makes multi-party data transfer pretty accessible.
I think it also would align quite nicely to something like persistent queries, like a publish-subscribe system,

where you subscribe to some topics or a subset of the data and then get notified of new CIDs and then request them through the usual means.

Thanks. I might have missed this, but when you said the ingest, do you crawl these RSS feeds from people who have agreed to get it,

or do you just crawl what you think is valuable? Currently, these public demo instances, the data that is crawled there was part of the project.

The whole thing started with some funding from the European Cultural Foundation that wanted to connect different media outlets.

So these are all part of the project. How this would evolve is, I think, an open question.

You could treat it as a general RSS feed reader thing, where you just put in any feeds you wanted to.

The idea so far is more that it would be, as I said, socially close to the publishers.

So how exactly that would turn out is not defined yet. Thank you very much. This is a young project. It's still quite prototype-y and alpha.

If you know people or are yourself interested in advancing this further, please contact us.

Thanks.

