
# Pando: Notarized IPLD Data Network - Taosheng Shi

<https://youtube.com/watch?v=aYlrSxiUNyI>

![image for Pando: Notarized IPLD Data Network - Taosheng Shi](/thing23/aYlrSxiUNyI.jpg)

## Content

Thank you. Hello, everyone. I came from China. And this week I know much of faircoin ecosystem

in China, lots of providers and IPFS funds in China. But there is there are not much

China engineers come here. It's a pleasure to come here to exchange the ideas about our

sound source on IPFS and faircoin and IPRD database related topics. I wanted to say,

have you been to China before? Here, no. I have to the organizer of IPFS things and

IPFS camp. Hopefully, next year or someday we can have IPFS camp and IPFS things in China.

Thank you. Today I will introduce the Pandwa project, the notarized IPRD data network.

It is a kind of layer two data network of faircoin. I'm Tao Shen. So maybe you have

known the Pandwa is a famous organic trace in the world. But in the context of data network,

Pandwa here is a one tree forest. That means Pandwa will aggregate the huge forest of IPRD

CAD data around the faircoin and over time. And why Pandwa? In context of the big data

systems, Pandwa is for the data that does not meet the consensus in the global team.

As you know, for the global blockchain such as faircoin, they have consensus for other

nodes to reach the agreement with the data. But for lots of streaming of the data, they

don't need to meet the consensus bar of this global team. So in the context of the big

data, we have a consensus bar. As you can see here, it is a big data iceberg. There

are many, many data under the consensus bar. So that is the scope of the Pandwa.

The Pandwa project is born from a reputation system of a faircoin network. There are several

mechanisms we can see for incentivization feedback loops for the open data network,

for example, faircoin. You need a reputation system to feedback the control system. You

can see here the input and output. If you take the faircoin network as a control system,

so Pandwa is a layer 2 data network for a faircoin reputation system. There are more

than 3,000 storage providers in the network country, but the storage clients don't know

which storage provider is better, how to choose the right storage providers to store their

data or retrieve their data. So that is why we built Pandwa for reputation system, to

build a feedback closed loop for this faircoin open network.

And from a high-level point of view, you can see Pandwa as a set of metadata service, a

layer 2 data network of faircoin. You can see in this web3 data network architecture,

Pandwa is here in the three layers of IPFS and faircoin. We have faircoin nodes as a

blockchain, and we have IPFS nodes as offchain nodes, and we can also have Pandwa for offchain

storage service. And based on this storage layer, we can have a storage helper and logic

layer, for example, computation over this data layer. So that is the big picture of

this web3 data network. There are nice properties of Pandwa to serve

as a layer 2 data network. The first one is to keep the included metadata consistently

available, because there is much discussion in the blockchain area about the data availability

layer in the ecosystem. So we need this kind of data availability layer for faircoin.

And the second is we need lightweight and unbiased access to the metadata. I will discuss

the details of how to provide unbiased access, and also with the third property, discouraging

historical revisionism. All the metadata history in Pandwa is immutable, and we can trace the

history of Pandwa's data recording. How does it work? The pipeline of Pandwa is

much simpler. You can see it as an input and output system. For example, the first step

is to take the stream of the data, and then we aggregate this data into the archives,

and also we store the data in the Pandwa storage. The Pandwa storage is much similar to IPFS,

but has an immutable data layer. Currently, we provide a lightweight query

interface, a GraphQL and HTTP access API, and then we can back up this data to the faircoin.

The latest discussion about the backup is we will leverage the data dosks, not only

use the X-Ray projects, maybe you know. And inside the Pandwa, we had a snapshot chain

to track the changes and the histories of the data. For example, you can see we have

a low-layer HMT data structure, and we have a kind of block cheese to persist the history.

Okay, let us see how Pandwa works, what is Pandwa in different scenarios. The first one

is in the storage hierarchy. As you know, faircoin is for slow, large capacity, and

very cheap for backup. IPFS is kind of warm storage for data retrieval and for data delivery

network. But Pandwa is an SQL-enabled data network, so it is optimized for faster access.

It is a new database kind of database. So we think that we could introduce the SQL capability

to unlock Web3 applications. The next scenario is the repetition system.

You may have seen this picture very often in different places for the whole picture

of faircoin and for storage and retrieve the pipeline of the data. But here we added the

Pandwa to the data pipeline. You can see in the left part of this diagram that is repetition

systems and the repetition service. Pandwa is a storage layer of the repetition system.

You can see from there, just like I have mentioned before, the storage clients and the retrieval

clients will get the metrics from the repetition system to decide to choose the best or better

storage providers. You know, faircoin network is different with

other blockchain networks. It is the storage service network and the retrieval service
network. It's different from Ethereum and Bitcoin. Different storage providers have

different requirements and different benefit requests. So they have different services.

So repetition systems could give a list of storage providers for the storage clients

and other stakeholders in this ecosystem. Okay, next one is currently much popular with

the FBM launch. How Pandwa is working with the data DOS stack.

As you can see, we will introduce data operating system between the layer 2 data DOS and the

layer storage. For example, you can see we put the Pandwa, Faircoin, and IPFS as the

same data storage layer. We can leverage the FBM to build a data operating system for a

lot of frameworks for smart contracts. And we can enable more data DOS in the up layer,

layer 2 data DOS. Pandwa could be a service as immutable data storage for the data DOS.

Okay, next one is Pandwa in the modern data stack. That is quite an open area I want to

discuss with all of you because how to build the Web3 big data systems or Web3 data analysis

pipelines is quite an open area to discuss. But in my view, as you can see here, basically

we have the raw data and the transformed data and we have the ETL process over this raw

data, that means over this Pandwa data layer. Currently, we are doing some prototype based

on Pandwa that is transform 2 based on HTAP database. HTAP is a hybrid OLAP and OLTP.

That is quite a new area and we have much issues and open areas needed to discuss and

needed to figure out. But I think that is quite available for Web3 big data ecosystems

if we can build a transformer 2 based on IPRD data layer. That could be very interesting.

But Pandwa here is as a data layer for this transformer 2. For example, Pandwa could create

a chain of evidence for the input and output and the computation and the process, for example,

and could transform the data in the pipeline but with very nice properties of transparent,

verifiable and reliable. Currently, we are doing the prototype. We call it PandwaPlus

over Pandwa. Okay, let me give a short introduction about the HTAP transformation, the principle

of the HTAP transformation. Basically, HTAP combines OLAP and OLTP together. As you can

see, the left side is for OLTP, the right side is for OLAP. There is a step 1, 2, 3,

4, but for the raw star layer, the data star will be synchronized with the column star.

That means the raw star will be real-time sync with the column star. That could make

the OLTP, OLAP very quickly. So there is much context for this HTAP transformation. That

is the latest progress in the database area. Okay, I want to summarize why Pandwa is useful.

The first is, as I mentioned, we introduced the computation layer over Pandwa for execution.

Pandwa could execute a distributed HTAP process, which could be distributed as a task on the

Web2 infrastructure or backlog infrastructure. And the second is data verification. That

is the benefit from the container addressable features. For data input transformation process

and output, all these pipelines are built on the container addressable data. And for

linearity and provenance, we could certify data processing and data products and pipelines

by enabling chains of evidence. And based on the Pandwa, this kind of Web3 data network,

we could build cross-function teams, organizations, for example, multi-dollars, and cross-domains

on this verifiable data. So that is our source and our progress. We have the open source

GitHub, but I didn't put it here. But you can find the GitHub open source projects on

our official website. That is all the key information I want to share to you. I am open

to discuss this open area. Thank you all. Thanks. I think probably a lot of people in the room would be curious to hear about the places, like the mechanics of how content addressing allows you to do some of the things you talked
about. And so there's a couple that I took notes on that would be fun to hear more about. And maybe there's more other parts of the data motion that do similar stuff. And you

mentioned a little bit here, but in the execution pipelines, are you using determinism and memoization

and caching to accelerate the second run for repeatable compute?
Currently, for the computation to accelerate the repetition computation, you mean?

Yeah, I mean, just in general, for pipelines, for the data stuff.
Yes, for the computation part, we are leveraging the open source GitHub database to build the

over panel. We get the data from panel and calculate the aggregation data for the multi-dimension

data to understand back to panel. We can re-compute the process because we have the CIDs and the

output is also CIDs. We can verify the computation output.
Regarding to the cache? Are you able to cache the output based on the input?

Maybe. I think it could be a cache layer for the computation. Yeah. We can think of that

as a cache layer. Right. I think that that's a sales point for all of our stuff is that we can make repeatable compute fast. Yes, yes, yes. And then on the HTAP synchronization, I think

it was the next slide, moving from the row store to the column store, are you doing in

data in number three? Is that just CIDs? So if it's the same for the field, then it's

in zero copy or how does that work? Currently, we package all these processes

together. And the next step, we will slip this step. Currently, step three is basically the Web2 technologies. Different introduces the CID

because for performance consideration. But in future, we will make it separate service

and separate node to separate synchronization. And the leverage is CID. You can verify the

input and output of step three. Yeah. I guess, yeah. And then it would be interesting

to hear just a little bit about the operational challenges for doing the Filecoin scale stuff
with reputation and all that. For reputation data, basically, the first

thing that you can think of is the unchecked data. ELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEELIPEEPEUEPEE
Basically, the unchecked data is big data. Currently, it's about over 70 tick-back. And there are lots of

often data we are cooperating with the proto labs, for example, retrieve metrics, lots of often metrics

we need to also integrate to produce the reputation metrics. I think that is, combine the unchecked data with the often data together,

I think that is a big data scenario for Web3. Thank you.

Thank you. Thank you.

