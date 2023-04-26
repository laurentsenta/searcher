
# The Name Name Service - Blaine Cook

<https://youtube.com/watch?v=CHiCEd36KtI>

![image for The Name Name Service - Blaine Cook](/thing23/CHiCEd36KtI.jpg)

## Content

Hello! Yay! That's what happens when you put plants in the crowd. Hi, I'm Blaine. I'm going

to talk about NNS, which is the NameName system. It is not the NomNom system. This talk will

be maybe a little bit weird. It's sort of, this is experimental, as I think most things

here are, so it's a system that's kind of imaginary, so I've tried to make the slides

kind of indicate that, but I don't know, most things at the IPFS level are kind of imaginary,
so I think it fits. So, what is NNS? It is a way to securely map names to data. That's

it. So, I'm just going to start and talk a little bit about what NNS might be for, and

these are just some ideas. There's lots of other things that you could use it for. So
if you want to find Alice at example.com's key without knowing which did method she uses,
so if it hasn't been gossiped to you, you might use NNS to look up that key. You might

want to know how you can contact Alice using an encrypted channel, so Signal today uses

text messaging to negotiate this, but you could use NNS. Where does Alice share content,

like her Instagram photos or Masteron posts or whatnot? Where can you get information

at a URL on IPFS? So, we have IPFS, we've got lots of CIDs in the content cloud, but

if you want to know where someone's blog exists in the IPFS network, that's a little bit harder

to deduce and you have to use some tricky things to get there. NNS, I think, is a nice

solution to that. And we've been talking about IPVM, which we'll hear more about tomorrow,
I think, but where can I get a piece of Wasm code that has a name rather than a CID? So

if you want to be able to evolve that code and update it, NNS I think can help us do

package naming. So let's step back just a little bit and talk about what a name is.

So as far as I can tell, and feel free to correct me, a name in this context is a public,

publicly unique and verifiable name. So verifiable means that it's possible to prove control

of the name using a DID. So for example, DNS hosts and domains, email addresses, social

media handles, all that kind of thing are names. So like my Twitter handle, atblame,
is a name that I can prove control of. But also things like HTTP URLs, ISBNs, it's sort

of notional that you could prove that, but your publisher could publish a key somewhere and then give you the ability to prove that you published a book. Orchids, the academic

IDs, social security numbers, if the government provided a way to prove that you own that name, public keys, et cetera. So a DID is not a name. And I think this is an important

but subtle point. No one's ever going to say my name is didkeyzbe0538247 out loud. It's

never going to happen. It's not a name. It's not something that people use as names. But

I think DIDs are really important because they give us a way to prove that we can control a name. So all NNS names would have at least one valid DID. And so that's sort of a starting definition. So I'm going to go into a quick little demo. Let's see here. Can we see that?

I can't see that. So I hope you can see it. So this is just a little web interface. And

it's sort of a simplified version of constructing a YouCan. And there we go. I've got my mouse.

So we're going to take this data and we're going to construct a YouCan out of it and then we're going to send it into the NNS network. And yay. Oh, I can make it larger? Sure.

Uh oh. Oh, I am peerless. Uh oh. What has happened? Let's see here. Now I can't find

my VS code. Uh oh. It looks like VS code closed the window. So I'll open a window. And we

will take a look here. Live demos are always a mistake.

Okay. Let's try this again. There we go. Yay. Woohoo. Yay. So now I can go it's been stored

so I just have a little endpoint that I can go fetch the YouCan. And this YouCan is coming
from the NNS network based on you can see the name up there. I guess I'll make it bigger.
I can't make that bit bigger. Anyways, if you look up the bcook.ca, the name that I stored the YouCan against, it returns it. This is all very exciting, I know. I'm going

to make a little change here. And I'm going to try and save my YouCan with my private

key into NNS. It's really hard to use the mouse like this. So I'm going to store this

into NNS but now with Boris's name. And it says that it doesn't work because my key that

I have posted at bcook.ca, if we look there, I've got a key stored against a DNS record

that doesn't match on bman.ca so I can't store into Boris's name because I don't control

Boris's name. This is all very, very basic but I think an important property because
now we have a way to store names into essentially DHT that we can only store if we control.

So that's the basics. Let's see here. Go back to the slides.

So I'm going to go back into the theory a little bit about what NNS is. One way of thinking

about it is that it's a hack on Zuko's triangle. So essentially, if you go really hard on the

decentralized requirement of Zuko's triangle, then yeah, you have to pick two. But NNS,

because we can put any name into it, so any of those like the HTTP namespace, the email,

emails from social media, et cetera, we're pulling in a whole bunch of decentralized

and federated and even centralized names that we can prove and we're putting them into one system and so now we have human readable, secure, according to the name of the security

of the name system that you're rooting against. And then we can choose the level of centralization

or federation or decentralization that we want. And so I think it gets a lot of the properties that Zuko's triangle says that we can't get at, but sort of lets us do it

in a nice way. NNS is not a pet name system. And I want to talk a little bit about pet

names because a lot of the time in the decentralized world, people are like, oh, you can solve this with pet names. And I don't think you can solve this with pet names because pet

names don't cross connectivity boundaries, for one. So if we're riding in a bus and our

phones have died because we haven't had power in 12 hours and we've just had a really great conversation but we don't know how to contact each other with a QR code, we're kind of hooped.

And so we need a name that we can say that we can remember in ourselves that we can share with each other so that we can get in contact when we have connectivity again. And so I

think having that sort of human readability is really, really important.
The other thing is that pet names don't cross trust barriers. So if I share my pet name
database with you, you have no way of trusting whether or not those pet names match with your expectation of what those names are. So that's a huge problem, actually. So we

can't use pet names. Got the Brooke chuckle. So NNS is also an attempt to rein in standards

proliferation. So we have all of these different naming systems, but they're all sort of independent.
And now if you want to use them, you have to kind of understand and interact with all of them. NNS tries to encapsulate all of them so that we don't just have another standard.

It's something that can kind of work with all of the standards. I've excluded the PST

and the telephone network because there's no way to verify ownership of a phone number that I'm aware of and pet names because those are sort of local contexts.

It might be a replacement for IPNS and DNS link. I'm going to quote Brooklyn on this

one. She said it was okay. No one really likes IPNS. It's kind of a Baroque system. It's

got some issues. And the names in it obviously are keys, right? Like they're not meaningful

to people. And so you have to use DNS link with IPNS anyways. So you kind of have this
weird indirection thing going on. And I think we can achieve most of or all of what we have
with IPNS and DNS link with NNS. DNS link also has the disadvantage that the DNS infrastructure,

which is frankly amazing and like working on NNS, I've done a bunch of digging into DNS and it is amazing. But it does have some problems. So like latency and the fact that

you know, the security really only applies to the top level zone file and that kind of thing is really problematic. So I think NNS might help with those. One way that might

resonate is that NNS you can think of as just a decentralized key base. That's it. Like

if you get that, you're done. You can go to a different talk. But my slides are pretty. So you should probably stay. So it's a way, you know, if you can prove that you control
access to a name, if you can associate your name with a key, then it's NNS is a place where you can publish that name and other people can get it. That's it. So how does
it all sort of work under the covers? We start with dids. Dids, it's I think worth noting

dids didn't really exist when key base was invented. So it's sort of, oh, I forgot to
mention key base would be great. We would just use that except they were centralized and then sold to zoom and now they're like a proprietary chat software. So we had to
invent it here. So we're using dids. Dids are kind of complicated, but they're not really

complicated. I don't know if you can read that. I'm lying. They're really complicated,

but they're actually pretty simple. I think a lot of the cruft around dids is really complicated.
But if we, if we pair them back to what the core is, they're really simple. So we take
a did method, right? So for example, did DNS what that means is given a name like did DNS

example.com, it says go fetch the text record for underscore did dot example.com and you'll

get a public key and then you'll have a public key and you can use that public key to verify
that the owner of, you know, the person who was able to publish at underscore did dot example.com signs with their private key. That's it. If you take one thing from this

talk, that's what it did is. And basically nothing else. All the other stuff around did
documents and everything I think is too complicated. But we can, a lot of these are imaginary.
They haven't been defined, but we can define these did methods. And we've been talking about, you know, what if, what if it did method is wasm? There might be something interesting
there, but at the core, it's just get a public key for a name.

So the next part of, of sort of how we compose NNS is having a record. So this is like a

DNS zone file, but instead of using like a zone file, we're using a who can, sorry, a
you can maybe a who can. And all that is, is for a name, you take a, a you can that's

issued by a did that is valid for the name that you want to publish. So in our example,

my, my name, be cook.ca was valid for the you can that I published, but be man.ca was

and then we have facts. So we can say, all location or DNS link or, or, you know, any

number of different attributes can point to other stuff. So we're talking about doing
this for like account delegation for redirects, like all sorts of different, different things.
And then you just sign it with the key that's available for that did you know, this looks

a lot like a DNS record. We've got the resource record type. We've got the fully qualified domain name. We have the resource record value. That's a DNS record. And then the signature
over the you can is basically what DNS sec does, except it's a lot simpler to do this
than publish something into, into DNS sec. And then the, the, the third and final part

of, of NNS is a DHT which I know is kind of a bad word around these parts. But basically

we just make these named values available in a decentralized way. So there's more to

say on the DHT part in particular, but yeah. So I'm just going to recap. So we, in order

to make it all work, we generate a you can that delegates from a name to some stuff authenticated
by a did that implies control of a name. We upload that you can to the NNS, NNSD DHT with

the key of, of the, of the name. And that will only succeed if and only if the, the,

you can sign in key matches that you can issue her and the name matches the issuer. And then

you fetch them and that's basically it. So the, the initial version of NNS just has an
HTTP API that fronts a lib P2P DHT that is currently Kadimlia, but we will talk about

that. So this is all just review. I've said all of this, but you know, I've added, you

know, maybe, maybe this is a decentralized DNS. I don't want to be too ambitious, but
this is IPFS land. So who knows? Yeah. DNS is great, but it's also pretty old and doesn't

serve a lot of the tasks that we need. It's, it's worth talking about what NNS isn't. So

it's NNS itself is not a way to prove ownership of a name, right? That's what the dids are
for. NNS helps you discover those proofs, but it doesn't root those proofs. So when

you download a, you can from, from NNS. So you say, Hey, I want, you know, brook at vision

dot codes information, her profile. I can go to NNS and ask for it. And I might trust

the NNS network to provide the correct you can, but they might be lying to me. But the
nice thing about this is that I can go and check myself and verify this information.
I don't need to trust the, the DHT in order to in order to use it. It's not a new namespace.

This is a personal bug bear. There's a lot of systems like I could name names, but I'm not going to because I'm better than that. Where, where people go and create new namespaces

and basically say, okay, we've got a new, a new namespace. It's secure. It's got all
these great properties, pay us five bucks and we'll sell you a name. And I feel like
this is a it's quite a cynical capture. And often, you know, from a regulatory standpoint,

it's never, it's never going to be different than I can ultimately, because I can has sort
of fought through all of the regulatory uphill battles and come up with a regulatory structure.

That's, that's pretty good. That's not to say that you don't want your own namespace and NNS, you could run internally to your organization, have your own namespace independent from my cannon. Great. Away you go. But but I don't think the solution to this problem

is to create new namespaces. So, so NNS is not trying to do that. This one I'm less sure

of. I think it's probably one of the bigger questions with the NNS work which again is
very experimental at this point. It's not guaranteed to be globally consistent. We could build this as like a rollout blockchain kind of thing and have global consistency and whatnot.
But I think there's enough diversity in the namespaces and how people want to use this that it's better to basically use it as like just a unstructured DHT. You can just find

stuff in this, in this bucket and let the individual mechanism sort of describe how
consistency should happen rather than having one consistency model for all of the names.

So if this stuff works, we're talking about like trillions of keys.

so consistency is harder at that scale. Whereas if you can scope down what Consistency means for your use case, then it's a much better sort of scenario. And it's not a blockchain.

It probably depends on some blockchains like certificate transparency and that sort of thing, but it's not it's not itself kind of one one central blockchain. So why, why am I working on this?

Fission kind of needs this, but also we don't it's kind of, you know, DNS link mostly works. But I think I think there's a lot of opportunity in exploring what what other things we can enable.

So, you know, email, HTTP URLs, social handles are widely used really important in the way that people use the internet. There's lots of ways to like link DNS names to decentralize stuff on the internet.

There are very few or none that let you link email, HTTP and social handles in a decentralized way. Like I said earlier, key base was a great start, but it wasn't decentralized.

DIDS are great to go from the like the DIDURI to a key. But most often we don't have that DID in the first place, we need to get the DID to get going.

So this this hopefully helps bootstrap that process. Pet names I mentioned. And one of the really big ones here is enabling permissionless usage.

So if Google hasn't implemented some standard to enable like my email address to exist on the IPFS network or blue sky or something like that, I can't I can't participate.

But I'm not going to move off of Google and move my email address of 18 years or whatever, just because some new decentralized system says, well, you have to if you want to use us.

So the nice thing here is that NS allows us to take control of names that we don't own, that we don't own the infrastructure.

So with email, they're still playing with some of these these ideas, but there's we have a working sort of test case using DKIM and DMARC to basically sign a proof that says that I own an email address without any involvement from the service provider.

So basically, the DKIM and DMARC keys let you send an email that in the email you say I'm delegating to this key and then all of that signed and then you can put that into the you can as the proof.

There's a research paper pretty recent called Open Pubkey that does a similar thing with OpenID Connect, which is wild and super cool.

And I'm really excited to try that, but we haven't gotten there yet. But I think there are ways that we can actually bootstrap out of some of this corporate infrastructure.

And then, yeah, so so that's basically it. There are a lot of questions that I have still.

So there's a bunch of technical challenges, latency and locality, obviously.
I think NNS, if it's going to be successful, needs to have lowered latency than DNS for a much bigger key space.

I think that's possible, but we're going to have to do some thinking on the on the DHT side.

And it's also important for authorities to be able to opt into hosting parts of the namespace.
So obviously, we can do pinning, but that's expensive and complicated.
And realistically, a lot of this stuff is going to look like hosting a DNS server.
So you're going to have an organization that you're happy to pay to just sit there and say, like, I'm answering requests for names like Fission.

We'll definitely run one of these and we're definitely going to pay for it.
It would be great if we could tell the DHT, hey, we're here. We'll just answer your question.
Other people might post names that we don't host ourselves, but we'll be available and it'd be nice to be able to support that natively.

And then there is I kind of have punted on this consistency question, but I think we do need good answers for this sort of sub sections of names and what consistency looks like so that you can know that you're getting a recent version of someone's information and whatnot.

So those are sort of the some of the open questions on the DHT side of things.
I think there are some pretty good recent research into hierarchical DHTs. And because the naming system is sort of by definition hierarchical, we can leverage some of that to get better locality.

So that's sort of some of the coming work. But if you know anything about that, please let me know.

And that's all I have. If you have any questions, let me know. And this is a link to the talk page on the Fission site that will be migrating to a GitHub repo at some point soon.

Thank you. This is awesome. This is really cool.

It's great to see this push when dids first were being built, which is also after where when IPNS it important historical note had IPNS happened after dids, it would look a lot more like this.

And it was kind of after trying to struggle with some of those problems that dids ended up going that direction. I think it's great.

I think we should think about how do we get it into an implementable structure so that it can kind of percolate through implementations.

Like, what does it look like for things like Kubo and others to learn how to resolve names through this?

And I think on the on the consistency piece, one clarifying question. At first, I thought you were sort of deferring to the did method to decide where to look.

But then it sounded like you're aggregating the proofs elsewhere in the in like what you're sort of describing as potentially DHT.

And I guess the question is, like, is that is that the case? Yeah, yeah. Yeah, because otherwise you can't like I can't publish a in order to publish a proof for my email address for my Ramada at gmail.com email address.

Google's not going to host it. So I need somewhere else to put it. Yeah. Yeah, that makes sense. You could come up with some way of mapping the the method to a canonical way of doing it, because some methods, some did methods will, like you were describing, mean vastly different numbers of key of names to resolve.

Yeah. Or vastly different frequencies at which these might change or different security requirements.

Like, is it really important? Is this like a bank? And is it really important that this is exactly one possible address?

Or is it kind of like the latest state of a chat? And is it OK if it's like a little bit delayed or whatever?

Yeah. And so I think maybe resolving that question in the name system itself would be good, because I think if you enforce the same consistency semantics across all methods, that'll get it'll sort of like it won't won't do well enough at the really important things to be to be adopted.

Totally. That makes sense. Yeah, this is really cool. Thanks.

Any? Robin first. Oh, no. OK.

So you know that terror you have when you turn up like three minutes into the start of like a talk and then you have a question, but you're really scared that it was the first thing you address.

So I'm not stupid. I'm just tardy. But my question is, is what's the thing that you look up? Right. What's the key to the key value?

And are you going to put any constraints on that? Or are you thinking like what do you think that should look like?

Where you turn to slide one. Yeah. Yeah. God. Any public, globally unique and verifiable name.

OK, but like so what why do I look up? What do I you're sort of saying is it does it look like Blaine Cook or does it look like so so Elephant Man or what?

Right. So like personal names wouldn't be named like wouldn't be names in NNS because, you know, even even though I've got an unusual name, there are other Blaine Cook's in the world and there's no way to disambiguate between them.

So there's no way that I can prove that I am Blaine Cook because other people can prove that my email address or my Twitter handle are global unique and only I can prove that I control them.

So so that would be the criteria. I think. Does that make sense? Do you think that there is going to be a challenge here about like defining like, for instance, your Twitter handle is at Blaine, right?

But do we end up going into a thing where we have to say, so what we really mean in this lookup is that Blaine at Twitter because Twitter dot com. And do you think we do you think you have to sort of paint some picture about what the internal structure of those names should look like?

Yeah, I think that's actually really good. I think URIs work really well for that. So schemes, URI schemes. I can't think of an example where we don't have a URI scheme that sort of does that.

And in most cases, you're going to have a context. So like if I let's imagine a sign in page that just like literally the only question is, what's your name or what's your handle? And you can either put in a discord handle or a Twitter handle or an email address or a URL or something like that.

In virtually all cases, there's actually not very many globally unique name schemes. And so you're going to be able to infer what that name is. So like you can say at Twitter, like at Blaine means it's a Twitter name, you know, that kind of thing. Or you can have like a little drop down to say which scheme is this.

Got it. Yeah. Yeah, you'll need that.

You mentioned Keybase a couple of times. Are you also aware of Keyoxide? I think the project is and if so, did you look at it? How does it compare?

Yeah, yeah. Yeah. So I did a bunch of research because I really didn't want to build this.

All of the existing sort of Keybase comparable ones, either so Keyoxide doesn't have a lookup mechanism. So there's no way to like, or it's a centralized lookup mechanism. Like it's I think at the Keybase site, if you go to Keybase.net or whatever, you can do a lookup. But there's no story around what that looks like decentralized.

So it has the same fundamental problem as Keybase did where if it if Keyoxide goes away, if it stops being maintained, then all of those names disappear. Same thing with like PGP key stores. And then there's I'm blanking on the name, but there's a decentralized DNS, GNU DNS, GNU NS, which does a lot of this stuff.

But it's only for DNS names, like they reinvented DNS, but then they didn't extend it to anything else besides DNS. So it was just an extension of that. So there was kind of nothing that did all of the things. But yeah.

And I should add, if you know of one that does do all the things, please let me know because then I can stop working on this.

I'm not getting too much into the implementation details, but why DHT? Like, I don't seem to be that big of a, like an address space, like there shouldn't be that many key values in the whole space of things. Like, why not just fully replicate everything across all nodes?

So one of the use cases I alluded to this was, I mean, first of all, like if you've got every email address on earth in here, it's potentially 5 billion names right there.

Or many more than that, I guess. But one of the things I alluded to was this HTTP redirection sort of scenario. So there was an issue that came up in the Mastodon network where when you post a link to a video, all of the Mastodon servers that see that feed go and fetch the oembed data for that video from the server.

And so there's sort of this situation where you have thousands or tens of thousands of servers descending on some unsuspecting server. And it was causing some problems for people.

You know, this is a thing that CDN solved very nicely, but a lot of the people out there don't have a CDN and aren't going to CDN enable their site because the Mastodon network kind of operates inefficiently.

With this, they could just publish a link up into NNS of their HTTP URL, like of the link of the address of that video of the oembed endpoint.

Sign it with the did DNS, so really, really simple, not anything as complicated as an HTTP signed exchange. And then all of the Mastodon clients could just go check in NNS and then go and get the content from IPFS and know that they're getting the correct content, that no one's interfered with the content.

So when you start doing things like that, we're talking like every HTTP URL could be in this. So that's not something that we're going to be able to replicate.

And I think we're done. Cool. Awesome.

Oh, one more question. Sorry, one further clarifying question. So like Danny, I was, I had assumed that the naming was derived from the DID so that the way that you solve the uniqueness of the address of the namespace was by leaning on the DID namespace.

But then the answer they gave to Danny made me think that that's not the case. And I'm thinking of some other.

Yeah, that's true. So this is one of the problems with DIDs because a DID is a verification method, but it's not the name. Right? It's like, there's at least four DID methods that I can use to prove ownership of a domain name.

But in most cases, when you're using it, you only care that someone has proven that they control the domain name. You don't care how. And so the and most often people aren't going to know how you like which DID method you chose in advance, but they will know the domain name.

True, but the reason DIDs have those is that it's trying to self-describe the system by which you control the namespace. So DIDs have this like self-describing name system in them that let the method describe the sub namespace that you have to think about and look up through.

And if you kind of remove that, then you have to solve it the same way again. And you sort of like solve it the same way again, as you were giving the answer of like, oh, you have to say at Twitter and something else, which is kind of reinventing the DID method.

And so you might find a way of like, if you don't like kind of like how the DID naming scheme works, you might find a one to one mapping. Right.

But I would lean away from reinventing that piece because that's one of the strongest things that DIDs have to offer. And it's a very hard problem in like, you will encounter lots of namespaces that are like that will clash very strongly in the namespace that they have.

Like you can't look at just the the name string and resolve it without self-description in some way.
Yeah, for sure. Yeah, that's yeah. We should we should talk more about that. Awesome.

Thank you. 