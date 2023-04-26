
# What is Rhea? - @willscott

<https://youtube.com/watch?v=p89i9_AskIw>

![image for What is Rhea? - @willscott](/thing23/p89i9_AskIw.jpg)

## Content

Cool. So what is Project Rhea? Rhea is an effort that a bunch of people around Particle

Labs started on in about January. And it is an effort to sort of take another iteration

at the current ipfs.io gateways. So to understand that context, like I did on the keynote two

days ago, I'm going to dive into what is going on with the gateways so that we can understand what we might want to change with them. So I had a picture that looks sort of like this. And this is the sort of bulk of requests that are coming in to the gateways right now. And

so that's where it's meaningful for us to take some effort to try and change it. You've
got a bunch of requests. There are a set of different actual requests that get made. But

sort of the most common ones look like ipfs slash sid. Some of these have some sort of

other things going on around them. They may use an IPNI domain where they've got a custom
domain. And so we have to consider and factor that in. There's some edges that go over and
ask for API v0 and some other sort of edge cases. And we may have some things around

the sid. So some requests ask for a raw block or a car file version of content directly.

And some requests will not ask directly for the sid, but will use the sid to point to

sort of a directory and then ask for a file within that directory. So they'll have a path afterwards. These requests come into a load balancer. Right now, protocol labs for the

gateway that we run at ipfs.io has seven points of presence in Equinix data centers around

the world. Requests will come in. They'll hit an Nginx load balancer that will distribute back to a bank or a set of Kubo IPFS nodes that then handle fractions of those requests.

These IPFS nodes right now are provisioned with pretty big caches. Think, you know, at
least hundreds of gigs of block store. And they're actually serving many of these requests
directly from their disk. So a lot of requests where we're having those direct IPFS nodes

just sort of they already know what that sid is and are able to directly serve it. And when they don't, they act like a normal IPFS node. They go to the DHT. They figure out
other clients that might have it. Or they use, in many cases, their existing bitswap connections because we've got relatively high connection limits. So they stay connected
to many peers. And so they'll ask over those ambient bitswap connections to find other IPFS nodes that have the content, fetch it over bitswap, bring it back. The other thing

to think about, right, is where is this happening? If we were to access the gateway today, the
closest data center that there is an ipfs.io termination in is up in Amsterdam. We're down

in Brussels. But when we look at the route, if you were to trace route, you would see the first thing that happens is that traffic goes to the data center that's about five

blocks up from us. So there is that data center that you can see in yellow up there. That's
the Brussels commercial Internet Exchange. Our traffic is going from the final metro

fiber that exists in the city, terminates and transfers over to a long distance carrier

that then takes it up to the Equinix data center. But if you were to look at most other CDNs, Cloudflare, Netflix, et cetera, they're going to terminate right in the Brussels city center. And so if we're thinking about how do we make this faster, the thing we'd really like to figure out is some structure for your request to ipfs.io to also terminate at that
local data center or that local Internet Exchange. You'd like to have the five, ten millisecond

termination and response to your traffic, especially for common files, rather than that taking 15, 20 milliseconds here. And in general, people will say something like up to 50 milliseconds

to get to a data center for your region. And so in the structure we've got right now, where

we've got these large block stores that sort of need to live in a data center, if Protocol

Labs was to go and say, okay, instead of having, you know, seven points of presence, we're going to, you know, get 50, we'd still be in big data centers, we'd still be 50 milliseconds away from things, and it would cost a lot. And so we need to figure out is there some way for us to get this faster speed without it, you know, being orders of magnitude more
cost to just have that centralized infrastructure. So what would a decentralized alternative look

like? How do we find ways to get to a lower latency for most requests without it being

us running a lot of NGINX and a lot of Kubo nodes in not only data centers, but also these
metro city-level POPs or Internet Exchanges? So the good news is that from last fall, Protocol

Labs launched a CDN, an incentivized and decentralized CDN called Saturn. And so we might think we

can put in Saturn instead of this middle layer of NGINX and Kubo, right? We've got on the

order of a couple thousand nodes there. They're much closer to the end users from the measurements

that we've got. And so instead of having to hit that, you're hopefully hitting some local

Saturn endpoint, and then you'd like that Saturn endpoint to be able to fetch from the backing IPFS node, Filecoin node, or other source of content address data. So that's

been the basic sort of path that we've been trying to follow is how do we make Saturn work to replace this NGINX Kubo mix that we've been using as Protocol Labs for our gateways?

This is a lot of people that have been involved in this. I probably missed some, but I just

wanted to try and get as many of the people who've been involved in doing work on this as I could up here, because it's a much bigger effort than me. I'm just trying to provide
sort of a general summary and direction. So we got given three sort of joint goals or

goals to jointly optimize around. It's not we're going to do one thing, but we want to
do a combination of these three, and we've got some flexibility. One is that we want Filecoin retrievals to become a first class citizen, equivalent to IPFS retrievals that

the gateways do today. So right now, when we look at that path of NGINX going to Kubo,

we've got BitSwap really managing a lot of the peer selection and how things are going
to happen to get that origin content. And so we need to find some way where we can think about getting content both from Filecoin storage providers, but as well as other IPFS peers,
where those are sort of equal choices. We want to make that more equal than what the
IPFS gateways are serving today. We want to validate Saturn as a CDN. So we want to have

real traffic going through, prove out with this IPFS gateway as a customer of Saturn,
that Saturn is able to live up to that CDN thing on this scale of traffic. And we want
to reduce the cost of having these central, relatively high bandwidth Equinix deployments

that are serving IPFS.io today. So over the next 15, 20 minutes, this is the

set of things that I'm going to try and cover. We've talked a little bit about what is RIA. Hopefully you've got a sense of sort of the general trajectory that this set of work is encompassing. I want to talk about what we've been thinking about with TrustModel. I think
that's probably the, I don't know, hairy, but like the tricky set of problems is what

are we trusting, what are the issues around trust that we've run into, because that ends up being a lot of what drives design. How we're going about fetching content. The performance
that we've got so far and that we're sort of looking at. And then I'll finish by going
and connecting the various components that are getting built, what we're building, to try and provide some better context for the rest of the talks today.

So let's talk about trust. When you go and get content in your web browser from IPFS

today, those rendered pages that you get back are not always a thing where you can say,
okay, I asked for this SID, I got back these bytes, are these bytes actually equivalent to the SID I asked for? I asked for a SID of this directory, I get this nice rendered

HTML page showing the files in the directory, but what is my client going to do to know
that that rendered HTML page actually matches the hash of the SID that I asked for in the
first place? Because there's been some HTML templatey thing that's applied, right? The

SID that I asked for is not the SID of this HTML page, it's of a Unix directory, and then
all of this prettiness got returned in the page to make my browsing experience nice,
but that's very different from actually direct content address data being returned to me.

And so then we can say, okay, well, what's different? Could we do better? Like, should

I be able to verify on the client? And so there's two things to think about here. One is sometimes we decorate the responses, like that pretty HTML page, but then also we'll
render a file, and so you'll potentially lose out on metadata blocks. So if I ask for a file on the gateway and I get the image back, the thing I'm not getting, you can look at like the car and the actual raw underlying content address data that that SID maps to,
and you can see, for instance, this is a single text file, and there's three blocks in here.
There's a raw block, which is the actual data, but we're rarely having the gateway actually

directly ask for that hash of the raw data. Instead, it's the file below that, which is this DAGPB node that contains the metadata around, okay, this is a file containing these

bytes, but it has permissions and it has some metadata around it. And so that's the SID
you actually get given after you IPFS add. And so if the file is large, it may actually
have multiple chunks of data. So you're asking for a metadata SID that is the top of the

Merkle tree of your overall data. You're getting the rendered bytes contained in that file.

And from that, are you going to always be able to reconstruct back up to the SID that you asked for? So right now, you could maybe guess. Our chunking isn't in general that

smart. So you could say, okay, I'm going to try and also re-chunk these bytes I got given at every megabyte and then see if I can reconstruct what a metadata object might be and see if
I can get to the same SID and I got the right response. But you would have some false negatives.

You would maybe try at different chunks to see. And so you might be doing a lot of unnecessary computation. And in general, this seems pretty sketchy as a way that we would try and do
it. And so then there's also cases where you just can't. Where especially if you ask for

like a range request, so you get part of a file, without getting all of the file, how
are you going to know what that top hash is going to be? Because you haven't been given sort of enough of the middle pieces, only the bytes of the file, to actually efficiently
have any chance of reconstructing up that this is really the right part of the movie that you were asking for. The other thing then is we don't have any real signal. So

when we think about, okay, so the client gets these bytes and the client is going to, you know, if we were to implement a client side validation that it's getting the right things by trying to reach chunk and trying to make sure it's got the right SID, we don't know the client's going to do that. We know that there's a bunch of existing clients that aren't doing that. They're just downloading. And so what they're doing by just downloading is they're implicitly trusting the gateways to render correctly, to give them only the data that they asked for. So we've ended up in a system right now where a lot of clients
have implicitly put a lot of trust in protocol labs or the gateway that they're using to give them the right content address data. And that limits us quite a bit in taking this
current request flow and being able to make it less trusted or switch where that content's

coming from. This is in particular interesting as we consider

that. So Saturn, as I mentioned, is a couple thousand nodes, but they're people who are
just running a piece of software. So we don't trust them fully. If I go to a specific Saturn
node, how do I know that that Saturn node isn't going to give me malware back or isn't going to give me some random bytes back? I don't necessarily trust a particular Saturn node to the same extent that I'm going to trust protocol labs. And so we've got some real issues in being willing to say, okay, either we're going to give IPFS.io as a DNS

and TLS certificate to these random nodes that now can potentially ruin the reputation of the domain and serve malware on it, cause it to get added to blacklists. Or even if

we were to do something like redirecting the HDP with 302 or some other way to just the IP of the specific or some other less trusted domain name, you've still got the question

of, okay, if I'm just doing a one-on-one browser download from some untrusted node, we've got
this implicit trust that you're getting the right content in the current request paths, and we're not sure we can fulfill that with what Saturn's got right now.

So we've taken a couple paths towards how we're going to deal with that. The first is

that we're building out our trustless gateway specification. And so this is how do you get

content address data from an HTTP gateway when you don't necessarily trust the gateway?

And the general answer for that question is you ask for car files. You get the blocks
back. We don't do the rendering anymore. And as long as you put in your accept header, that you're either going, you either want to accept IPLD.raw, which is going to give
you the direct SID block, so the specific bytes that hash directly to that SID that you asked for, or you can ask for car, which will then give you the car archive of the
block of the SID and then the DAG below it, so the blocks that it points to. And so you'll
get the specific content address blocks that you can verify they're the right blocks that you asked for, and then if you want to render it, that's now on the client.
But this is a different set of requests than what we have today. And that's worth remembering.

So we can build out this new pipeline and we can start promoting this type of request
of, hey, you should ask for car files and verify them yourself. Because we can serve that with something like Saturn, and so we can do that more cheaply and more quickly for you. But that's not the clients today. That's the clients that are going to be willing to incorporate a client library or otherwise be willing to handle getting car files and doing something with them. So for that, there's a couple things. There's some of these longer-term efforts that Adin and that Alex are going to show in the rest

of this morning about what do client-side and sort of smarter client-side libraries
look like. So that we can start to move more traffic there. But we've also got some knobs
on our side, right? I think if you've used IPFS.io in the past, you know some requests

are not super fast. Or you get 429s of, hey, you've used too much. We can tune those. We
can make it so that your experience on these implicitly trusted things isn't great, such

that you're motivated, if you really want your users to be using it, to do the one that's faster and cheaper. And so some combination of these knobs, having a good path that we
like along with various pressure to make people move over, seems like our best bet at shifting
traffic to something better. So this trustless gateway spec, you'll hear

more about it. I think that that's going to, you know, there's already some IPIP work.
That's likely a thing that we're going to be trying to standardize, get community involvement on, get to something that everyone's happy with in terms of what is this way to be getting cars. Okay. You get the requests. Right now, those
requests are served with Kubo with a big block cache. That's pretty expensive. What can we do there? This is a secondary piece primarily around Filecoin retrievals and cost.

So if we want to be able to get Filecoin content with that challenge that that content should

be sort of a first class citizen equivalent to current IPFS nodes, we've got a few options.

One is we enable bitswap on Filecoin. And now it's all just bitswap. Great. It's all equivalent. We could extend our bitswap library to be able to speak not just bitswap, but

also GraphSync that Filecoin speaks or some other protocol. And so in particular, this
is that there's bitswap, the protocol, and then there's bitswap, the piece of software implementation that contains not only the protocol, but also a pretty significant scheduler about which nodes should I ask for and when should I ask for blocks from which nodes.

And that scheduler is the thing that we need to figure out. Are we doing a refactor? Are we diving into that? Or we're going to write another scheduler above bitswap that's able

to do a higher level scheduling between Filecoin protocols and bitswap protocols. Right? Because

these are sort of like the three places that you could come in and say, where are we doing this extension? We're either going in or we've got some place that we're going to have to have extensibility. The current approach has been to use Lassie. Hannah talked about this

two days ago. Which is going to be that higher level scheduler. And in some sense, you look
at that and you're like, okay, so you're adding another layer of complexity. Is this really going to make our lives better? I think that is a hypothesis that we're hoping that we
introduce something that's a little bit simpler as that layer. And then we can iterate on bitswap, but not in the critical path. Where is Lassie finding out about peers, then? Are

we doing that through bitswap still? We're currently using IPNI, the network indexers,

as a way to offload that peer discovery part. It reduces the need for all of these thousand standard nodes to be querying the DHT all the time or to stay connected on bitswap all
the time to as many peers. There was a description of sort of what this looks like from Lassie

yesterday in the content routing track that goes deeper. But the basic structure here
is that when you hit IPNI with a query, who has a given SID, it then scatters it out to a few things. It's got a local database, store the index, that is stuff that's been published
into IPNI. It has an accelerated DHT client called cascade DHT that already has a full

DHT routing table and so is able to do a one hop query of someone who has that part of

the DHT space to ask them. So it's able to do that reasonably efficiently for a DHT query.

And then it has a piece of software that's able to do that.

ELLERThat we built over the last month, thanks to mossy also called cassette that stays connected to a set of bit swap peers and and sort of cascades the query to them. But now that happens from only, I think we've sort of got one cassette Primary peer ID, so all of these thousand nodes appear as one query stream that's able to batch. So you get a bit swap one half with, you know, that second worth of all of the SIDS that various peers want.

And that's then the way that it cascades back when content isn't available, either through IPNI or to the DHT. The sort of high level of what Lassie is doing is it gets in requests for a DAG. It's got a subsystem called the Candidate Finder that is going to IPNI to find peers that are potentially interesting. That comes back as a stream of found peers. And then as these candidates come in within the scope of that session, as soon as it gets one for BitSwap, it starts a BitSwap session to begin trying to get those blocks on BitSwap. If it sees them over GraphSync, it'll start up a GraphSync query to try and get them. There's a little bit of session logic there of should we be duplicating? Should we, you know, okay, I started on GraphSync, but it hasn't really started. Maybe we should also spin up BitSwap. And that's likely to evolve. This is that level above the BitSwap scheduler. And so that's likely going to, as we add complexity there, we'll be able to potentially then remove it from the BitSwap layer. So that's the basic structure of what's going on in these nodes. The nice thing here, and this is really just reiterating a lot of what Hannah was saying in introducing Lassie two days ago, is that we're doing less sort of stable state ongoing traffic than what previously has been happening on Kubo in this Nginx Kubo deployments. We don't have a 200 gig block store that we're maintaining and paying for storage on. And we aren't sort of needing to stay connected to all of the BitSwap peers for performance, is the hope longer term, that these are just doing retrievals and are able to be a little bit lighter weight, essentially. So we can look at sort of what that looks like in terms of caching and performance to see where we are right now. Just to provide sort of a basic diagram of comparison of what these request flows look like. I think this is sort of a useful thing. The top line is what we have today, right? You as a client, you hit the Nginx Kubo in an Equinix data center, which then goes back to the origin potentially, or hits it, or is able to serve from cache locally. The middle line is how this changes for the current request flow. So the client is going to hit a lightweight Bifrost proxy, which is going to validate responses from Saturn and render them in the same way that you've got them today. Saturn is going to fetch it and return the cars back to that Bifrost proxy, and then get it from the origin when it needs to. The bottom one is for new verifying clients that'll be able to go to Saturn directly. So they'll directly get this car file from Saturn. And so you'll be able to skip having to go through these PL-operated centralized Bifrost proxies. In terms of caching, when we look at the system today, that Nginx and Kubo nodes are able to cache something
on the order of 70% of traffic, is terminating directly at the data center and not going back to origin nodes, right? So that's sort of like, when we look at our request distribution, a majority of traffic right now looks sort of like you go to the CDN and the CDN serves it out of cache. We expect in the lighter weight proxy, we may keep some load balancing cache. It'll be relatively small. We can hope for maybe 30% cache rate of the very most popular requests, but we're not gonna have large amounts of storage like a Kubo block store. So we don't expect that we're gonna be able to reconstruct or want to reconstruct all 70% of traffic there. We're not gonna have terabytes and terabytes of cache storage anymore. That's pretty expensive. However, what we hope is that the Saturn L1 caches, which are relatively beefy caches in this decentralized CDN are going to be able to be a caches. And so we're gonna be moving that where it's the Saturn nodes that act as that cache, either in the untrusted or trusted cases. Right now we're seeing on the order of 60% caches on them.
So it's a little bit less in terms of how we're doing it. We'll see if that goes up over time to be equivalent to what we've got right now with Nginx and Kubo. The storage is enough that it should be. And so it's a matter of, are we doing the right things to match that current caching? Here's just a couple of graphs over the last week of what we're seeing on caching. And so this is splitting apart that 70% that I was showing you before, because that is two systems. There's an Nginx load balancing cache that's getting maybe 30% ish. And so that's why we expect to continue to have around 30% ish. That top level Nginx thing is a relatively small HTTP cache and for a four cache and that sort of thing. And then you've got the Kubo block store cache, which is the bulk of it. And so this is where those two numbers are coming from.
So you've got 30%, yes. And then of the remaining 70%, no, you've got about a 60%, yes. And so if you multiply those out, you get a 70% overall cache. When we look at where Saturn nodes are from the Bifrost endpoints, this is from our New York data center. Just sort of, okay, where, if we were to ping all of these different Saturn nodes, where are they? How far away are they? The relevant thing here is the bottom ones are under 50 milliseconds. We've got a set of nodes that are pretty close. And so that's the hope is that we're not adding much more in that two phase unverified traffic requests
than the closest Saturn nodes. We want most of the traffic from these Bifrost gateways to go to a CDN endpoint that's near them, right? And this is supporting the current traffic stream. When you are your own node, you're of course going to go to the closest Saturn node. And so you're going to get that low latency, hopefully. But if you're going to IPFS.io and getting a rendered file where we can't offload you directly to Saturn, then we're going to have to go to Saturn and get the car, render it into the file. And so we want to know how much additional latency that's going to add. Right now for the performance stuff, we've got a pretty smooth request distribution across the Saturn nodes. And one of the things that we're working on tuning is seeing how aggressively we can have requests go just to the closest CDN nodes. There's a tension here between if we send all of our traffic to the one node that's closest to us, we now potentially have that node fall over and need to make sure we hot swap very quickly. But also there's some questions around fairness, making sure that we're properly incentivizing Saturn. So that has been a conversation that you'll hear more of from the Saturn team about this tension of a customer clearly wants the lowest latency, whereas you also want to incentivize the growth of your network in addition to optimizing the current customer. So what are we building? I've given you these block diagrams. I'll finish by making sure that I attach names that you can talk to and try and talk to about the people who are doing these various things so you know who to ask questions about. You first hit this proxy gateway on the current request flow called Bifrost gateway. Lytle is going to talk more and provide a deep dive after lunch about that component and what that looks like. A bunch of that code has been pulled out of Kubo and now lives in Boxo. So Boxo slash gateway is a package that does all of the HTTP semantics of how are we gonna render the final file. This Bifrost gateway combines that code with a library called Caboose that Arsh is gonna talk about. And Caboose is a front-end client library for Saturn that's gonna choose which Saturn node gets content pulled from. So it's basically maintaining a state of nearby nodes and routing requests with failover for which Saturn and CDN endpoint should get them. On the Saturn L1 node, which is Filecoin Saturn slash L1 node as a code repository, that is integrating Lassie in order to then pull contents from origins. Lassie is using IPNI, which has these components of Cascade DHT and Cassette as part of the lookup of who has content. Here's a couple more sort of deeper diagrams of that. The first one of these you'll find on the Bifrost gateway readme that is the set of requests and semantics that that proxy server expects to handle. And the second one is then Lassie and the sort of the general request flow of how Lassie is expecting to get content. So where are we right now? We currently have on the order of 30% of our production traffic being mirrored through this new system to validate that we're able to handle it at scale. The sort of current piece that we're working on is rolling out these Verifiable Car Requests to Saturn and validating that we're always getting back the right responses and that we're asking for the right things. So this is a change in Bifrost gateway and in Boxo slash gateway. Previously, when attached to Kubo, you had a block store. And so you'd ask for blocks as you needed them. And as you got specific blocks, you didn't have Kubo within spin up BitSwap to ask for those blocks. And so it was an iterative process. Now we need to have something more declarative where you figure out what is the shape of the data that I'm likely to need to serve this whole request and ask for a single car of that. So we're tuning that to make sure we're asking for the right things from Saturn. We're currently fetching the car that we need into a local block store for the scope of that request and then able to serve it. What we want to be able to do, and so this is the work that's going on in Bifrost gateway, is to also understand sort of the walk and the shape enough that if we get part of the response, we're able to make another car request for what we are lacking, rather than falling back to just the blocks that we don't have. So right now we've got enough to know the shape of the overall thing that we need, but if we get part of the response, and then are we able to subtract that and figure out a single next request that we need to make as opposed to all of the individual blocks? And that's where it gets a little tricky. We're probably not, I think, expecting to get that fully solved of always being able to ask for one more thing of the full remaining weirdly shaped remainder, but we should be able to do better than we are now. In Caboose, Caboose is currently, so this is which Saturn node do we talk to? Right now we've got a stable hash ring, so different parts of the SID space are being routed to different Saturn nodes that are close by. We're currently using a dynamic set of nodes that we've heard about recently that we think are potentially near us, and we change their weights of how much of the requests they're getting based on how well they're performing. What we'd like to do is reduce that churn, because right now that means that there's some fraction that are sort of trial nodes. So we hear about someone we don't know if they're good, we add them with a small slice of requests to see if they're good, and then weight them down or weight them up based on how well they perform. But that means some of our production requests are going to a node that's potentially pretty far away. We'd like to get rid of that so that all of our requests go to close by known good nodes, and then we maybe mirror some of the traffic to some trial nodes to see if we should include them. The work within LASI, so this origin fetch component, currently we've been identifying places where we're overfetching. So if we get asked for blocks, are we going to prefetch additional blocks expecting that those will also be asked, and other cases where we've ended up having more load back to origins than we were expecting to make sure we're doing the right fetches that we intend to be. I think we're, at this point, largely happy with the amount of traffic that we're generating from LASI. And so the next piece of work that that component is working on is including an HTTP transport alongside BitSwap and GraphSync so that we can fetch from a set of HTTP trustless gateway providers. There have been a couple origins that have said that it's much cheaper for them to serve an HTTP gateway than a BitSwap origin. And so we want to support that as well using the same trustless gateway spec. If you want to see more about where things are and how they progress over the next month, there is a public notion page that is extremely detailed
with all of the specific line items that are happening each week. You're welcome to follow that with whatever level of detail you wish. It, at its most, is probably too much information for any of us, but it does contain a really detailed progress in public of how this effort is going. So that's what I've got to say about Rhea. Thank you.

