
# Iroh - IPFS reimagined - dignifiedquire

<https://youtube.com/watch?v=9jR9EWZ2bO8>

![image for Iroh - IPFS reimagined - dignifiedquire](/thing23/9jR9EWZ2bO8.jpg)

## Content

Cool, thank you folks. So, back here again. So this, I'm kind of in the right track I

hope. So this is mostly about stuff we did in IRO. We had, as you might have heard this

morning, in IRO we made a lot of interesting decisions earlier this year and the end of last year. And so this talk is kind of like summarizing a little bit of that. We got a lot of questions like, why did you do that? Why are you doing that? What actually are you doing? So I'm going to try to cover that and see if I can answer some questions. And

I'm planning to have some time for questions afterwards, so if you have more questions, hopefully we can answer them too. For those who don't know me, I work on this company
number zero and this is kind of like also is related to where does IRO come from. So

one and a half years ago roughly, I was like, can we have better IPFS? Can we do things?

Some people might know I like Rust, so I was like, I'm going to do this in Rust at least. And it was kind of where it started, but it went quickly farther than that. And so we
had actually an implementation that was somewhat compatible with the existing IPFS network

by the end of last year at IPFS camp, where I got to launch the first version on stage,

which was kind of fun. But when we came back from that, we sat down and had a hard look

of like, okay, we have a prototype, we have a thing that kind of works, it kind of interrupts, but it wasn't anywhere close in terms of actual performance and actually the goals that we
had set ourselves for actually this new implementation. And we're trying to understand where does
this come from? What is the problem? And that led to an interesting experiment that is called

SendMe. You can find this repo still. I'll talk a little bit about that later this year,

and then that actually becomes, so what SendMe was become what is now the IRO. And IRO got,

we deprecated the original implementation of IRO. Not even a year old and already getting
deprecated. That's a very short life cycle there. Almost like Google.

That's called now Beetle, and this is like somewhat interoperable implementation in Rust,

but not what we're focusing on now. But if you have some needs, feel free to still send us PRs and talk to us about it. And we shipped the first version of the renamed thing at
the end of February 23. And our first success story is actually the death chat integration
that I mentioned earlier this morning, which is actually we have an implementation that now works across mobile platforms and desktops actually on local networks to send content

address data fast in the way that we actually wanted to. And that was really important for us in that development process was like, okay, if we're doing this from scratch, we still need to make sure actually that very early on, we
can't wait six months to see if this works for anybody. We need to put this into actual
hands, into actual other developers' hands and be like, does this work for you? Can you actually build something useful for you users with this? All right. So I mentioned a couple of hard, some hard problems that we had when we came

back from IPFS camp last year. And so I'm going to talk a little bit about those. So

one of them, which you might've heard me talk about, I talk about BitSwap and we've also

mentioned, talked a lot about the move, the bytes working group that was kind of spawned from this was like data transfer was like not where we needed to be. Right. We had an implementation. I eventually wrote an implementation of GoBitSwap, which is like a straight up port of the Go implementation and Rust, which was very painful. And actually it still exists.

It's, but it's not that great for many reasons. And we tried to figure out like, how can we,

how can we make these things better? And just like, we just didn't get, like, there was
no like happy path where we got. And so we started moving the bytes working group. Thank
everybody who was participating still. This is a great ongoing effort to like figure out, Hey, what other ways can we move data around? Why is, why do we have problems with BitSwap?

Maybe BitSwap is not the problem. Maybe the implementation is, we don't, I think it's a mix. But that was one of the hard, really hard things. So we like looked at like, okay,

what data transfer protocol can we actually employ? The other piece, which kind of also
is related to BitSwap, but also to DHTs and it's all your, all your favorite topic is
content discovery. And like, just very quickly, this is very important for anybody who's like

new to the space. Like content discovery is the actual hard part I claim in these systems,

like downloading data, like HTTP does that. Cool. Like, like if I know somebody has the data, like we can find a way, like, even though we talk a lot and move the bytes, but like, how do we officially move the bytes? This is not the really hard part. The really hard part in most cases, especially in a system like where I profess where you're like, you want a global view into the world. And I be like, I have this thing that's roughly 32

bytes. Who has the answer to this? That's a really hard question. And so content discovery

is basically how do you answer this question? Where are these bytes on the internet or rather in the world, in the world? Because they don't necessarily have to be in the internet, but if I want to retrieve them over the internet protocol, they have to be. Looking at the

existing solution, there was like, this is not, this is not where it needs to be. We like, we need to rely, like we had like the one big issue is like reliability, right?
It's like something that's older than 24 hours on the IPFS network. And that is not on the
gateways kind of hard to get. Oftentimes high latency. Again, if it's not, if I can't go

to the indexers, like usually it was like, although the gateways, like it can take seconds,

right? To even just know where to download things. And I haven't done anything yet, except the record where it is. The existing DHT sometimes worked really well and sometimes just didn't

for oftentimes like unknown reasons. ProLab time team, for example, spending a lot of time, like figuring this out. There's a lot of research in like improvements to DHT. How can we make this better? But another thing that we also realized here is like, there
might be solutions to this, but pretty much all of the really good solutions mean breaking
the DHT. So like basically have to have fork in the network anyway. And then the third

one, which is the one that was like probably the most painful and hard to see kind of was

too many layers. So we, we ran into this issue. It was like, we just don't know where the

problem is, right? Like you have the system and even you like implement it as you look at it, it's like, it doesn't work, but nobody's like, even if I instrument, it's like there's

pain, but it's basically in the whole body and you don't know where it is. And so it
was really, really hard for us to like understand which piece should we improve? Which do we need to actually work on to get the problems solved and moved out? And so it's really searching
the needle in the haystack. And yeah, I'll talk about how we actually went about solving

this in a moment. But like this was like really, really hard, especially with a lot of abstractions

that are in IPFS. Juan has this famous slide where it's like, there's like all the components

of the P2P and multi-formats and IPLD, and they're all nicely stacked together. And it's a beautiful picture. And I'm like, but if you try to find a problem, the problem is that in reality, these all have arms and they're all, these arms are all intertwined. And now,

if you try to know, like find the problem, you now have to go through this labyrinth
and it gets really, really hard to know which of these abstractions might be wrong, which of these abstractions might be exactly right, but like they both look wrong because like together they just don't work. So that was really hard to see and understand and kind

of accept because like as a programmer, eventually if you had like two classes in CS or so, you

learn like, oh, abstractions are good. Right. And eventually they're not. And there's a

reason people have been built monoliths. It goes really quickly into the same discussion that people are having, microservices versus monoliths. And I'm not going to say like either one is right, but either one going to an extreme is going to be wrong and painful. Right. If
you have on your machine, 2000 different microservices running and you need Kubernetes to like manage

them, that's probably not where you want to go. But like you also probably don't want
your Slack implementation to be part of the operating system. Right. That's also probably not the direction you want to go. But finding this balance is really hard. And there's like,
the most things there's no very easy solution, unfortunately. Speaking of solutions. So got

to think very hard. Right. So Feynman had this great method. So we wrote down a problem
and we have to think very hard. At least we have to pretend that we think, thought very hard. And this, this quote captured like one of the themes that I eventually ended up going

to, which is perfection is achieved not when there's nothing more to add, but when there's
nothing left to take away. And, and this came very much from the, like, there's too many

layers, there's too many things. I don't know what to do. Right. It's like, so I like the approach eventually became like, okay, what did we start with zero and only add things

that we've checked that are fine. And that we actually need. And this is the other piece
through this is like, okay, so we remove everything and try to remove as much as we can, but,

but also have a very strict rule of what, what we add back. Right. When you, anybody
has built the software and like had it interact with any other user, there's going to be a request, like, can you maybe add this feature, which is like printing emojis, but upside
down in reverse when I press the button C. Right. And you're like, sure I can. Should

I though? Probably not in this case. But it seems like a fun program. Kubo has this problem

for many years, right? Kubo have to solve everything for everybody. Right. There's one implementation that everybody was using. And so it had to jump through all the hoops and like, yes, you want it. And also Kubo had the, the, the challenge of like when it, when
it was being developed, nobody knew what was IPFS was right in the first years. And so you're, you're, you're both like trying to tell people what the thing is. They're telling you how it needs to change. And you're like in this, in this dance. Right. And, and it's
very, very easy and actually predictable that you very quickly end up in this place. We
have a thing that kind of does everything for everybody, but also nothing for nobody
because it, it, you can't do everything at once. So we're trying to push to very much

like, okay, one user is not like one user request basically is not enough. It has to be like a really good reason. And especially pushing to a level where why is it like always

asking why is this not application land? I think some, some systems do this really well
and they're like, they, they push everything into application land. Application land is big, right? And developers of their own applications, right? They will know best what to do with
their metadata, how to deduplicate data. Like we can't take all the work from applications
developer and like, it would be nice, but we can't. And so, but the less we have to
do and the more abstract of a platform we actually can provide, others can go and be

like, you know what? I like this thing. And another one can be like, oh, I like this thing.
And like, they can both interact on the same platform and we don't have to make decisions of like, no, but your data format is much nicer than yours. So we're going to use yours and the other one is going to be upset. And like, they can talk to each other and it's like very sad. The third one here is everybody, everybody counts. I have this nice picture

of like all the wrapping and frames that we have on like a regular UDP packet. I was like,

this is more just to highlight back in the day when it was not that much RAM and not

that much processing power around, people did a lot of many hard tricks to store. They
literally started to try to save bits, right? They would like encode bits and two of like, oh, I have one zero padding here left. I can just encode my bit in here. Cool. People are
kind of have stopped doing this in most implementation places. It's, it's very time consuming. Like,

I'm not suggesting everybody starts doing this, but if you're building fundamental software that you expect to be used by millions of people on millions of devices, you should
start thinking like, okay, if I add 10 bytes here, that means every device in this room

sends those 10 bytes. Maybe once a second, if we just have very low, like low communications,

this adds up very, very quickly. So if you're an application land, it's oftentimes fine. Yeah, just use Jason, just send it over. It's fine. Compression will take care of it. But
if you're implementing protocols that are going, actually, we have to worry about the packet size. And I was talking with Martin earlier today, like, can we get bigger packets

on UDP? Probably not. We probably suck for a while with like 12 to 1500 bytes of packets,

right? And like, okay, so this is a very limited number. So if I add 50 bytes of framing, that's

a high cost. And somebody is going to pay that. Not to say that sometimes you don't
need that. It's just be aware when you add it. And just like, don't add 50 bytes and
10 bytes and 10 bytes and 10 bytes. And then a length prefix. So you can manage the overhead

of the header. The other piece is there is every byte that we don't send makes our things

faster. And every byte we don't have to decode makes our thing faster too, right? If you
want to make something fast, what's the easiest way to do that? Avoid the work, right? So

all right. So eventually you have to write down some answers. Some of the answers that

we ended up with and looking at are kind of those five, well, they ended up being five.

One is special. Kind of goalposts for us where we're like, okay, we have these attempts of

like principles, how we can solve this. But what are we like, where are we like sticking
our hand, like posting a sign and like, we have to do this. And like, we will measure

ourselves against this afterwards. Right? So the first one is kind of this, and this is why we're here, right? It's like, we love IPFS. We love the idea and we love the original promise. Like I pulled this from the way back machine. This was very nostalgic when I did that. This is how the IPFS website, this is not the first version of the IPFS website, but it's the first like one that you want to look at. Really fun. Go, go, go, go check

it out if you, if you want to. And so it's like, we want to focus on, we're not going

to, there's some stuff in there, which is like cool, but we're probably not going to focus on. And as you've heard this morning, like the vision of like, what is an IPFS system
has been involving like for the last, what, nine years, eight years, how old is this?
But we really want to focus on some pieces here. Like it needs to be global. It needs to work global and needs to be peer to peer and needs. And one of the things that here really stood out for us is like, it needs to have a simple interface, right? It can,
it needs to be very easily explained and said to people like how to use this, go and not,

you have to read three books before you can start using it. So the other four pieces I talked about earlier, a little bit this morning is like, so reliability,

right? I can't have a program and be like, go to somebody and be like, you should totally

use this program instead of this working HTTP transport. Right? Like, right. This is, this

was like one of the like claims, okay, can we, can, can we replace HTTP? We're like, it's a different story. But like, if I go to somebody and like web two, quote unquote, until I'm like, here, this cool technology, it verifies your data. I'm like, cool. Okay.

I'll try it out. And like, works sometimes that, that I can't write. Like, I mean, I'll

be embarrassed. They'll be embarrassed. We'll both feel bad and like sad story. So let's,
let's not do that. The other piece is like, as I mentioned, it's like configuration and like understanding, you can't expect people to like understand how your software works. You have to give them software that works with some instructions, but you can't have to like, you cannot be like, Oh, I need to teach you eight different concepts of like,
I don't know, object inheritance before you can use my software. No, it's your job as
a developer to understand object inheritance, to implement the program, but it's not your
customer's job to like, understand that you need to give them a simple interface that they can actually just go and use. And for us, it's also like, it would be really nice

if there's a certain delight to using software. I think software engineering is a certain
craft and it's like, that's just, it's kind of a cherry on top, but it makes a big difference
for a lot of people, especially when you download a lot of software today and you're like, this is not very delightful to use. It needs to be fast. I said like, look, if I go to somebody

and like, Hey, you can use this instead of curl. And like, it takes like 10 minutes to download five megabytes. It's like, Hmm, they're going to be like, I go back and I'm going to use curl. So the thing that we have to measure against ourselves is HTTP web servers.
Like this is like, this is the, if it's slower than that, we're going to get laughed out
of the room in most conversations. Um, because we need to have a conversation where we're
like, Hey, we, we're this fast, but also we add this big benefit, which is content addressing.

Cool. Your data is verified. You actually get the data and like, you know, it's the
right data and not just some arbitrary other data, right? This like, we need to have additional
features, not less. We can't be like, Oh, you trade off performance, but we give you
some content or something like, I'm going to make, no, I need my data today. My user is going to kill me if I don't do that. Efficiency. Um, so efficiency originally this was like

kind of lumped together. but I pulled it apart specifically because I think, um, it gets all left out a lot because you think you're like, I have the most efficient algorithm to compute pie. Cool. But what if I told you, you don't need to compute pie. You can just look up the first hundred digits in this lexicon, right? There's, there's no need to do that work. Right. And so, um, oftentimes we'll have sometimes where we get like very performance obsessed. It was like, it's going to optimize what is there.

And I was like, no, like let's took a step back and let's say, see if we can a avoid the work and be most environments are very resource constrained. Even though we, we, we tend to forget is like, okay, our Mac books are very fast. Cool. My iPhone is super fast. Cool. This big ass machine that runs my home server, which is totally overkill has like 64 cores. Like cool. Okay. Right. Like what resource constraints? Nah. Um, and then you go and like look at your phone and you try to

use an app and like an implement an app on a phone, like even on a really modern phone. It's like, oh, after two minutes I get killed by the operating system. So. Okay. I've like,
I've probably better make use of those two minutes very well. And oh, if my CPU spikes too hard, the operating system is also upset with me because I'm draining too much battery. So I guess I don't know about betray compute course here. Um, and so there's, there's, there's a many pieces
on the efficiency side, but it's really, you need to look at binary size, CPU usage, RAM usage,

network usage. Yeah. Mobile networks are not fast. Um, sometimes they're about there. There's the five G right. Sure. But do you have like, how much do you have five G if you're not in a big city,

like for two minutes a day, if you're lucky and then you get to another wife and all your IP
addresses change. And that's a fun story. Um, and then the fourth one is Peter pier,

right? As I mentioned in the original piece of about IPFS, um, for some, some is like

content dressing is the core, um, for us, peer to peer is still still really important.
And, and this is not necessarily just for the reason that like, we think things should be
less centralized, but also because this is actually one that both makes our problem harder, but also gives us an advantage. It's, it's the one thing where we can actually be like, Hey, we might be able to be faster than they. Right. So yes, conceptually, it's very easy to just
download the thing, sorry, from the big server, but there is actually inefficiencies in this system.

And if we are clever enough, and this is like, I want to, like, I cannot emphasize this enough.
This makes the problems that we're trying to solve a magnitude harder than the centralized

version. It was like, but it is theoretically at least possible that we can be more efficient and actually more actually be faster. Right. Because it is faster to download this content

across the room than to pull it from Google servers, even if they're like three streets down, but it's really hard and the hardware and the way that the software, most of places is architect
today is not optimized for doing this. So we have to like fight a lot of obstacles and like, it is really hard, but if it works, it is kind of magical and really cool. All right. So I already, earlier I mentioned, then writing down the answer. Part of that was writing some code because, you know, I like writing code. So we made this

prototype called send me. And the idea was just like, okay, so what's the dumbest thing we can
do that kind of does this? And so it was like, okay, we need, we need to talk to two devices,
need to talk to each other, two nodes. Okay. So I'm going to use quick. All right. Quick kind of gives us, it gives you encryption, gives you cool streams, like nice. And like, there's libraries
that can just pull off the shelf and like use them. Cool. All right. So I have a network connection. Now quick is not very cool with like peer to peer in theory, because it uses CLS and TLS

like certificates and like certificates, like authorities. And we're like, ah, but luckily the lip PDP teams that solve that for us. So there's lip PDP TLS, which does generate the right certificates for you. And there's the library you can just use, which is really nice. Okay, cool. So we have, we can talk to each other. Okay. Like,
so there's this thing called there isn't right. So we need a hash function. What a hash function.
So there's this cool hash function that we like called Blake three, which allows you to do

verified streaming. So you have a gigabyte blob and you send it over and you don't have to trust download the whole glob. No, you start verifying it immediately as you go using a protocol called
bow. There's a nice talk from Rudiger earlier today, but all the fun things you can do with
that. So we kind of smushed those two together and be like, does this work? And it actually worked.

That was really fast. And like the quick implementation still have some way to go to where as fast as TCP, but it's getting there. Um, and there was nothing else we kind of knew.

We just sent the hash and it was like verified streaming. We just exchanged a gigabyte of data and it was like, cool. And then we're like, okay. And now, and now what? So we, we kind of spun on

this idea and try to like, okay, how far can we push this? And, and back to the thing, like how
many things can you remove? We were like, basically took this on the one hand side and then the other side, like all of like our existing, then existing IRA implementation, all the things we have to like
add for like full compatibility with Kubo. And so like compare the two and we're like, okay, like,
look like what's pull over what we absolutely need and see how far we can get. And that's basically

what the new IRA is. And some of the things I'm not going to talk about the things that we added, I'm going to talk about the things that we didn't add very explicitly. So the first one was there's
no data store. So if you've ever run Kubo, if you want to add your data, just gets sucked up and

it's gone. Well, it's still there, but the data that Kubo actually tracks by default, um, is gone.

And so the data actually ends up being chunked up and stored in a block store though slash data

store in Kubo that, um, there is a pattern called file store in Kubo, which does not do that,
which means it will just reference the original data. Um, but adding, so, but if you want to do

both of these things get very quickly, very complex, right? Because then you're in competition like, okay, maybe this data is managed by the user. Maybe I'm managing this youth data gets very nasty. So we originally, we were like, well, we obviously have to have a database. Like it was like thinking about writing our own database. And it's like, we can optimize this the hell out of this. Cool. But like, then we went to the Delta folks and they were like, dude,
you can't duplicate storage. Right? So the implement, the thing there was like, this is a backup service. So they make a backup and now we need to ingest that. And we're like, yeah, you'll just have double that. And they're like, you don't have the space for this on my SD
card. And so like, okay, I guess we need to rethink this. So we're like, okay, there's a

file store pattern. We can do this. But then when you think about adding complexity, it gets really

messy very quickly. So we're like, okay, what if we didn't ever store data ourselves? We already
have Blake three. So Blake three gives us the ability to avoid blocks. So the actual blocks

chunking, so we can just reference whole arbitrarily large files. And so we ended up

actually not using that at all. Right? So we just entirely store things outside.
And when the user gives us data, they give up as a path. And we're like, okay, this is the data. If you change it, you're on your own. But we'll verify when we serve the data, that it is the data

that you gave us. And you're saying we're serving. So we don't become a bad node, but everything

else is up to the user. One other thing we're leaving out gateways. So HTTP gateway is cool,

but there are other people who are much better than writing web servers than ours. And like
writing yet another web service seems to like not be the ideal thing. So our current thinking

is here, we, I was focused to be a library. We're going to make it easy for people to write
an API on top of that, if that's needed. And I can just, you know, you can just make Jason RPC APIs to this, and then you can call this from a browser. That's cool. And or from JavaScript, if you want to, like, we don't need to have actually provide a web server. You can just add the API and then use some tiny amounts of JavaScript to put things together if you want
in the browser. If you don't need this, like I wrote, like a lot of cases will be just fully embedded into the application. You don't need a web server either. Without the P2P, for now,

this one, this one was a tricky one because it was the hardest breaking change we currently have.

The biggest one, as I mentioned earlier, was the many layers. And the P2P focuses on
solving a really hard problem. It's like, it wants to give you a modular configurable network stack

for P2P systems. That's at least what it says on the website today. That is a really, really hard

problem. But we don't want configurability. We want one protocol, one thing that does it,

and that works out of the box. And if we change something, we're going to increase the version number and it's going to be entirely incompatible for a while anyway. So we don't want that, actually.
And so for us, it is right now just a lot easier and faster to iterate because we remove those
layers of configurability. Because every time you added the possibility to change something, somewhere there is a config option stored. And you have to make decisions based on that dynamically,
and it costs you. Without a file system, this one is also potentially kind of weird, right?

Has it IPFS file systems kind of in the name. Our current abstraction, there's no actual file

system abstraction. As people know, nobody likes UnixFS, I think. Everybody's like, UnixFS v2,

WinFS, IPVM. But we haven't figured it out. WinFS, there's implementations, but it's like,

this will change again. I'm pretty sure there will be WinFS v2 and other cool file system
abstractions. We don't think we actually need to make that decision as long as we provide enough
underlying building blocks for people to actually build a file system abstraction on top of this. But a lot of applications don't need a file system under the hood, right? Like, if you write a simple data application, you're just like, store my data, give me back my
data by key maybe. File systems are make a cool, like, they work for a lot of applications,
but they're not necessarily the right abstraction. And so we're saying, we're trying to go a little bit deeper, lower. And if you want to have a file system abstraction, hopefully we give you all the
right tools to actually build that on top, and we can iterate in application space. Very similar for IPLD in a sense. Data modeling is really, really hard. And the problems IPLD is

trying to solve are really, really hard. But there are also a lot of other people who have been
trying to solve these problems and have very strong opinions about these. And we're like, we're not going to make the right decision. It doesn't matter. Like, we can choose like, A or B, and it's going to be wrong for a lot of people. So we're trying to basically, again, remove ourselves as much as we can from that decision process. So you can put Cboard data on this, you can put JSON data on this. We don't care. We're not going to attempt to read that.

It's going to be like, we're going to make it, try to make it as easy for you to read that and do logic based on that in your application. And you can probably add IPLD libraries and you just run
them on top if you really like that. But we don't have to actually understand what that means. We can just hand that off. The same way your HTTP connection doesn't actually understand

what JSON is. That was a bad comparison. Anyway. So we focus on these couple minimal

building blocks, right? So we like hashes. Hashes are cool. We like bytes because usually that's
the thing that we all agree on. Like, what do we need to move around? Bytes. So we take hashes or

bytes and then we need this one additional piece, which we might get almost rid of, which is called

list of hashes. So you want to like have more than one hash. You just don't want to send them, but you want to kind of group them together. Currently call them collections. And they're
just a list of hashes. And you can say, give me all the, this collection until just download
like links, just like each piece, one after the other. Currently we're thinking we might be able
to actually get rid of this concept for the most part. Iris still needs to understand this idea, but we don't need to have a separate type. Technically that collection is also just bytes. And so maybe we can be very clever and simplify this even more, but that's still trying to figure

that out. All right. My time has ended two minutes ago. So the future is coming. Hopefully

a couple of things we're working on right now. Connectivity. So turns out right now we don't

have pole punching. Like, so things work with static IPs. That's great, but nobody has static IPs anymore. So hole punching and relays is getting close. So this is coming very soon.

Data transfer. Almost done. Very coming very soon. Resumability and range requests.
All right. So if you interrupt things of large transfers, we can actually resume in the middle.
And range requests. So we can say, give me the first hundred bytes and the last 200 bytes and the three bytes in the middle at offset 14. And the same for collections. So we will be able to

say, like, give me the first three items in this collection and the last two items and give me the five bytes of those last. Which makes for some really interesting things because with a single
round request, the application knows exactly what's going to be in those bytes. So you can
now start embedding bytes at the beginning of files, for example, again. And content discovery,
I mentioned, is a hard problem. A lot of people are thinking about content discovery. We've been doing some research. We have a rough idea of what we want to do, but still need to actually
implement that. All right. Sloppy. So the extension to... There's a nice paper about

distributed sloppy hash tables. Just kind of an extension of Cademblia. All right.

I don't know if I have time for questions, but yeah. That's my spiel. So you tore out IPLD, which I love that. But I think that Adine's lenses are kind of useful

for making sure that you don't have to... You know, you can, like, compute where the data is. Do you have any plans for standardization of that? Or are you going to leave it up to, like, some kind of very, very arbitrary computing layer? Is there any thoughts about this yet?
Mostly just leave it up to the application. So I think we're going to see a lot of different
experimentation with this, especially with, like, the various WASM approaches. Like, somebody very eloquently earlier this morning said, like, put WASM somewhere in IPFS,

tacked on. I don't know. I don't want to put a VM into IRO, but that doesn't mean there couldn't be,

like, a nice library, which is just WASM IRO. I don't know. Which just runs arbitrary WASM and

exposes hooks to, like, fetch data through IRO, right? But the goal is, like, especially with
the range requests, make that, like, give a lot of, like, very low-level, actually, primitives

to people to allow to experiment with these things, right? So that if your application uses
lenses, cool. Everybody who uses lenses over top of IRO, they can all collaborate. But not everybody
who uses IRO has to now use lenses to get any data out. 