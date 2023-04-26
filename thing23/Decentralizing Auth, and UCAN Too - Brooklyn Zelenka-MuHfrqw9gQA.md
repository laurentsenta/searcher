
# Decentralizing Auth, and UCAN Too - Brooklyn Zelenka

<https://youtube.com/watch?v=MuHfrqw9gQA>

![image for Decentralizing Auth, and UCAN Too - Brooklyn Zelenka](./Decentralizing Auth, and UCAN Too - Brooklyn Zelenka-MuHfrqw9gQA.jpg)

## Content

and a lot of other things. So thanks, everyone, for coming to the first of several YouCan

talks. I think we're just going to call this now the YouCan track for the rest of the day, basically. Because I'm up, then Irakli is giving a tutorial, and then after that, talking
about IPVM, which is actually built on top of a lot of this work. So YouCan decentralized
auth. In the YouCan community, we really enjoy the pun, you can do this, you can do that.

To kick things off, Mark Miller was, well, or slash is, yeah, the guy who really came

up with the modern conception of capabilities. So YouCan's a capability-based system. And
goes way further than what most people think of auth, right, which is just login. To the
point that you can structure your entire application around it, right? So we'll touch on that a little bit in this talk. But just to kind of, you know, help free your mind a little bit from auth is login. My name is Brooklyn Zelenka, you can find me anywhere on the internet
as xpeed. I'm the co-founder and CTO of a company called Fission. You can find us on
Discord, on the Protocol Labs network, Mastodon, or all over the internet. And I might know

a thing or two about YouCan. I helped start the project. I still do a lot of work with
the governance and standards in it, though it's now grown well beyond Fission and is
a fully community project. And because programming languages and distributed systems are my jam,

capability systems are right in the middle of that. So it's like the exact sort of thing that I like to do. So let's get started. So why do we want to do auth in this way and

everywhere? It used to be that you had a beige box under a desk somewhere and somebody would
log in and then they would log out and then the next person would come and they'd log in and they'd log out. And they wanted to make sure that, you know, person A didn't have access to person B's files, right? And that's really how access control lists were born in the 70s. These days we have a lot of stuff, right? With different consistency

levels, with different distances to the user. We have commons networks, right? We've got

stuff way at the edge. We have computers in our pockets. It's a completely different world
and there's so many applications now that need to be able to interoperate and connect and that today you can do it, but it's way more painful than it has to be. So how do

we actually get things to talk to each other in an easier, simpler, consistent way? When
we started at Fission, we wanted to always get to compute, which we're now just starting to do, which is great. But to do compute, you need to do data and to do data, you need to do auth, at least if you want to do data in production, right? Having everything be totally public and accessible all the time isn't really a production use case. It's good for some things, not good for a lot of things. So with Ucan, we're solving this bottom auth
layer. So I think in IPFS generally, we're trying to power a new internet. And to do

that, we have to think about how we're going to grow all of these technologies to impact
as many people as possible. And people often get really worried about this idea of crossing the chasm, right? Of taking it from early adopters to the early majority who have very different needs and philosophies about things. But we're not even close to that one. We are,

everyone in this room, are this tiny little sliver called innovators. We are crossing
that tiny little chasm there right now. And if we want this to grow, we need to focus

on these people, the early adopters, the people that are just going to hack something into a system and get it running and get it moving, right? We want to bring all the nice security properties of capabilities and decentralization, right? But we need to put it in a format and

in a way that's interoperable with how things currently are to kind of help drag people

kicking and screaming into the future, right? I think it's relatively uncontroversial to

say that Web3 UX is too hard, just like in general, right? So in order to meet this goal,

we need to be easier, as secure, and more open than OAuth, X.509, SAML, Macroons, Metamask,

WALConnect, et cetera. Basically all the other options, right? Now, I won't claim that we've

surpassed all of these in every possible dimension, but we're actually making pretty good headway against a lot of them. So OAuth is the big one, right? That everyone interacts with when

you sign in with Twitter, sign in with GitHub, all those things, you're using OAuth. This
is pretty typical OAuth login flow. You have a user, the application that's asking for

access to something, to some resource, an authorization server that actually controls the access to things, and the resource server. The first phase is the leftmost three coordinating

on can the application actually request the thing, and then these last few steps are actually

getting the resource. And you have to have this authorization server in the middle, because everything, all of the knowledge and all the information about who's allowed to access what, is centralized in that OAuth server. Which then begs the question, what if we got rid of that? And we made it, instead of a stateful protocol, into a stateless protocol.

It simplifies the picture quite a lot. It's really easy to reason about. We don't need

an OAuth server anymore, because the user is their own OAuth server. The application
asks the user, can I have access to this thing? Can I have OAuth? Yep, here you go. And then
it can make requests to the resource server. One way of thinking of the approach that Ucann

takes in general is that we're trying to be a Trojan horse for Web 3 into Web 2. So building
on widely supported, familiar, well-understood standards and tools, things that are not cool,
right, like JWTs, gets us in the door to bring these ideas to where people are actually using

things. So one example is putting in a bearer header, a bearer token header. Ucann is not

a bearer token, right? It is absolutely an authorization token. But by formatting it
this way, it automatically works out of the box with every web server out there and every
front-end framework out there. They just all work together. Newsphere is doing this. We've
been doing this at Fission for a long time. It works really well.

And another approach to things like this, right, because we try to be self-sovereign,

and Ucann gives you a lot of flexibility and self-sovereignty. One of the things that we're
trying to fix in there is when you have to run your own infrastructure completely, so you have to run your own IPFS nodes to do IPNS, for example. So this is great. You can
exchange messages with all the other peers in the network. You own this particular node.

You can run it on your desktop. You own your own key. But the problem is, what if you're
on a device that doesn't have an IPFS node in it, or that's not currently accessible
because it's asleep? Well, then you need to have somebody else run one of these things for you. So now there's users of a particular node who have to log in with some separate
mechanism, username, password, something like this, to get access to the namespace that

that IPNS node is managing. So what we've done by creating decentralization is actually
created a point of centralization, ironically. The same thing goes for edge and decentralized

apps. If you needed to keep a list of who's allowed to access things or who's allowed

to do updates, you've created another point of centralization. The nice thing about this

is that they're actually the same picture. They're the same situation. So if we can solve for one, we can actually solve for the other. So enough preamble. Let's actually talk about
what a capability is. Broadly speaking, there's two models. There's access control lists,
which most people are more familiar with. And as I mentioned off the top, this is really from the day of people will log in and log out, right? For sharing a terminal one at
a time. It's since been extended to more things. Capabilities were designed from day one for

logging, for concurrent use cases, for many. ACLs are fundamentally stateful. So here's

my user. Here's some service that they want to access. And when they make the request,

they get stopped by some guarding process. So this is a little bit like having a bouncer
at the club who checks their list to see if you're one of the cool people and says, okay, yeah, cool, yeah, you're on the list, and I'll forward that on to the service. The advantage
of this is that it comes in three really clear stages, right? You can throw an auth server in front of basically anything, right? It's totally decoupled. The downside is that these two are not in control. So if you either take control of this one or the service, you have

access to absolutely everything. It doesn't provide the user or the service with agency.

And we have all of the data centralized in this one list. So if another service wants
to reuse this, they have to either synchronize in the back this list continuously or make

roundtrip requests always to the same server. Some of you might be thinking, well, that's

really simple, right? These days we have things like CRDTs. The bad news is that CRDTs don't

express ACLs very well. So not to pick on this one post in particular, but this is to
say there be dragons. A lot of very, very, very smart people have tried to do a CRDT

ACL. Hasn't worked. There's a bunch of reasons, but just, you know, I mean, this is a half

hour slot. I could give an entire talk about this. But just as an intuition pump, if you

have, for example, two admins of a system, so they can accept and reject people into a group and they have the power to reject each other. If you have a malicious one, it will immediately try to eject the other admin. The good admin will try to eject the malicious
one. And nobody in the system now knows who to trust because you have an eventually consistent CRDT, right? People have tried to tackle this problem at the user experience level and expose

all of the, you know, the gory details of that to the user. It doesn't really work out.

Capabilities don't have this problem because we always root our authority for a resource in the resource itself. It always owns from the get go. It can delegate out to who it

wants to be able to access it. Capabilities have a slightly different picture. Here's
the user, here's the service they want to access, and the user has some address, some
pointer to the thing, and a cryptographic token that says they're allowed to go and

access this. So instead of being like a bouncer in front of the club, it's like having a ticket to the movie theater. When I go to the movies, they don't check my ID on the way in. They check I have a ticket and that's it. If I can't make it, that's fine. I can give my ticket to Steven. Steven can go see the movie instead of me. This puts the user and the
service in control. All of the required info sits inside of the token. It's stateless.

I can make copies of the capability and hand them out to other people. I can attenuate
them and I can share them with others so they can also access this.

You can do things with this that are actually more powerful than with ACLs. There's a really
great paper called ACLs Don't that explains that you can model anything in an ACL as capabilities

but not everything is from capabilities as ACLs. It's a more powerful concept. The composition

of capabilities is interesting. The classic example of this is I have a capability that
gives me access to soup in a can. I have another one that gives me access to a can opener.
If I just have the can, I'm going to go hungry. If I have just the can opener, I'm going to go hungry. If I bring them together, I can get into the soup, I can have a nice lunch. To put this in more technical terms, if you have access to a database and access to send

email, you can now compose, read from database, operate on it, send email. But if you want
to make the read or the write side of that something else, you can compose those together in different ways. Instead of being a database, maybe it's going to read out of Filecoin. Instead of email, maybe it's going to post to IPNS. Now, that's a lot of detail. That's

a lot of things that are unfamiliar. But auth, fundamentally, should be just extremely boring.

So this is something we've had in production for a while. It looks exactly like an OAuth

screen where you say, ah, I want to give them access to these things. And then you get access to, in this case, it's basically a Dropbox clone. I want to give them access to my public
files, my private files, for this amount of time. Yeah, go for it.

So let's actually look at one of the tokens in detail. One of the things that people ask

me all the time about UCAN, like, literally including during breakfast this morning, was how is it different from DIDs? And DIDs are actually inside of UCAN. It's not an or. It's

a building block that we build UCANs out of. DIDs say who you are. They give you a public
key that you can prove that this agent, this person, this process was allowed to sign.

And UCANs say what you can do. So the distinction is a did is, let's make sure I don't get these

backwards, is authentication. It proves that some data is authentic. It comes from a trusted source who you know. And UCANs are authorization. They say that you're authorized to do some

action. So here's an example token with the signature
and header taken out. We have an issuer, which is a DID. In this case, it did key. And an

audience. So in this case, I'm transferring authority for the things in the cap section, which we'll look at in a second, from, say, from me to Steven.

We have some time bounds on this to say this can't be used before these times and it expires
at this time. So that you only give access to things in the, if somebody were to, say,

get a hold of your private key, it's not a disaster now that they have access to all this stuff. So you follow the principle of least authority whenever possible. You can make them as long lived as you like, but scoping them down to just the resources that you need for a particular request. And then the actual capabilities themselves.

There can be several in here. We'll look at these in more detail in a moment.
So I mentioned a moment ago signature and header. It's a JWT by default. So you also
have a JWT header. In this case, it says the kind of algorithm you use, and that's JWT.
You have the payload and the signature. The signature must match the did in the issuer,

which is then signed over as part of the payload. So you have this guarantee that this was signed

by who they claimed the issuer was. And we have proofs that say I claim that I

have access to these capabilities because somebody delegated them to me. Here are those delegations. Here's those other UCANs that I had access to.
The actual capabilities. Again, you can have several in any given UCAN. They always have

a resource. So it's any URI. It's the noun. So in this case, it's a HTTP path to Alice's

photos. Then you have some verb, in this case, read access. And then you can add optional

additional fields such as in this second one, sending email only internal to phish and dot

codes email addresses. So you can send as Boris, the CEO, only internally.

Because we have dids, we have all the resources and tools available for PKI. So we don't have

to send keys around anymore. Moving keys from one machine to another is super dangerous.

Browsers now, all the major browsers, have non-extractable keys. So you can sign things without ever seeing the private key. Obviously, you see the public key. And without them being
able to be stolen by, say, a malicious browser extension. So I have my keys on these different browsers. And I can use them to sign tokens. And then

doing this delegation and pointing between them from issuer to audience without moving keys. So we say this is delegating authority without delegating keys.

I've mentioned attenuation a few times. The basic idea here is when I'm delegating to
somebody else, I can give them access to the capabilities that I have or fewer. So I can't,

out of thin air, say, oh, yeah, totally I have access to that database. I would have
some proof that I have access to it to give to them. So here's Alice. Alice has a bunch of capabilities represented by these various icons. Here's
Bob. Bob has a desktop. And she's going to delegate absolutely everything to him. Maybe
he helps her out with stuff. He also has a phone that maybe he trusts less. So he doesn't

need to delegate absolutely everything to his phone. So it's not just about the individual person. It's any agent. It's from phones to laptops to servers. You can move straight
through those. Bob from his phone delegates to Carol here, who only gets the one capability at the end.

And these all are proven in this chain with those proofs, which are just CIDs for the

previous you can. Now, it doesn't have to be a linear chain like this. We can branch at any point and give a different subset of capabilities to somebody else. They can further

sub delegate that. And you can also take from multiple sources, and they don't have to be rooted in the same user like they are here, but for multiple sources, and bring them together to say, well, I can actually compose together rainbow and dog. Everybody's favorite capabilities.

The other advantage is this doesn't care where it lives anymore. We're not waiting and looking at is it in this database, is it in that database, do I have to talk to this one, do I have to talk to that one? It's just everything's in the token itself. So it doesn't care which
application it's powering. It can go from...

a messaging app to doing analysis to spreadsheets to just sitting on somebody's

cli doing individual tasks. It doesn't care, it can move straight through all of

those. So it lets you compose applications as well. Revocation. So let's say that Bob discovers that Carol's laptop was stolen

and doesn't want to have all those, you know, this capability in use in the wild
by some malicious person. He can issue a revocation by CID and as long as he's in

the proof chain he can revoke anything below that. So it doesn't have to be the very next one, it's anywhere in there and that will revoke everything after that.
These are gossips. It is eventually consistent. So it's not that when you
issue it, it's immediately done. You can send it to where they'll need to terminate that invocation of the UCAN. So if it's, say, access to, you know,

some specific server, say it's for Web3 storage, you could send directly to Web3 storage, hey I'm revoking this and then I'll gossip it to everybody else as well. But because all this is designed to also work offline, we can't guarantee a
strong consistency model. You can add more consistency as you need, but it's designed to be very flexible. It's also designed so that, you know, we have these

nouns and verbs, that those can be composed separately as well. URIs, there
are an infinite number of them but there's only a certain number that are registered, right? You could put literally whatever string you want in there as long as it's a valid URI. We think most people will only need a handful, right? So
by having those and then a standard library of capabilities, it makes it
really easy to not have to sit there and think like, oh how am I gonna model this? You just say I need one of these and one of those, I'm gonna stick them together and send it off. And now we all understand the same vocabulary. It's always extensible, you're always allowed to add more, but to get people started at
least this will cover sort of the 95% use case. I mentioned a few times now,
this is all done with JWTs and those aren't like the hippest, coolest structure. Good news, the good folks at DAG House have solved this. There's a
UCAN IPLD and CACAL from Ceramic that puts this into IPLD schemas. You still

have to be able to turn them into a JWT if you want to interoperate with anybody who's adopted the standard, but if you're say communicating between your own servers you can absolutely do this. The case study from before, the IPNS version,

so we had this point of centralization because the key only lived in that one place, but now with UCAN we can transfer authority without having to move keys around. So we can completely break this up. I don't even need to have an IP
an IPFS node around anymore to update records. I can issue a UCAN as long as I

can prove that I have control over that namespace and I can gossip it through the network. And as long as those point back to this one, it all works. I can
delegate access between people so that it doesn't even have to be literally this machine or even me as an individual making updates. I can share that with other people and they can make updates as needed. And this is something that
we're actually exploring at Fission for a sort of broad system called NNS or the
the name name system as some of you may have seen the talk yesterday. And DAG
House also has a proposal for something between this and today's IPNS to get

closer to this without having to throw out all of IPNS in the meantime. Basically having a proxy server that you can use UCANs and then it'll do the
publishing on behalf of people. We can also do things like authorized data
retrieval. So it's great to have encryption at rest, we do a lot of that at Fission, but if you want to prevent somebody from even asking for something over the network because it's always better to do something off the network, you can also model that with UCAN and ask for that even I think now it's

actually even implemented in BitSwap. You can put that in a BitSwap header. If any

of you saw Philip's talk day before yesterday I think, this is also baked in
deeply into the web native file system. So all of the write access in WinFS is
managed with this and it powers a bunch of apps today including the one that I
showed earlier, the sort of Dropbox clone on IPFS called Fission Drive. We are

building all kinds of stuff out of this, so authorized channels. So if you want to
have a group chat or set up gossip between collaborating peers that are

working on some shared resource like a database or you know a group chat, you

can use this to say hey look we all have a shared capability, we're gonna join a group together and use that to prove that they should be allowed into the into the secure channel. We had built this out of originally the double

ratchet from Signal, but good news messaging layer security from the IETF just got approved and so we can plug this in directly into the authorization

portion of MLS and it just works. We need to go back and update the spec because this was literally last week I think that they finally got that thumbs up. But the actual flow otherwise in that spec basically looks the same. And if you

can't get enough of UCAN today, come back in the afternoon and we'll talk about how we're using this to power decentralized compute as well, including
interesting things like doing distributed pipelines and collaborating processes dynamically. So some final resources to leave you with. If you're

interested or want to get involved, there's the UCAN community. We run monthly community calls, we have a discord, it's an open process, anyone

can get involved. We have a bunch of implementations, they're not all in the
same version unfortunately. The main implementation these days are Rust and the TypeScript ones. We also have some early work with Passkeys, which is the

new Apple, Google, Microsoft key syncing system. Works in browsers, etc. You can

basically authenticate with your fingerprint so we can sign UCANs with those. Metamask as well, we've done some work with that. And if you want to add to this list of implementations, let us know and we'll even get you a cute sticker mascot. If you want to go deeper into capabilities, Capabilities Myths
Demolished is a great paper. ACLs don't. The eWrites website, so Mark Miller's
website, does lots of writing about this for quite a while. It's a fantastic resource and if you're interested in sort of more of the, you know, where have

people tried to do this before in the late 90s, the Simple Public Key Infrastructure or Spooky group did a lot of this. They didn't have all the the

same pieces that we have today, so it wasn't as successful. They had a hard time, for example, with revocation. That we now have the tools for literally the computer science and the networks to do. Later today, Irakli will be giving a

workshop on getting started with UCAN. If you want to do something from home, there's UCAN.lol, which was put together by Brian, which is a plain text

getting started with UCAN game. And we have UCAN.xyz, which is a token

explorer. You can paste a token in, it'll break it apart for you and show you here's who's making the claim, here's what

capabilities it allows, and you can even explore the proofs. So the whole proof
tree. You can go and click through dynamically and explore that. So thank
you. And that's the QR code for the community group and I have stickers, so

does Boris. Come find us if you want any of those. Thanks. To create one of these, you need to know the public key did for whoever you're

sending it to. What is the system that gets carried through? How do you discover
the public keys? So by default, it only supports didkey and didpkh,

same basic thing, which are the public keys. You can also do, as long as your
collaborators understand other did methods, you can have other ones that are more semantic. So you can give, you know, based on a username, things like that, right, in the did method. But yeah, you absolutely need to know who you're pointing it at. So this is one of the things that we hope to solve in the NameName system. So you could give somebody's email and it would look up for you because underneath it's all UCAN. So you'd be able to find, hey, here's their claimed did. You can also use things like didDNS. So if somebody says

I'm, you know, alitatexample.com, it can go and look up from the DNS record,

there's a text record in there that gives you the did key, for example. Okay, one more question. So I presume the proofs need to be public so that other
people can travel them. Where do those get saved? So they actually don't have to be public. You can put it entirely over private channels as well. So one way of

managing that is you can pack them into a car file and ship them around. And then once they're, like, let's say that you're just collaborating between a small number of parties. Once they have them, they have them cached, you don't have to send them again, right. We have this, well, have this running over IPFS. So they all

have CIDs, right. You can pass them around as car files, you can put them in HTTP headers. There's, in the UCann working group, there's a whole spec on how to do this purely in HTTP headers without any decentralized tech at all. Gotcha, thanks.

Sorry I ask a question at the end of everything, but I am a huge capability
fan. I mean, one of the things we support at the foundation is the Sprightly
Institute that's doing OCAPN. And so with kind of, kind of inside baseball thing,

I'm interested in how you might think of OCAPN, which is a sort of, I guess, an

RPC kind of protocol that uses capabilities and how these might fit
together, because I know that's going through a kind of pre-standards process. And I guess related to that, you talked a little bit about eventual
consistency and revocation. Definitely something in sort of Mark Miller's, sort

of, the historical view of this is you have this sort of proxy, right, where you
go, I can ask for this permission but it's going through something that
can kind of mediate in real time whether this should be rejected. And I was

wondering if that is, is that something that fits into the UCann space or is UCann more static than that? Yeah, both fantastic questions. So on

OCAPN, we're part of this OCAPN standardization process. So this is all OCAPN compatible or it will be when that's a finalized spec, right. So this
will all work together. Something Mark has been very vocal about in that process is that it shouldn't, there shouldn't be an OCAPN token. There should be a bunch of them that are just different representations of the same ideas, right. And so you can, and say ZCAP for example, there's some differences but

the overlap is quite large and so we can interoperate between those as long as both clients understand both, right. The second question was about revocation. So

you're absolutely right. This actually isn't full-blown, full-blown OCAPs. You can bootstrap up into them but this is SPKI or spooky. So there's, I

think actually in Capabilities Myths Demolished, I think, they have a comparison chart that says, you know, how far along this road are you and spooky is

the step before. So the reason that we need that is everything, at least for our

use case, has to work offline and be highly cacheable. You can put it into a
mode, like you could use them in a way where you don't have revocation and you have this proxy system which is awesome, but in sort of classic OCAPs you

have fail-stop. So if that proxy goes down, even if there's a network partition, you can't do anything and you always have to make these round-trip network requests. One way of looking at UCAN, this is getting a little bit in the weeds, is you can think of it almost like a set of instructions, like an AST, that you're going to execute when somebody reads the token. To say this is the intention as long as there isn't revocation. So it's been interesting talking to people like Alan Karp and some others about the differences and we're kind of exploring this sort of less-discovered space now that we have some of these newer tools versus the last time with the original spooky implementations. Firstly, fantastic slides. Could you talk more through the revocation

process and how that would work? So if I, for example, delegate authority to some
capabilities to you and you in turn delegate it to someone malicious, how would I, so yeah I'm the first link in that tree and I want to revoke access to the ones that are marked there, how does that propagate through the entire tree? Yeah, fantastic question. So there's a specific format where you say here's my

DID, here's the token that I'm revoking, and here's a signature over that. You

can't unrevoke something, so it's a one-way operation, right? And as soon as
somebody receives that, up until the point that the token is valid for us
because we have that time expiration, they cache the CID of it and if they see it in any proof chain it's not considered a valid proof, right? It doesn't have to,

it's not that you're revoking the first thing that you delegated, it might be something further down in the chain and so you can target anything that you're in the proof chain for and say yeah that one down there, like I like this person, I like this person, but not them. They can't have this anymore. So I have a question
which I know many people have been wondering over the course of this conference, which is who designs all of your wonderful stickers? That is Bruno.

We had him on sort of like a sticker retainer basically for a while and we

eventually just hired him. He's fantastic. Yeah, awesome. Thanks.

