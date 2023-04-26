
# Compute on data in space - Yan Michalevsky

<https://youtube.com/watch?v=nCI2qgH1Ha4>

![image for Compute on data in space - Yan Michalevsky](/thing23/nCI2qgH1Ha4.jpg)

## Content

I'm Jan Michalewski. I'm one of the founders of Cryptosat. We're a team that's based all

over, headquartered in the US, California, and New York. And I'll be talking about compute

on data in space and tied to kind of what we're doing and also integrations with the

Protocol Labs specific projects. It's a sharp talk, so I'm going to keep it very high level.

Welcome to ask me questions at the end about specific topics and dive into details or just catch me after and ask whatever you feel like. Cool. So in terms of the general mission of Cryptosat, we essentially build the trust infrastructure for Web3, that's how we like to call it, in space. And we do it by launching,

essentially building, integrating, and launching satellites into low Earth orbits for Web3

use cases, cryptographic protocols, and confidential computing. And the confidential computing aspect is what I'm going to focus on in this talk. So here you see the launches of our first two satellites. One of them was launched in May 2022, and the other one was launched just earlier this year in January. In terms of

some of the milestones, we did a couple of experiments aboard the International Space Station to test different aspects of operations with the assets in space. Launched our two

satellites. And one thing I wanted to mention is the recent participation in the KCG ceremony,

if you were following that, that's related to Ethereum scaling. So it's essentially the centralized ceremony that requires parties to produce public parameters that can be trusted
later on to be produced with computational integrity in a certain way. Otherwise, the

scheme can be compromised. So this is something we contributed to from our second satellite in space. And we're working on launching more, basically working towards the constellation.

In terms of the very high-level architectures, you can see here a mechanical design. You can see that it's basically this little thing with multiple boards, including an onboard
computer, including computers that serves for the cryptographic operations that we're doing. And in addition to the satellites themselves that we're launching, we also connect to a
ground station infrastructure and provide convenient APIs for users to be able to directly

issue requests to those satellites. And that's not very common, actually, in the space. Usually
it involves a lot of coordination and operation. And our vision is to get to a very simple, trustful API, plus direct integrations with smart contracts that we're actually already

doing that would enable users to request certain operations and have them completed in space.

Here, that's a screenshot from our tracking website. So that shows two of the satellites

we have. So you can see the different locations earlier this morning when I was updating the
slide. And another thing we built that's kind of nice for developers is mostly for onboarding

and explaining the different use cases that we're addressing is what we call the CryptoSat
simulator. So you can go online to simulator.cryptosat.io. And it's basically kind of an interactive

tutorial plus satellite tracker and a playground in JavaScript where you can try different

APIs. And it basically exemplifies what you can do with such a satellite, different use

cases that it can serve, and so on. So basically a developer onboarding tool.

I want to give one real-world example of a use case that we're serving already. I have

customers, some customers, for. So one of the use cases is essentially trusted setups
for cryptographic schemes. Some cryptographic schemes require public parameters, just some

numbers that need to be produced in a certain way. And if it's not being done correctly

or it's being done maliciously, it can potentially compromise the entire protocol. One example
of that which you're well familiar with is ZK-SNARKS. So some schemes require a trusted setup. Another example, polynomial commitments, which is also part of ZK-SNARKS but can also
be used for other things like the KZG, what KZG is used for in Ethereum. So those are

two examples of cryptographic setups that need to be produced in a certain correct way.

So here we're basically providing kind of a trusted execution environment, completely physically isolated. And that's the important point that we're trying to emphasize about our solution is the lack of any physical access and the ability to compromise anything in
memory, use any side channels and so on. Basically a trusted execution environment that can do those things for you. And two examples of that was the participation in the KZG ceremony
that I mentioned. And another one is producing a trusted setup for a ZK-SNARK scheme that's
used by the DoraHax DAO that serves for community project funding.

And now I want to get to decentralized cloud compute and tie it together. So it's essentially

a growing market, kind of emerging for the past couple of years. And there are different

examples of that. So within Protocol Labs, the Baka.io project is one example. There

are also additional projects like Super Protocol. Cache is more of a marketplace. And just today I met Sami here and talked to him and learned about TauByte, which is a serverless decentralized
compute. I hope I'm presenting it correctly. So those are just some examples of that, focused

on different aspects of decentralized computation. And one thing that can be very relevant to

this domain, not applicable to each and every use case, but is often important, is providing

confidentiality and integrity for various sensitive workloads. So often users of decentralized

clouds would like to process sensitive data potentially. And ensure that an attacker,

a very powerful attacker that has complete access to the infrastructure, cannot compromise
or leak their data and infer anything useful from it. On the left I put this example of genomic data that can be very sensitive. But you can think of other examples of financial
data, etc. The other aspect can be getting an assurance of computation integrity. Basically

getting an assurance that the computation is done correctly. And the cloud is not taking

any shortcuts in terms of not producing the right result and not putting the computational
effort needed to produce what you needed to produce. Or is not doing something maliciously.

So here I put those hands with beads, basically referring to something like an auction. So

for instance, think of a sealed bid auction, where you need to reveal the winning bid.
And nobody knows the actual bids. They're sealed, they're encrypted. So you need to trust this party that's commencing the auction to do it correctly and produce the right result. So for that you need a trusted execution environment, or you need some cryptographic means to achieve
that. I'll mention the cryptographic solutions first, but then we'll proceed to the other

alternative, which is trusted execution environments. So in terms of confidentiality, something
like homomorphic and fully homomorphic encryption potentially can be ways to achieve this goal

of working on data that's protected and encrypted, even when you're actually processing it. And

for integrity, we have SNARKs and ZKSNARKs that also preserve the privacy of the input.
There are different nuances here in terms of what you can achieve with those. There are practicality, a lot of, obviously, constraints around performance. So I'm not going to go

into all of that, but happy to discuss that after if anybody is interested. And I'm mostly

focusing on the alternative of trusted execution environments for achieving both confidentiality
and integrity. So jumping to compute and data in space, and I'll make it very short. I don't

have time to dive into all the details. But essentially, providing this kind of trusted execution environment, we can apply it to decentralized clouds in the following way. For instance, taking Bacayau as an example, we have this architecture that we have this

diagram that in a nutshell describes the Bacayau architecture. It has compute nodes that can
bid on workloads, on processing different requests from users. And they can run it on

different types of executors, and there are verifiers that are verifying the results. Often they'll verify that there's a consensus between different compute nodes or executors
that processed a certain computation. And basically, in our case, what we're proposing,

what we're looking into actively right now is the option of having this kind of an executor

in space. Basically, having one of our satellites or a constellation provide those executors

in space, and having ground compute gateways that would be able to bid on sensitive workloads.

Basically to the extent that the user can be very specific about it, and adding this kind of a flag that we called run in space, indicate that they want a workload to run
in this kind of a physically isolated environment. And in this case, it will basically go to
a satellite, the compute will be done there, and the results will be returned to the user

with the attestation that it all happened on an authentic crypto-sat satellite.

In another example of super protocols architecture, so that's their architecture slide, and basically

the idea here, and they have this notion of a TE provider. So the idea here is that we
provide it via one of our satellites, and that becomes the trusted execution environment plugged into the rest of their infrastructure that can process data. So just wanted to mention

those two examples and show how it applies to different architectures. An important aspect
of all of that is attestation. So attestation in this space of trusted execution environments
is this concept of proving that you're basically running whatever you want to run in an actual

trusted execution environment and not somewhere else. Because that's crucial to this whole concept. If somebody just tells you, oh, it's OK, it's running in SGX, or it's running on

a satellite, or wherever, you cannot trust that you wouldn't be willing to deposit any
sensitive data unless you get some cryptographic assurance, and that's usually done with attestation.
So first of all, proving that the user is sending some data to a space-based node, in our case, and also attesting to the authenticity of any produced results. And the way to achieve

it is the following. We basically have this key generation ceremony that starts after

the launch of each of our satellites, where we generate a keeper, or a couple of keepers,

after the launch. So the private keys never leave the satellite. And we start broadcasting the public key for a certain time, such that multiple participants, multiple ground stations
can receive it, and gossip between them and see that they agree that they're getting the same public key. So there's a signing and verification keeper produced, and also an
encryption keeper. And the public encryption key can enable you to encrypt some data that
will only be accessible later on by the satellite. And also, every result that's going to be
produced by our satellite is going to be digitally signed, so you can verify that it came from
CryptoSat using the corresponding public key. Just one thing I'm asking for a small help
with, so we launched this tweet from space campaign. Tweet at us at CryptoSatBot on Twitter,
and we'll get your tweets signed in space by one of our satellites. So that's a small PR campaign we're launching. Help us with that. And yeah, with that, I'll conclude the talk, and happy to talk to you after. Thank you so much. Thank you, Jan.

