
# ODD.js, a technical overview - icidasset

<https://youtube.com/watch?v=ByQbY3lNAck>

![image for ODD.js, a technical overview - icidasset](/thing23/ByQbY3lNAck.jpg)

## Content

Hi everyone, today I'm going to talk about the Auth SDK, or as some of you know it, it's

called WebNader before we renamed it this week. This talk builds on the previous talks

of the Vision folks, like Philip did a talk on WinFS this morning, this SDK builds around

that. The current version of the SDK has a prototype of WinFS and UCan, and this presentation

is talking about RS-WinFS and introducing some new ideas.

So what is the Auth SDK? It's a toolkit that allows you to create web applications, that

allow you to build an offline app without a backend, which is distributed. It uses all

the protocols designed by Vision, the main one is WinFS. Auth SDK app is entirely built

around a user's file system. The second one is the UCan, without that we don't have a

decentralized authorization. Thirdly, an important non-Vision protocol is DIDs, DeaDentifiers.

In the future this will also include things like IPVM and probably NNS as well.

So a bit more about the file system. It includes at least three partitions, these are bound

together in the root tree, which is a Cboard DAG. So you have public data, encrypted data,

and also a compatibility layer, UnixFS, which allows us to upload apps in the public file

system and then viewing those apps in the IPVS gateway. So the user is in full control

of their file system. This includes multiple private files and directories, which are sets

of AS keys and name filters. This is the private forest Philip was talking earlier today, or

dark forest as he called it. So this hides the encrypted IPLD blocks in a sort of hamped

DAG. And that gives us an immutable top-level SID. And then we have three crucial parts

in total, that's the IPLD blocks, the top-level SID, the pointer, and the private credentials.

So this shows an example of how those three important pieces could flow around the ecosystem.

So on the left, we have a file system and an app on a phone, and the same on the right,
but on a laptop. And in the middle, there's the protocols, or some of them at least, and

some of the vision infrastructure. So when we make a change in the file system on the

app, we push the IPLD blocks to the IPFS node from vision, and then we update the SID and

DNS. And then when the laptop is like a new device, it doesn't have the credentials yet

to encrypt those private nodes. So then we have to like establish a secure session, and

that's done using AWAKE and YouCan. And we can also announce the changes using WebSockets

to other active apps. But in order to access the DNS and IPFS nodes, we need some sort

of identity or an account. And that's done by registering the agent DID from the device

you're on, and that will become the root DID or account DID. And then we also need a username,

which is basically the identifier used for DNS, the subdomain for vision.name subdomain.

In the current SDK, there's only one type of an account, and that's our first class

vision account, which has like full rights. You can create apps with that. But in the

future, there will also be like an app account, which is tied to one specific app in the vision

ecosystem. DIDs are generated using the WebCrypto API, which is either RSA or AdWords curve.

The vision server also has a DID. So every time like we contact the vision server, for

example, when registering a new account, we create a YouCan, and the audience is the DID

of the vision server. And then when the registration is successful, we get a YouCan proof from

the server, which is also the proof of line that we have a vision account.

This is another example why we need a global namespace when doing private file sharing.

So we have Alice on the left and Bob. Alice is the share of a file, and Bob is the receiver.

So what happens here is Alice looks up the file system from Bob and then lists all the

share keys, which is another type of key, not the one used for signing YouCans, but

for making exchanges. And then we create the share in the sender of the share, and then

the file system is updated, like before, updating DNS and the IPLD blocks. Then the receiver

of the share looks up the new information, and he also gets a share ID or counter from

the sender of the share using a URL or some other form of gossip. And then using three

and four together, we can decrypt the share and using the share key on that device.

So all of that together is organized into components and layers in the SDK. These are

some of them. We made it possible to customize all of them, so it's easier for testing and

adopting to other ecosystems. So for example, you can change how and where the IPLD blocks

are stored, how to look up DNS workarounds, where to store keys, and so on.

So yeah, all of those things fit in components and layers. There's other interesting ones
like the capability component that allows us to ask other apps for capabilities using

UCANs. And all those components together form a program, which is what runs in the browser

then. Then you also have plugins, which are predefined compositions, basically, and they

can also have functions that create a program separately from the original SDK implementation.

So, for example, we have WalletOdds that creates a phishing account automatically instead of

filling in a form like you usually do in an app, but it connects to MetaMask and then

uses another component to get the root private ref from WinFS, encrypts that using MetaMask,

and then puts it in the public file system so you can load it on your other devices.

So yeah, that's it. That's the result. We get a simple way to create apps without the

backends. So for example, here we create a program. We have a user session, and then

we write to the file system. I'll try to do a live demo. So this is the photos on the

public file system and a separate private file system. Then you can see here that it

makes an update to the phishing server using a UCAN, which is the long token here. It's

quite long because of RSA. Then you could decrypt or decode the UCAN using the UCAN

website. Then this is an example from the DNS. When you do a registration, it puts the

DID key in DNS. The DNS link is the top level SID of the file system. Then you also have

the DID of the phishing server. This is an example of an older file system. The current

SDK has a prototype of NFS, and this is how it looks. This is DAGPB instead of C-BOR.

But it shows you the public and private file systems and the UnixFS compatibility layer.

I'll show you device linking. So this is the templates, which has a pre-configured

UI. You can use this to start immediately instead of using the SDK directly and implement

everything manually. That's it. That's the device link. Now our devices are linked, and

we can load the folders on our phone as well, if the connection is fast enough at least.

But yeah, that was it.

