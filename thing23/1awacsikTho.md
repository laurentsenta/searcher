
# AI data & compute - AI scaling limits solved via decentralisation - moderated by Iryna Tsimashenka

<https://youtube.com/watch?v=1awacsikTho>

![image for AI data & compute - AI scaling limits solved via decentralisation - moderated by Iryna Tsimashenka](/thing23/1awacsikTho.jpg)

## Content

So, hello everyone. Now, this is our last talk slash panel. And the subject is AI data

and compute, AI scaling limits of AI decentralization. So, I want to welcome our guests, our panelists.

Wes from Bakayau, Don from Nevermind, Zeeshan from Fusion, and Hari from Genesis AI. So, do you mind to quickly introduce yourself?

Hey there, my name is Wes Floyd, a member of the Bakayau team, also helping run the compute over data working group at cod.cloud.

And been in the space, crypto space for a few years, was working in enterprise before that, and really glad to participate in the panel today.

Alright, Don Gossin, I've been in crypto for a while now. I co-founded a project called Ocean Protocol, where we created compute-to data.

Yeah, totally different. More recently, I've spent the last three and a half years working on a project called Nevermind.

So, we're quite data-centric, therefore we've got to, we skew towards the data side of things, but we have this thesis that the future of AI will depend on its access to data.

So, we've created tools that enable AIs to discover and acquire data, as well as for the data publishers to control set access and enforce compensation.

Hello, I'm Zeeshan. Yeah, I spoke, I'm at Fission, working on Homestar and IPVM. Homestar is our implementation of IPVM.

I'm a staff researcher there, doing that among other things, and also one of the founders, organizers of Papers We Love.

And so, obviously a lot of thoughts around data, compute, AI. Back in the day, I used to do, kind of before the deep learning boom, I did music recommendation systems with the old machine learning ways.

But yeah.
Harry, could you please introduce yourself quickly? Sure. I'm Harry Greave, co-founder of Jensen. We're a deep learning compute protocol. So we enable deep learning developers to train enormous scale neural networks across ultra low cost GPUs.

Our kind of thesis is that the key bottleneck in the space is access to compute. It's something that I felt when I was leading a research team in an industry, essentially in the reinsurance catastrophe prediction marketplace, where we use more statistical machine learning methods to estimate risk.

Happy to be here. So let's start with the first question. And question to us, what are real world applications of decentralized AI and how they differ from centralized AI system?

Thank you. Yes, I think, fortunately, there's a lot of emerging use cases. I'll cover a handful, and then I know there's other folks in the panel that have a much more broad perspective.

Two in particular that I think have a lot of potential. One is on the decentralized finance side. There's limitations in the way that smart contracts operate today where it's difficult to do large scale machine learning training or inference.

And so if you could improve the machine learning you could do through a smart contract, you could look at more complicated use cases like hedging risk on complicated financial instruments. You could look at sort of derivatives contracts and sort of explore more sophisticated allocations of capital and defy.

Also in the emerging decentralized social media space, I think this whole space could be good for humanity, just providing an alternative to the big tech social media space. If I have a nice new UI and it's blue sky or it's lens and I'm scrolling through my app, what content should appear there?

Should it be a content based on an algorithm that I wrote or a content based on an algorithm that you wrote? Or how is it going to get fed? Because now we're sort of breaking away from the big tech monopoly on algorithms and recommendation.

So I think there's a lot of opportunity for machine learning there as well, and looking forward to seeing the space as it grows.

Nice. So a little variation on that, whether we're talking about like decentralized ML or AI or just the application of decentralized technology to those applications.

I think I'm going to scoot more towards the latter for right now. So I think there's a real need, a real drive for proper attribution. You see that in particular with like text to image creation that's taking place right now.

So I type in some words, I get a nice image out of stable diffusion. What is that based on? In all likelihood, it's based on images that were scraped, sourced from the public internet, but there was no consent provisioned from the publisher.

So the question becomes, can you trace and track that source material from start to finish? And then with a blockchain, you've got an innate payment gateway.

You could then potentially, if you can do that track and trace, prescribe proper attribution, compensation, royalties, residuals back to that publisher.

So the specific use case that comes to mind right now is Getty Images suing Stability AI for copyright infringement. Can you avoid that problem set entirely?

Can you incent a Getty Images to actually, I guess, modify or add to their business models and leverage this technology to do so?

Cool. Yeah, actually, we'll add to that. I think at Fission, we do a lot of work obviously with UCANs and capabilities and authorization delegation.

And we also built that into like WinFS, which is a concept of like having volumes that can be in directories that are private versus public.

So knowing which data you're okay with sharing with these systems that build models versus not, I think is actually really crucial.

So that's actually a huge one. And I think decentralization really lends toward data that can be private and things that you don't want to share.

I mean, a lot of companies now, even with ChatGPT, I have friends at large companies who are basically getting notices to say,

please don't put in any of our non-open source code into ChatGPT.

We'll separately buy a license with OpenAI where they have a separate API for that kind of stuff.
So the ability to kind of carve out what's for public and not public use within these models, especially because IP could come around here,

I think is a big proponent for decentralization.
Hari, do you want to contribute? Yeah. So very quickly from our side, the main kind of thesis we have for decentralization,

helping of scaling is around the economics, primarily kind of two areas.
So the first is by being a layer one or by creating layer ones, you remove the kind of large margins,

which are placed on the entire system by the kind of current cloud oligopolists.
So, for example, the computers, a 63 percent margin with EC2,
and a crypto economic protocols don't need to charge margins because the value accrues to the token as opposed to being extracted by a margin.

So you immediately kind of depress the prices, which increases the scale you can access.
But on top of that, you can also kind of pull together globally all the resources,

which are relevant for machine learning at various places in the kind of stack, be it on the kind of data side or more on the compute side.

And that increase in supply also depresses the price. So as opposed to paying the kind of exorbitant prices you see in the EC2 instances,
you can actually get a much kind of lower unit cost of compute, which means that more people can train large language models.

Yeah, so and this is coming to my second question,
how we can overcome these technical regulatory challenges of scaling decentralized AI?

You know, what's interesting is a lot of the popular machine learning frameworks that we use today rely on centralized training.

They expect that not only the hardware, but also the algorithms expect the data is close to the other data sets in the same data center.

And most existing infrastructures in the cloud or centralized systems, they've been built around that.
But there's a lot of good dialogue going on in the decentralized AI community about rebuilding the algorithms to operate in a more federated approach so that there is latency involved.

But if we have plenty of scale and obviously like, you know, with Ethereum miners, we know that we can source scale.
There are people in the world that want to contribute hardware. If you take a different paradigm, a different approach to the hardware, and you rethink the algorithms to a certain extent,

you can really achieve some interesting decentralized outcomes. There's a project that we're working on with the Bacquiao team right now around a component called insulated jobs where we want to do federated machine learning.

And so we're looking at projects like Flowers, which you brought up. It's a really good job.
Bloom, pedals.ml is another good example. FedML.ai, flock.io is another good group in this space. So I think as this demand for these use cases grow, you'll see hardware and algorithms all kind of come together to make it more possible.

Just to plug, nevermind. We had the Flower framework integrated like two and a half years ago.
But my drop.

Why not? I'm up here. Excuse me. I think education, but it goes without saying, I think what we really need to work towards is consent and in particular informed consent.

And how can that informed consent actually manifest? I think what we're talking about from our point of view, blockchain also serves a purpose there.

It's a global database where you can issue or burn informed consent.

So you can actually distinguish what should or shouldn't be available in the context that you are discussing.

And so from a regulatory point of view, if you can designate what's available versus what isn't, I think you can mitigate a lot of the risk that goes along with that.

For me, regulation is kind of a manifestation of a lack of control.
So how do you bring control to the publisher and also the user?
Because it's kind of incumbent on both sides. I want to know what I can use and I also want to inform the ecosystem what they're allowed to use.
So working on informed consent will go a long way.
Yeah, I think to the point on scale, I think things...
I look a lot back when I was working on networking stuff at Comcast, a lot of IoT edge device places we had compute at the home and compute at the hubs in between all the way to the centralized services.

So the way I think of it is not so much as decentralization versus centralization, but thinking about what are the...

There's actually tiers of where you want to do compute. Maybe you have to do training models at the large scale machines and you run those on your centralized cloud providers.

But within a compute context, you know that certain parts like applying information from a model can happen on your device at your home.

And then using the stuff we're working on with IPVM, which is also UCAN based, like what do I want to control within my set of devices versus what I have to go get or what I have to go compute on something at a different tier.

So I think that's one part of it. The other one that we're super exploring right now to a lot of what Wes was saying was how do you do matchmaking to figure out, again, looking at tiers again, how do you have a set of work that you want to do and matchmake with who should run that based on locality, based on the kind of job.

One thing that we're building into our implementation is bootstrapping nodes with information shared between what's your GPU, what's your CPU, all the process information that you have, using traces over time with receipts to actually look back over time and say these players haven't been as good or haven't been able to process to certain SLAs that you want to match.

So using history and traceability also comes a long way to kind of build toward that scale that we all want.

Haru, do you want to add? Yeah, I would just highlight that a lot of the kind of centralized training of the larger models that we've seen have been built with that paradigm as the only option. I think up until the kind of involvement of the kind of crypto networks we're talking about just now, there haven't really been alternatives.

You've had to build these large clusters, they've had to be centralized, and they've typically kind of used a few types of reference hardware, notably like the A100s.

What's interesting from our perspective is if you were to give AI researchers the option to actually use a kind of higher scale and cheaper alternative, it's a bit of a chicken and egg problem.

It would give them the ability to actually design systems which didn't rely on decentralized clusters. I think that's just a kind of chicken and egg point that there's another option, but we just haven't seen it because there hasn't been the opportunity to do it yet.

Yeah, so there are several times that you mentioned blockchain technology. Can you please tell me benefits and challenges of using blockchain technology for AI, data, and compute? Haru, do you want to start?

Yeah, in terms of challenges around blockchain, I think the number one we think about is kind of around verification. So if you want to use decentralized technologies, if you want to be permissionless, you essentially have to verify what is exceptionally computationally expensive work in a way that is

A, low overhead, and B, resistant to scaling problems. I think a lot of the kind of issues that we've seen in the space so far kind of come from people building systems that either don't scale well, so they rely on reputation systems, or they do scale, but the kind of cost to making the work verified is extremely expensive.

So just a good example, the expensive part would be, if you wanted to train a neural network on Ethereum, like on chain, it would be completely cost prohibitive due to the kind of the way in which the verifications run.

So, you know, almost the entirety of the work we've done so far at Jensen is focusing on how do you get that verification overhead as minimal as possible, whilst allowing that scale to essentially be uncapped. That's the kind of main kind of challenge that we've seen in the space of applying these technologies.

Well said. I think I'll save some time for the other folks. I know you guys are very much involved in some of the challenges. I think the biggest benefit is just the opportunity to both democratize and also to reduce friction in terms of the economic components.

I mean, we were just hearing about how Nevermind is focusing on enabling accreditation or credit given to the original data sources, and I think the ability to then pay folks and create those marketplaces is going to be really valuable.

So, challenges, overcoming, broadly speaking, the mental gap of why you need it. You know, there's a lot of education that goes into this stuff, which is one of the reasons why in particular, you know, as opposed to going someplace that looks like it would be a great fit, like enterprise, the time to market there is just exorbitant.

So, focus on where people understand it, and you know, you've got some control on that aspect. And I think, you know, the primary benefit from our point of view is attribution, right? But there's a lot that gets coupled to that.

Again, regulation, and how you manage the control of that information, how you navigate different data policies, GDPR, CCPA, HIPAA, etc. And then how those manifest across likely a number of different blockchains, smart contract or not smart contract enabled.

So, you know, there's a bunch of different permutations that and because this environment is, you know, that there's so much activity going on in the space, you know, that's also a hindrance to overall development.

Because, you know, there's a lot of different choices, a lot of different distractions. So, you know, it's also maintaining focus. Anyway, that's a few.

Yeah, I would say, I mean, one thing that, you know, we're also looking at and have been thinking about around this idea of delegation authorization in regards to the chain is, or in relation to changes, and Brooke actually was bringing this up in her talk that's happening like 10 minutes ago, whatever it was, I think fissions kicked around for a while called you can't, you can't pay payment channels.

So also using the same kind of things that we have capabilities and being like, are you able to run this? Or can you run this and then tying that to attaching that to payments?

I think another kind of positive note to this too, and why we're putting a lot of stock into wasm, of course, is that it comes essentially with metering. So you can actually gauge and and tie in, you know, cost with fuel that's run by a certain set of execution. That's much harder to do in other compute systems, right?

Where you have to just attribute payment to some, some understanding of what's being used. But wasm, you can actually measure that using instructions. So I think there's a lot of good tie ins with all the stuff that we're moving toward and the tools that we have to find better ways to incorporate payment tokens.

And of course, to something you mentioned, I think in the previous question, you know, using in a compute model like that we're looking at, there's also a lot of good examples of how, hey, certain parties don't want to participate or are not participating well in the network, you can kick them out, kind of fire and burn them, as you think you mentioned.

And I think that's, you know, a thing that to do kind of global massive compute this way, that's going to be a major proponent, these, you know, that you don't want, you don't want bad actors, right? So you have to have ways to remove them.

So how can decentralization improve the scalability of AI systems and overcome limitations?

of centralized data storage and computation? Why don't you start? Yeah, I'd be happy to. Yeah, one other thing that I was just thinking about that we didn't mention is particularly decentralization is this issue of encrypted data sets and private data sets.

Because obviously, if you're, you know, training on your own company's data, you have it locked in your own slice of your public cloud, and it doesn't become an issue.

But I think when you go to decentralized solutions, there's folks like Ocean Protocol, who have been working on this for a long time,

who are, you know, who are thinking about smart approaches to how do I maintain my private data set?
How do I encrypt it? And then how do I sort of share the economic benefits? And there's a number of really good projects in the space like Lit Protocol. Also, the folks at nCloud, encloud, are doing a lot of work in this space.
So that, you know, once you can say, OK, I have a health care data set that's relevant to my specific group, maybe I'm a nonprofit or research organization,

then maybe you have some other data sets or folks want to sell their own personal health data or whatever purposes and then get, you know, remuneration for that.

If you can still maintain privacy and have some level of compute over that private data or do some smart things with key management,

then you open up things that you really could not do in a centralized system, because it's very difficult to get millions of people to grant HIPAA authorization over all their private data sets.
But in this, you know, sort of crypto centric world, you can do it. So hopefully, with encrypted data, we'll get some other good applications. Yeah, I mean, obviously, that's a big part of the fission stuff as well.
Encryption at rest, encryption on flight, you know, with dreams of grandeur toward homomorphic encryption and being able to compute there,

which, you know, it gets closer and closer, even though it seems far away. And I think, yeah, I think actually to go on that, I think that that's the super really important part of it.

You know, one thing we've even talked about when it comes to this idea of affinities and capabilities, well, having the ability to,

if you have certain work that has to be signed, and you know, that's that you have to have that key.
That is a capability that you have for certain kinds of work. If you don't have that capability, you know, you can't run that transaction. Right. So you know how to filter these kind of things out. Right. So obviously, baking, baking security into your point, Wes, I think is like a huge kind of boon for what we want to try to do.

So just to add to both of these, I think, and you said it quite well, I think there's going to be this like blending of decentralized and centralized technology.

So for the foreseeable future, the development of like LLM will probably be centralized because they take up ship piles of bandwidth and compute resources and all this stuff.

However, transfer learning or augmented retrieval modeling, you know, that could really benefit across the board from decentralization,

especially if you have privacy preservation at play, because now what you can do is get more fine-tuned inferences,

but base that off of potentially multiple counterparties training that transfer learning model independently in a federated fashion across their data sets,

but the whole or the collective getting the benefit of that. So that's something that's potentially very powerful. And then obviously, tagging on all the economic components to this and being able to like incentivize multiple counterparties to perform these types of activities is just, it's a superpower.

Perfect. Thank you. So how can decentralized machine learning techniques can improve efficiency and accuracy of AI models? Who wants to start?

Harry, do you want to start?

Yeah, sure. I think our kind of house view is that, you know, more is better.

So the more kind of scale you can apply, broadly speaking, the better results you get.
The scaling laws have held relatively well over the kind of recent iterations of large models.

There's kind of chinchilla scaling laws about whether or not you scale the parameters of the data, but broadly speaking, more is better.

It's quite clear that, you know, the model, you know, that results in better model accuracy of various tasks.

We saw that with like the GPT-4 increase in various kind of like academic benchmarks.

When I kind of think like longer term, well, how do you kind of get accuracy improved further? It basically looks like hooking up more and more and more compute.

And there reaches a point with centralized clusters, at least, where you just physically can't connect more.

You know, if you think about like a kind of a physical building where you're hosting A100s, etc.

You reach a point where you just can't fit anyone in the building.
Or there's just sort of just electricity limits locally. Long term, medium term, having a global cluster, a universal cluster that you can access solves that problem and allows you to build ever larger models.

So I think the benefit, the accuracy improvements at least comes from the scale.

I think one interesting scenario that might emerge, especially, you know, with FEM launch, a lot of people are talking about data DAOs like Ocean Protocol and other Lagrange and some other folks.

If you could have a hybrid scenario, there's a lot of work in this group at Stanford called the Center for Foundational Model Research.

And they basically build these massive models that are trained on large data sets. And they're generic enough, but they're still effective. They're foundational model. If you could have a private company offer a foundational model that had the resources of centralized compute.
But then you could tap into the unique data sets that people would buy and sell over decentralized systems and have the best of both.

I think that would be a very interesting kind of combination. If you could have a DAO that was actually paying a private company to use its foundational model. And the DAO trained its data set on specialized things that it had access to, like weather data or whatever proprietary data set it's at.

That would be kind of a fun and interesting way to go beyond the current status quo.
So if you tie scale to relevance, basically we're looking at a state.

I mean, there's a couple. Well, there's more than a couple, but there's a few ways of thinking about this. One is you can work on the parameters of the model.
It requires a lot of knowledge and expertise. Alternatively, what's going on right now, you're just throwing shape hulls of information at the model and training it that way.

If you align with that school of thought, there is a wall that you hit with the amount of data that's freely available from the public Internet.

And what ultimately happens with models that are trained in these conditions is they basically converge to the same responses.

So sure, that's scalable. But from a model point of view, from an AI relevance point of view, that's not really scalable. If Wes and I come up with the same response, who's better than the other?
It's not really scalable from a business point of view anyway. So now the question becomes, okay, if scale is attached to relevance, then we need to access information that isn't necessarily publicly available or requires consent in order to avail itself to the model.

So I think enabling that access is paramount and working towards that type of future.

Yeah, I mean, I'll head on, I think, going back again to that multi-tier term, which I think is a really legit thing that a lot of people are working on, particularly in academia as well.

But the ability to have it, to the point, you need more scale, but knowing that you can filter information that's not important for a model locally.

Or when I began back at Comcast, we used to have how voice remote worked, which was really well done.

And so obviously a lot of the modeling was built in lots of data centers, centralized servers. But the hardware on the set box was getting better and better and better.
It could run WASM, it could run these kinds of things. And so you could do some small scales of compute for localized data there that then can be deemed important for the more aggregate, larger training models that you want to do.

And again, there was a middle ground as well. At an ISP like Comcast, they would actually buy houses and put small data centers to be like node hubs in certain places before you have to go to, which for us at that time would have been data centers that we had or Amazon or whatever the case might be.

So really understanding that there's these different levels where you can actually try to reduce the amount of data that you need to also make things more economic or augment data that you need closer before you have to go to the larger models actually will really help at scale at the large.

Thank you. So the next question is, how do you ensure privacy and security of AI data in decentralized network?

Yeah, I think there's been a really good history of that.

Particularly if you look at some of the other social media technologies working on decentralized technology, a lot of times they prefer the NFT approach. So NFT is your way of owning and having a token for your data set.

And then your data set can then reference data that lives in IPFS, potentially, as an example. It's a very popular technology.

So your data lives in IPFS, you own it, it sits you, you can trade that and move it around.

That seems to be a scalable mechanism, particularly if you're on either a mainnet chain or an L2 chain that can scale to the number of transactions and things like that.

So that feels like kind of a natural emerging way to handle data management.
I think like every other technical problem, it's use case dependent.

So right now you see a lot of use cases where privacy isn't a concern. Everybody's throwing their data up to chat GPT, like, okay, cool, Microsoft's got it now.

But there's lots of different techniques to use, pseudo-anonymization, trusted execution environments.

It just depends on how accurate your information needs to be if you're training a model, right?

How reliable that is if you are leveraging privacy preserving techniques.

And then if you're using trusted execution environments or MPP, it's a question of what's the latency on those techniques.

MPC, sorry, not MPP. Yeah, I mean to kind of go with that, and again, I was working at a company that's done a lot with UCANs and trying to really push that with execution in a compute graph.

But also, I think actually you mentioned the trusted execution stuff like SGX and stuff like that.
And when I think of the capability model, again, to me that's another form of like if I'm a node that has that and I know how to work on certain kinds of data,

we'll be under the SGX umbrella that's going to run. I come up and I register with that so you can use me and then you can pay me because I'm running that, right?

And so, and those are like very good positives that really come out of like understanding the space of registering and understanding who's available for what.

And so, you know, if you're really good at execution with privacy, you'll probably get more money out of it, right?

And that's a big one. Hari? Yeah, it's saying the concept is, as I admire what someone said about it being a use case dependent, when you think about the kind of large, the largest of the language models,

currently they're trained, you know, exclusively on open data. So if the use case is kind of training those models, then the privacy concerns are less kind of salient.

I think it was something that we saw when we interviewed kind of over 100 AI developers was that we were surprised by the kind of the percentage that actually had data privacy as a primary concern.

It was definitely there, like there was over kind of roughly like 40% of them were, had that as a top concern.

But breaking up by use case, there was a large percentage who actually didn't have it as something top of mind. I guess on top of that, there's a second consideration as well around, you know, there's the training data privacy, but then there's also the kind of model parameter data privacy considerations.

So for example, you might not mind what the kind of, about the training data being public, but if you're, you know, building specific or you're tuning specific parameters, those can actually be quite important.

And you don't want those to kind of be leaked to other sources so there's considerations on the kind of, I guess network parameter size as well.

Okay, and last question of our panel. I would like to start with you. How can decentralized AI contribute to solving global challenges such as climate change and healthcare?

Yeah, I think the single most important thing we can work on as a species is building artificial general intelligence.

I think it occupies a kind of category of technology where there are just unknown unknowns about it in terms of what they can fix. I think a lot of people that's terrifying, because it kind of raises these existential questions around, you know, doom and alignment, etc.

But having, you know, access to a tool which is legitimately smarter than us in ways that we can kind of fully comprehend I think is one of the most potentially beneficial things that we've ever done as a civilization.

I think the most kind of immediate kind of short term impact of it is purely just in freeing up humans from, you know, labor which just isn't, you know, isn't kind of creative. So, so much of the kind of, you know, LLM tech generation that we've seen.

It automates away a lot of administrative work, which isn't the kind of best use for kind of human time, it can be spent on much more creative endeavors. And I think yeah short term there's that, but long term the kind of unknown unknowns are the most exciting part there.

Yeah, in general, the thing I like about crypto is that I think in particular like AI use cases for it is that it doesn't necessarily have to be a better solution than the current centralized solutions.

But if it can provide a viable alternative that can make a massive impact for humanity in the sense that, you know, Bitcoin didn't need to be a better alternative than US dollar but once you have it now it's sort of it's an alternative and now eventually traditional fiat systems have Bitcoin to contend with.

Ethereum didn't need to replace the financial system and financial contracts but now it's an alternative which takes away some, it provides sort of a check. So I think in the same sense, if users can have better privacy with decentralized AI and better trust and better representation of the societies that they operate in, then that's good because that's a check and then that means that traditional tech companies will have to contend and it'll be a healthy market force for other folks.

So this is pretty specific but we spent a lot of time in Deci, another shameless plug, Nevermind was behind the first IP NFT.

So anyway, when you look at that space, obviously the records that pertain to patients is quite important as it relates to the subset in Deci that's focused on healthcare and what emerges out of that is again this idea of informed consent both on the healthcare side and also on the general research side.

So you can leverage AI to help with that informed consent process both from a cost displacement point of view where you don't need to have as many experts in the loop helping to solve that challenge of keeping the patient informed but also potentially scaling that capability.

So you can actually potentially include more patients in your research by scaling that informed consent process.

I was going to say too that study you were mentioning, that's not published anywhere, right? The developer study you did, AI developers, Harry?

Tim?

You want to include that for certain kinds of models, but you want people not to be able to have to give away their information.

That's super key. I think the other thing too, with hardware changing as well, as we know, my MacBook now has 10 GPU.

It's pretty good. It can run stable diffusion. I forget, I was rereading the OG stable diffusion paper. As much as it is about a lot of the great work they've done around working with images and the learning model,

a lot of that paper is how well engineered the algorithm is to actually run on less powerful devices.

Still more powerful than my phone, but not needing as large clusters of compute to do the work that it does.

That's a really key part of the paper, in the abstract in the beginning. I think that ubiquitous hardware concept growing, and we know how Moore's law is out the window,

and all these things that we've seen over time, that's going to showcase that we can do a lot more work in a decentralized way.

That brings people who work in climate change, who work in places who never maybe would have the money or the sources

to actually run certain models that they want to run on their data, they now can. I think that's where everything opens up. Can I just add one thing? We kind of missed the most obvious scalability thing attached to AI.

AIs aren't going to have bank accounts, so how are they going to transact? Blockchain.

Yeah, great point. Thank you. Thank you everyone. This is the end of our panel on AI data and compute, and also end of our track on decentralized computing AI.

Thank you everyone for coming and listening. 