
# WNFS: Versioned and Encrypted Data on IPFS - Philipp Krüger

<https://youtube.com/watch?v=LBMyRp4Ywew>

![image for WNFS: Versioned and Encrypted Data on IPFS - Philipp Krüger](/thing23/LBMyRp4Ywew.jpg)

## Content

Hey everyone, I'm going to give a talk about the Web Native File System, which is a versioned

and encrypted file system on IPFS. So, first of all, hey everyone, I'm Philipp, you can

find me as Matthias23 on various kind of internet networks, and I work at Fission. And what

we've been doing at Fission for the past couple of years is we've put encrypted data onto
IPFS from browsers. And so, the way I like to think about this, and personally how I

like to explain it, is kind of like a data backpack is what we try to build. So, this
is Winnie, this is our mascot for WinIFS. It's slightly altered, this image, it's not
the original author's intent, so to say, but Dali helped me out, put a backpack on her.

And basically, she wants to have her data in her backpack and take it to different devices.

So all of her devices now have her data, similar to like Dropbox or something. But she also wants to bring this data to different apps that she uses. So there may be like a music
player like Diffuse, some drive app that is similar to Google Drive or whatever, or something
like FX Photos, a photo manager, something like that. And that all should work like online and offline, so it should be a local first kind of experience where you can be offline for an extended period of time. And I think of this as like a data backpack because it's

really about user agency. And it is about user agency, and I think of that as a backpack

because you can take it anywhere, you control who you share stuff with from your backpack.
And at the end of the day, for us, this means you have control over the keys that are used to encrypt the backpack or give write access to different parts of it. But this turns out

to be kind of difficult to build, especially because of this. The browser is quite a hostile

environment, right? You have malicious extensions, you have cross-site scripting attacks, code
injection, supply chain problems. And yeah, so at the end of the day, every time we tell
people that we're doing this from the browser, they're like, what? You're putting keys into the browser? What are you doing? But yeah, we do do that. The WebCrypt API, for one,

is amazing, and thanks, Dietrich, for the work on ED25519. I'm excited for that. And

non-extractable key pairs are what makes all of this possible, as good as possible, essentially.

What that means, though, is that, well, you need to have an asymmetric key pair for every
device because non-extractability basically means you can't take the private key and actually
read the bytes. All you have is some kind of JavaScript reference to something that allows you to sign or to encrypt. But it also gives you revocability. So no malicious code,

cross-site scripting, or extension can take your private key and steal it forever. You'll
always be able to revoke things that were signed or encrypt things differently when
they were once encrypted or decrypted with your key, essentially. But yeah, with decryption,

this doesn't work that well. Once you decrypt something once, of course, the data needs
to be readable inside the context of your browser. So what do you do there? Well, we
were thinking the best you can do is probably using the principle of least authority. I mean, sandboxes are another kind of way of trying to solve this issue. But that's what we were going for. So essentially, in this context, what I mean by this principle of least authority is you want really, really fine-grained access control on all of your data. When I go to an image editor app, what I want to have is basically I just give the

image editor only the image that I want to edit and no read or write access to anything else. So that's what we mean by it. Or if you have some kind of compute job that you
want to run somewhere else, well, you give them your file system, but you give them just a snapshot of what the computation needs to know about from your file system. Another

thing with browsers is persistence. So your browser may just decide to throw away your
data, which is kind of not ideal. So that means that key recovery becomes a problem,

because what if your browser actually holds your keys like we have them in WebCrypto? So you need to think about key recovery. This talk is not about that, but I'm mentioning it for completeness sake. But it also means that encrypted data needs to be persisted
somewhere else. That's why we put it on IPFS. We want to have, let's say, a pinning service

or a storage provider or someone should be able to persist your data. But it also means

this is another point of where it's becoming very difficult, because now it's kind of public data you may or may not trust or to different levels you trust a certain storage provider

or pinning API. So the data that is stored there should be leaking as little information

as possible. So let's gather up those requirements of what we're trying to build. So we wanted

to have fine-grained access control. We wanted to store data with untrusted peers, which means minimizing leaked information. For example, the file hierarchy shouldn't be readable from
reading all of the encrypted data. We want to actually verify valid writes without read

access because, let's say, you have some kind of storage provider. They keep around your latest state of the file system, and they will be the entity that most peers will interact
with when they have updates in order to share them. And so ideally, they would be able to

figure out which ones of those are actually valid writes, which one are actually signed
by something that originated in a user's key. So we are using UCANs for that, plus also
something else that we've built into WinFS. And finally, another problem that happens

when you have a bunch of devices or you're interacting with a bunch of other people when you're sharing data or sharing access to data is that you will have concurrent writes. So those are all of the things that we're trying to solve with WinFS. All right, how do we

make this work? So in this section of the talk, I want to give you, with an example,
a small peek into what happens underneath the covers. So here's back, we need her novel

style. And she has a bunch of files. She has some notes on her thesis. She also has a rendered

thesis. And she also stores a love letter in her file system. Obviously, some of these
things she wants to share with some people and some of these things she doesn't want to share with other people. And actually, we don't only store the latest versions of
files, we actually store all of the history. So if she accidentally deletes something, she can recover it. And in general, you have this kind of Mac OS time machine-like environment

where you can always go back and look at what the state was before. Or if you're a developer,
maybe Git makes a lot of sense in that case. But actually, Winnie is very, very tidy. So

she organizes her data in the file system with a directory hierarchy, and she separates
her work from her life stuff. And now, if she wants to share something, she wants to be able to, for example, only give access to this final rendered thesis for her professor.

But if she has a work computer, she actually wants to be able to easily have the work computer access everything in her work directory. So how do we do that? Well, for one, we encrypt

all of the files with different keys. So every file having a different key means that if
you have the key for one file, you won't be able to read anything else, obviously. And

that also means, though, that now you have to juggle all of these keys. So what do you do? You build hierarchies on top of that. So one of the ways we do this is for revisions,

every time you write a new file revision, you don't want to do another key sharing,

like, basically dance with someone else. So you organize these keys in a ratchet. So ratchet

will give you a different key for every revision, but in a deterministic way. If you share the state of the ratchet, someone will be able to derive all future keys. But it also gives us this distinction between these two access levels where you can have access to a single revision of a file only, or you can have access to, let's say, the skip ratchet on the bottom
left, which will allow you to derive the key for that revision, or go up one step further
into in the skip ratchet and get that revision's key and so on for any future keys. And now

this gives us the distinction between temporal and snapshot access levels. Oh, and by the way, we're using a skip ratchet for this, which is an invention by Brook, and there's a paper about it. If you're interested in these kinds of things, it's super fun. Read it. So, yeah, we now have this distinction between temporal and snapshot access levels.

But we also want a hierarchical kind of access level so that, for example, if she has a work computer, she can just have a single key she shares with her work computer once, and at that point, it will have access and read all future values or files and directories in

the work directory without having to do another key sharing dance. So the way we do this is

we use a basic crypt tree kind of structure where we put the keys of the files into the
directory, and then we encrypt it, and then we get more keys, and we do this recursively
up to the root, right? Now, at this point, we basically just have a set of files and

a bunch of keys. Well, we have one root key that will be able to discover all of the hierarchy,
but for now, these aren't really, like, organised in any kind of structure. They're kind of
in a flat namespace, but ideally, in order to link to stuff, you can either use SIDS

or we use in this case something else to enable verified writes without read access, and the

way we do this is we give all of these ciphertexts a meaningful label. Right now, I've put the

file path as a label which obviously is leaking a lot of information about these files or

these ciphertexts, but from there, I will get on to how we do this so we actually hide

what the path is. So, first of all, we start with these paths. I will actually make them
absolute paths, but then we will replace every path segment with a random number. So every

file now stores both, well, this random number, because, in practice, it's going to be a 256-bit
number, as well as the human readable name for entries inside directories, let's say.

We call these numbers iNumbers. I think this is a term, if I'm not mistaken, from the web back then. It's a fun idea. Read about it, maybe. Basically, we call these iNumbers.

And finally, this also still leaks information. We don't know what exactly these files are
supposed to be, but you see, oh, well, there's a hierarchy in the files, and I see that some
things are in the same directory. That still leaks information. So, finally, we take out
of these file segments, or iNumbers, and we hash them together to basically get something

that deterministically derives a label for each ciphertext. And we also add in the key,

or something that is derived from the key, so it's unique per revision of the file, into that hash. That gives us a very constant-size identifier for all files, and even for across

revisions. And, actually, this is not probably a hash function as you're thinking of it.
It is not SHA2, SHA3, or Blake3, or whatever. That's why I put an asterisk in here. We're actually using RSA accumulators for this, which means that if you're sharing write access

with someone else, you can sign something that is going to look like random stuff, but the person who received that certificate will be able to present that to a storage provider,

or a pinning service, whatever, that is aware of WinFS, and will be able to prove that it only did writes to paths that contained some subset in this hash, essentially. But more

on that is much more complicated. Talk to me afterwards if you're interested in that.

So now we have all of these files. They're actually in kind of like a flat data structure.

And finally, we put an IP of the Hamt on top. And so we get a root SID, something to pin,

something to contain all of your data. Ta-da. We call this data structure private forest,
or sometimes jokingly a dark forest, because technically you don't know what other trees are in there. And in practice, we actually use this property. So basically, this is just a collection of things that you care about, and you may have one or two or three or whatever trees inside of that. So what do we get from this? Well, we're trying to leak as little

metadata as possible. You just have a flat namespace. If you have no read access, you just see a bunch of files, ciphertexts more precisely. The labels of them don't mean anything

to you unless you can read the data. The hierarchy is scrambled. And also, we split files in
practice into multiple chunks. So you can't distinguish a 512 kilobyte file from two 256

kilobyte files. And you can't, in general, see bigger files. We also get very fine-grained access control by the Crypt Tree method, as you saw. Basically, you can share different parts of your file system with different keys. And you have very succinct things that you can share. Sharing something means literally giving someone 200

bytes maybe. And they'll be able to read some subpart of your tree. And additionally, of

course, they need to fetch everything else over IPFS. We have different levels there. We have a snapshot and temporal level. And that holds for both files and directories. So you can have a snapshot access to a whole directory or a whole revision of the complete file system, for example.
We also get write verification without read access. But remember one thing that I mentioned
in the beginning that I didn't mention yet in there? And that's concurrent writes. So what do we do about that? So you have a bunch of devices. And they're doing stuff on their own. And now you need to reconcile these changes that they did in case they didn't communicate

with each other. But you have this problem that the person or the peer that is trying to reconcile all of these changes may not be able to read them. So what do you do? So you have one device that did a bunch of writes and created this hand. You have another
device. It did a bunch of writes. And on the one hand, it created some other data. Like,
you can see that some of the labels overlap and some don't. And in some way, they create
like conflicting data where the labels overlap. And basically, someone who has to reconcile

these changes is like, OK, now what do I do? Do I take the left or the right? Basically, what we say is you just merge them together. And we actually don't use normal maps. But we are making it a multi-map. So you just store both revisions. That's the whole trick.

But now, obviously, that means you're pushing the problem to the read time because you're trying to read this label, but you're getting multiple ciphertexts. It essentially means
you're trying to read a directory, but you're actually getting multiple. So what do you do? Well, you'll see, OK, now you have read access, though, when you're trying to access this. So you can see, oh, OK, so there's two different divisions, two different directories in the same revision. They seem to be referring to content-wise seem to be not the same. So

I can't just take one of them. In fact, they seem to link to two different versions of

a subdirectory at path A. So I go down there, and I look at these two different subdirectories.

And again, I can't merge them since one of them seems to be different from the other.
So one of them has a file inside of it, x.png, the other one has y.png. But now, merging

them together would be pretty easy, right? The obvious thing to do is just create a directory that has both of these changes. So that's what we do. In the next revision, we just create a directory that contains both of these things. And we go back towards the root and

fix all of these links so that in the next revision, you actually fix all of these issues. We also add in links back into the previous versions so that you can still follow the history and figure out what happened. And this also enables deletes to be preserved.
And in general, it's basically, if I'm hand-waving here a little bit, the CRDT clock that makes
all of this work. All right. So with that, we're getting concurrent right-reconciliation.

Yeah, that's basically all we wanted. But where are we actually at? Because not everything

that I've talked about yet has been implemented. So maybe let's give you an overview of what
is there at this point in time. So here's some example code. We have created a Rust

implementation, rswinfs, that is supposed to be very, very, very portable. So you can compile it to Wasm. There exist bindings for SwiftUI and Android. So there's people who
have been using it for iOS apps and Android apps. And we're using it at Vision inside

the browser. Yeah, here's like, you can find it on Docs.rs. And here's the implementation

inside of the Winifrest Working Group organization. It is extremely portable because we've abstracted

out a bunch of things that are very context-specific or host-specific. Specifically, that storage

network. So you basically just provide a block store with get block and put block as the interface. And what we give you is all the DAX surgery and encryption that you need to do. And also, you need to provide the secret storage externally. So that is going to be
very host-dependent, of course. What you get is a read and write and create directory and
all of that APIs. Essentially, all of the DAX surgery. Yeah, so let's look at some code.

So this is some Rust code. These aren't like, these APIs exist in this way and you can run
this code. But we are in the process of improving these APIs so they are more usable and more

easy to use, basically. But yeah, this works today. You can create some randomness. It

randomly generates keys for new files and new directories. You create some block store.
Here we're just using some in-memory block store. And you create this forest to put private data into. And there's a new directory. You can create some stuff in this directory. For
example, here's...

an MKDIR command that creates a pictures slash cats kind of directory. You also give it the time at which it was created and all of the access to all of the things that it needs. You can write some data to it. For example, here's a picture of Billy the dog. Actually, it's not a picture. It's just it just says hello world. But well, I couldn't fit the picture quite in there. And then you can do something like LS and read what's inside of the pictures directory and

print that. And finally, you can serialize the whole file system and get the root seed and that's something you could send to a pending API, for example. So yeah, the output of that is that in the console, you can see LSing something gives you like all of the entries in there and some metadata about it when they were created, when they were last modified, and these kind of things. And also I put up a QR code here, what you'll get is I see people happily scanning this, but they'll be disappointed when they see it's just a car file.

Or basically just a very, very unreadable DAG Seaborg kind of DAG, which is basically the private forest, but look into it and you'll just see a bunch of random looking data, which is exactly the point.

All right. Also, I'll be sharing the slides and the link at the bottom is clickable. So we do have a public roadmap. So what is implemented? We've implemented basically all the private file sharing stuff that I haven't talked about yet that much. We have some things that we're working on this quarter. We were also working on the RSA accumulator stuff on this day.

So that isn't quite finished yet, but we're getting there. The spec is basically ready. It's just an implementation matter now. And also we're starting in this quarter now, we're starting on conflict reconciliation stuff and also big data set support, for example.

So if you want to store like 10,000 entry directories, that's going to be a bit harder for us. We'll need to figure out sharding directories there. And so that's something we haven't done yet. If you're interested in these kinds of things, this is a community project.

So this is the whole idea, right? So you come and get involved with us. We'll figure something out. This has worked in the past. So for example, the function land folks have created this FX photos app. So this is powered by our SwinifS in the background.

It's connected to MetaMask as like the secrets kind of storage thing. It stores a bunch of photos that you can upload and encrypts them and puts them on IPFS. So that's these people here. And they have created the Swift and the Android bindings in order to create these apps.

We've also worked together with Banyan very recently, and they've already been happily creating pull requests and contributing to RS WinifS, which I'm super glad about. So yeah, that's basically what it is. Come use it, talk to us, and get involved. Thank you.

Any questions? We have plenty of time for questions.

Is the mic working?

Okay, this is better. Yeah, so you showed a picture of the conflict resolution when there were two different files added, which is obviously the easy case. What do you do if there are two files added with the same name? So how do you do conflict resolution in that case?

Yeah, so the interesting thing in that case is, well, we need to do it automatically, obviously, or we strive to do it automatically. So what we do is we compare the hashes and use the lower hash. The most important thing for us is it's going to be consistent across all of your devices.

What you can do on top and what we're planning to do is once you have these automatic merges, a app that maybe knows how to, let's say it's not images, but instead it is actually some app data, some JSON, and the app knows how to reconcile changes from concurrent writes, then the app can go into the history of this file.

It can see, oh yeah, there was an automatic merge. It will go into the history and see the two versions that were merged, and it will put on top another revision, another write, where these files were actually merged in the right way.

Well, the problem we're trying to solve here is we can't have the correct CRDT for all apps, obviously, so we need each app to do it in their own unique way.

But the history is available where the API is, so you can basically query the history and figure out how to do an app-specific merge. Okay, thanks.

Yes, exactly. There's another question over there. I have a question with regard to the write permission management. Does each drive or file system have something like a did or a unique identifier at the root, or how do you delegate permissions to it? Because so far it was all just SIDs, which are immutable, of course.

Yeah, exactly. So the system that we're... At the end of the day, RS WinFS is very agnostic about all of that. So for example, the function landfogs have based their whole system around wallets, and the wallets being the way that you do verifying these writes and how you store the secrets, etc.

What we've been doing at Fission is we've had a root pointer that is controlled by a certain DID, and we will use UCANs to delegate to different DIDs that may represent more devices.

And basically that is stored externally at the point where you have the basically mutable pointer for your file system. You could also imagine that being controlled by IPNS, but at that point you need some extra capabilities in order to make use of, let's say, the verified writes without read permissioning.

Because IPNS of course only checks if it is a SID written by a certain public key, or signed by a certain public key.

And is the idea that in RS WinFS, if you open a file system, that it would check all the capabilities in there, or is that externalized to an app on top? Like, if I get some SID and I know the root key, would I be able to check if all writes in there actually had the capability to do these writes?

Yes. So there's kind of two different levels. At the moment what we're working towards is a kind of like authorized data structure WinFS approach, where you can actually just grab a certain SID that is supposed to be a WinFS, and you can go through it and you can just verify that its state relative to some DID that you know is valid.

We're not there today, but that's basically on the roadmap is the peer-to-peer write verification implementation thing.

