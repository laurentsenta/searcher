
# Project Lilypad: On Chain FVM Smart Contracts Invoking Off Chain Compute Over IPFS - Wes Floyd

<https://youtube.com/watch?v=X6LY01KEY8E>

![image for Project Lilypad: On Chain FVM Smart Contracts Invoking Off Chain Compute Over IPFS - Wes Floyd](/thing23/X6LY01KEY8E.jpg)

## Content

Thank you all for joining our session. As Irina mentioned, we're very passionate about

off-chain compute. Off-chain compute is really only interesting when you can tie the trustlessness
guarantees of a distributed consensus technology like FEVM. So my brief session is going to

talk about the power of on-chain state, bridging it to invoke off-chain compute. We really

want to have the best of both worlds. We want to have our cake and eat it too. And of course, please go to our GitHub page, check us out, smash the like button, and all that good stuff.
So this session is really about making your hearts, making your dreams come true, effectively.

So from a demo perspective, these are some things that I love. I love the movie Back
to the Future. Does anybody else like Back to the Future? Yeah, yeah, thank you, thank you. I also like 19th century French modernist painters. I don't know if you guys knew that
about me. That's the thing about me. I also really love off-roading. So these are the
three things that I love the most. And this is what we're going to use Waterlily to bring to bear. So before we get that, let's kind of walk our way up the stack. So Irina gave you some perspective on Bacoyal, which is compute over data. You can think of it as sort of an L2. It has a lot of ties to EVM. If L1 is the sort of the Filecoin chain or

the virtual machine there, Bacoyal would exist as a layer on top of it. And there's definitely some debate about how those fit together, but effectively, this is how I want you to have sort of a mental model of compute. Project Lilypad is the first component to invoke Bacoyal

from any smart contract. Hopefully, you will be building FVM smart contracts soon, if you're not already. And when you're building those smart contracts, you might think, wow, I want to be able to invoke some communication with the outside world, or I want to do some complex
math. It's too heavy to fit into my smart contract. In fact, this is actually a really important turning point for Web3 in general. There are a few off-chain solutions, off-chain

decentralized compute platforms today, but being able to invoke them from a trustless
chain is a very unique thing. So this is one of the first projects to ever let you do this.
And hopefully, this will inspire you to come up with your own interesting project ideas. And so yes, this project itself, the source code is available. Please check it out at Lilypad on the GitHub URL there. And just to give you a brief walkthrough of how this is going to work, from the contract that you write, you're going to invoke a Bacoyal job.
You're going to call this Lilypad contract through an interface that's easily available. Lilypad contract is then going to broadcast an event to the FVM chain. It's going to actually do, I forget the specific function, I think it's.send. Anyways, it's going to put that
function on chain so that it's written, it's broadcasted on chain. There's a daemon right
now, which is the bridge between Lilypad and the compute nodes, which is going to be listening
for that event. It's going to say, oh, okay, someone created a request to run an off-chain compute job. Then it's going to actually run this job here, number three, and number four is going to be something like maybe a standard CLI invocation. It's very close in native to the Bacoyal network. Over time, you'll probably see these consolidate a bit more. There's opportunity to further decentralize and such, but this is just a good initial implementation for reference. Then the Bacoyal network will actually run the job. Most importantly,

it will return the results to its preferred medium of storage, which is IPFS in this case.

The data's going to live on IPFS. It's going to post that back to the Lilypad contract. Then the user results will be returned to the user contract. That contract has then

requested some off-chain compute. It's happened. Now the results come back. You can do some very interesting things with it. We'll show you guys some examples here in a minute. Then you celebrate. You have your results ready to go.
Project Waterlily. In order to showcase the power of this end-to-end capability, we thought, let's do a couple things. Let's do something that benefits humanity. Let's do something that shows off some of the latest AI capability, in particular, stable diffusion and neural
style transfer. Let's also showcase FVM payments as a part of this capability.

If you've never heard of stable diffusion, the terminology comes from when you take dye,
drop it into water, and then the dye slowly stabilizes effectively, kind of an interesting,

clunky-looking color mesh. This is a very similar technique that they use for stable
diffusion and image generation, where you start with a lot of noise. Over time, you refine that randomly generated noise to an image that matches a text prompt that you
get. In this case, this is a person that's half Yoda, half Gandalf. Over a series of iterations through the machine learning model, it actually gets to something that looks like Yoda and Gandalf. That computation is important because this is an example of a computation that's way too large to happen on a smart contract. FVM smart contracts are very powerful because
they're trustless and they're verifiable, but they're not as open-ended. You can't do arbitrary compute like this. This isn't a good example of what you want to do. Here's an example of what these might look like, where you say, for adding to stable

diffusion, a thing called neural style transfer, where you say, okay, I have a picture here of a nice car out in the distance. Then I want to apply a classical painter style to

it. Now, it will refashion that neural style transfer accordingly. We're going to do two things. One, we're going to generate stable diffusion. Secondly, we're going to apply neural style transfer, but we're going to do it for those artists that might be underrepresented or artists that haven't had an opportunity to commercialize their work, hopefully provide a little bit of benefit to humanity. If you're interested in the technology behind that technique, this is a nice diagram towards

machine learning or towards data science, rather, which goes into more depth on how that works. If you will open up your browser to waterlily.ai, we're going to pull up a

quick demo of the Waterlily capability here. This is publicly available right now. You

can use the main net version, which is just waterlily.ai. You can also go to Waterlily and add in the network Filecoin hyperspace. You can start working with it. You can type in some text here. In this case, I want a rainbow unicorn. Then what you'll do is you'll

choose an artist whose work you want to, or style, rather, you want to apply to your generated

image. We've prebuilt these models with artists in the past. When I choose, let's say, Tanya here, in this case, there's some examples of the different types of artists. We've taken backlog of, let's say, 40 images from those artists and we've trained their style. What we're doing here in this case, when I say generate image, this is invoking the contract on FVM first. That contract, let me go back

to the visual here so I don't have to use my hands too much. We're calling that contract on the FVM network. FVM is then going to build the style transfer and the stable diffusion
on the back end. It's going to return our results to Lilypad contract. We're going to see our output here in user experience. Let's say rainbow unicorn with lasers. It's going

to be a very powerful rainbow unicorn. It's going to submit the job to the FVM network.
It's going to prompt me to pay in T-fill here. It's not too expensive, a fraction of T-fill,

so I'm going to confirm that. Now it's going to create the transaction on FVM. FVM is going to go through its process and it's going to invoke BapiYau on the back end. But like any good demo, this is going to be a bit of a baking show, so I want to show you guys some results, some examples of what it looks like. Coming back to our slides here, this is an
example of saying, let's take the style of some public domain artists, in this case some
18th century hand-drawn art, and generate some fun images like this is what Barack Obama

would look like if that artist had drawn it, or Mickey Mouse in this case. For me, my favorites,

DeLorean 4x4 and Offroad, this is my dream. This is what I'm going to hopefully be able

to buy one day when someone decides to make it. But until then, this is the next best
thing. Hopefully this gives you a good sense of invocating these arbitrary large compute

jobs off-chain, but still with the trustlessness and the verifiability of FVM. There's a lot
more that we're going to start building with this. There's a lot of projects in the greater
Web3 ecosystem that could benefit from this capability. There are a number of DAOs that
we work with where they want to enhance or augment their existing membership capability.

We have a group that we work with in the science space that says, if we could do some off-chain

bioinformatics workloads to simulate protein folding, and then maybe capture that as an
NFT, and then our members will have this NFT of this unique science work that they generated off-chain, that would be a fun validation of their membership. We have some folks that work in the DeFi space that generate very high-quality data sets. One of the things that the DeFi space is always chasing is better yield, better returns. Where should I put my money in different DeFi projects? In today's world, most of these smart contracts, even
the best order routers, rely on computations that have to happen on-chain using current state data. The big next threshold for DeFi is to say, if I can take into account all

the history of the Ethereum chain, and I can make more complex calculations, like you would do in high-frequency trading, for example, you can make more sophisticated decisions, you can increase your yield, etc., etc., etc. Lastly, one interesting one is decentralized
social media. Has anyone heard of Blue Sky or Lens Protocol in the audience? Okay, that's
about at least half, maybe more. This is a tremendous burgeoning space. In the sense
of social media, you may want to be able to not have to rely on the tech companies' determination

of what content you should see being fed by their algorithm. You may want to have your own algorithm. If that's the case, you're going to need a more complicated off-chain compute system to help build your algorithm of feeding the type of content that you'd like to have. So these are just some examples of the types of things that we're hoping to provide in the future with the bridge. Thank you guys so much for your time. Please follow
us, like us, and all the details there. And then, thank you so much. That's all I have.
I assume the stable diffusion example is like, we can run some compute and it's tied to an

FVM job, but there's nothing really about the FVM that you need in order to run stable diffusion, or unless I'm missing something, where there's a specific advantage of bolting the FVM onto the front of it? No, you make a good point. For stable diffusion, these
sorts of AI, even if it was chat-GBT or LLM and things like that, you do not necessarily
have to have FVM in general sense. And if you go to the docs.bakayau.org page, you'll

see a bunch of machine learning inference examples, like OCR and fun stuff like that, so you can invoke it directly as well. So I guess what I'm asking is, what are you

hearing from customers where there are specific FVM-related off-chain compute that is driving

this? Where does customer demand come from here? Yeah. Yeah. That's it. So just to replay your question, where is demand coming from for the combination of FVM and off-chain compute? There's a few, in addition to the sort of
the DeFi and the science example before, there's a couple others where I think Matt had a little
bit alluded to these data DAOs in the past, is an important use case. And so these data
DAOs, like one in particular that I try to follow is one called Lagrange DAO, with a
person named Charles Kao, a Phil Swan team, and they're building a data DAO very similar

to the work that Ocean Protocol has done. Ocean Protocol has been a leader in the sense of having private compute for a very long time. And the premise is essentially, I want to retain ownership of some private data set, but I also want to be able to sell it in certain
circumstances and ensure a certain amount of privacy to compute over those data sets.
And so in those cases, when you need a transaction, Bacco Yau itself does not have an economic,
a native economic component yet. So FVM provides that. And then it also provides the sort of
the public marketplace built in for those data sets. So I think like with the data DAOs and some of the other things that are being built on FVM, the off-chain compute will be a nice complement just to expand FVM's capabilities generally. I think that's probably where demand
is going to come from. I just have a clarifying question. So where does the, I think the previous talk talked
about Bacco Yau brings compute to the data. Where does the data that, where does the model
that encompasses the data actually live in this example? Yeah, and just to clarify your question, you mean when you say where does the model for the data live or? Or like you're submitting a job to AI stable diffusion model, model a set of weights that

have to exist somewhere and like where, where is it, I guess, where is it running? Like where, where is that diffusion model stored? Very good question. Yeah. And so to Irina's point previously, like you know, Bacco Yau
has done a couple of things. One, it's implemented compute along with data that lives in IPFS,
which may or may not always be native to where the data lives, but there's larger data sets that live in Filecoin, larger data sets that are too expensive to move. And so in those situations, we'd like to send the Docker container workload or the ML training workload to where the data lives at the Filecoin storage provider. How does that work with sealing unsealing? Do you have to have some sort of relationship with the storage provider as a compute node in order to make that work? That's exactly the right question. The way it's going to work is that initially the focus will be on unsealed data sets effectively because there are some depending on who you

ask, maybe 50% of the data in Filecoin might be retained as an unsealed data set. So we'd like to give them better utility, better economic opportunity to earn and profit from, from those data sets. But longer term, I think there's an opportunity for more of like a medium layer, like a hot layer. That's like sort of like more than IPFS pinned, more guarantees,

but maybe not as hard to get to is sealed Filecoin data. So there's a lot of work going
on in that space. So like doing like, yes, this section, this sector is like actively data that needs to

be actually worked on all on seal it and make it available to people I'm working with and then I'll put it back in the cold storage at some point. Yes, completely. And then a completely unrelated set of questions. So I, we were, my team was working on Ethereum

back in November and we were working with Chainlink in order to do our Oracle queries for part of our proof process. This seems like this is like a comparable solution built

on IPFS. How do you kind of see this, like this project as it relates to something like
Chainlink and bringing that functionality to FBM because we need Oracles. Yeah, no, thank you for bringing that up. So I, I'm a huge fan of the work that Chainlink has done in the Oracle space. I mean, they've really like, Oracles have existed alongside Chainlink, but they've really led the forefront. So it's extremely important. And we were talking about this on our team, you know, what do we call this? Do we call it a bridge? Maybe it's kind of a bridge, maybe kind of not because bridge involves financial, you know, you know, lending or financial bridging. But it's kind of an Oracle too, because it's listening on chain and it's providing trusted, you know, results on chain. So, so, so maybe we need smart people to come up with a better name for this type of integration, this type of bridging off chain. But yes, I think using Bacl.io as a type of an Oracle is a use case
in itself. And I think all these sort of these, these middle layers between on-chain trustlessness

and off-chain trustlessness and increasing the trust and the closeness between those, you alleviate hacking issues and things like that are extremely important. Chainlink's doing a really good job of leading with their new decentralized functions to service capability, which, which I love. So I think we'll just see a lot more overlap in this space. And I think it's going to have to like, there's so much opportunity for it to grow, that it'll be more sophisticated and more like refined this time next year.
So the last question, can you please tell where developers should go if they want to build applications on Lilypad? Ian, they have a questions. Thank you for asking. Yes. A couple options. Please go to our Slack channel, Bacl.io Slack

channel. We also we also have the Lilypad GitHub repo, the Bacl.io GitHub repo. We have

issues they can raise there as well. We'd love to hear feedback, questions, ideas along the way. It's very much a community effort.

