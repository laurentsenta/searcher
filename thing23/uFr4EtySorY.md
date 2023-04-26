
# Boxo: Build Your Own IPFS Adventure - Gus Eggert

<https://youtube.com/watch?v=uFr4EtySorY>

![image for Boxo: Build Your Own IPFS Adventure - Gus Eggert](/thing23/uFr4EtySorY.jpg)

## Content

So yeah, some scenarios some of you might have run into if you're working in the Go ecosystem.

Imagine for example that you have an application and you want to embed a Kubo node in the application and like customize it a little bit for whatever the needs are for your application, right? You can just, you think you can just add Kubo as a library, right?
You're going to quickly run into this, your new dependency graph.
You're probably like squinting at it going like, what the hell is that thing?
You can zoom in a little bit on it. If you squint hard, you can see some boxes, some circles.

The boxes are Go modules and the circles are versions. So you can see like a lot of these have like multiple versions.
The Go compiler will resolve these down to like one version when you compile it. So like when Kubo compiles, these all get resolved down to like one circle inside of one box.
But as maintainers of these libraries, you have to know like, it's not just Kubo, right?

There's a lot of other people using these too, like Lotus and other applications. And then when they do their compilation, these may get resolved down to different versions. And so you need to know like if you're making a change to a repo, who's using it? And like, do I need to port it to different versions? Which ones? Like all this stuff. So that's just one scenario. I have a few of these. Imagine maybe some of you might not have to imagine very hard for this. There's a case where Kubo doesn't perform well for your use case. So you want to like reassemble some components or whatever to improve the performance for whatever use case it is. No problem, right? These are all like, we just saw all the different components around there. You can just rearrange them, right? Well, there's also no documentation for any of this. So you're just going to have to like, you know, browse around on GitHub for a couple days and try to figure out what all this is and how you're going to reassemble all of it. That's Jean-Luc laughing at you for trying to do this.
Another scenario, you've been using Kubo for a while. There's a bug somewhere in one of those repos that's used by like all the other repos.
This one's close to my heart because I've gone through this one. You just refactor it and open up a PR, right? Well, once again, you've got to open up PRs for all these repositories, figure out which

ones need backports to which versions, cut all the releases, and you can't even test any of it until you bubble it all up into Kubo where all the actual tests are.

So like, come on. Another example. If you're a maintainer, which is what I am, in the Go IPFS ecosystem, and you need to stay on top of security updates, user requests, pull requests, CI failures, and dependabot

things coming in, GitHub has stuff for one repo, but for lots of repos, no.

So we built a custom website to track PRs and issues across all these things.
We have our own CI frameworks for synchronizing CI across all of these.
Some of them aren't even migrated to that, so some of them run CircleCI, some of them run Travis, some of them run GitHub Actions, and then people are like, why can't I get the stewards to respond to my request?
So yeah. I'm sure a lot of us feel this way. So anyway, some historical notes on Kubo. As a lot of you know, it's old. It was written before Go modules even existed, before there was version management stuff for Go.

It used to be a mono-repo, and then it was split out into lots of repos, into GX packages.

Some of you may not know what GX is, but it's an IPFS package manager that was made a few years ago that has since been abandoned.
And Go recently has this thing called module graph pruning, where if you depend on a module, you don't necessarily depend on all of its dependencies. You only depend on the ones that you actually need. And lastly, Kubo is a binary first. It's a CLI. That's the product, not the libraries. And it shows. And over the years, Kubo has accumulated lots of ways to extend it and get custom functionality out of it. We've tried Go plugins, which is horrible. I haven't found anyone who likes those yet, because you have to get the compiler versions exactly the same for all of the different plugins that you have. A lot of people use Kubo plugins, which are just an interface that you implement. And usually they use it by implementing the interface and then recompiling Kubo.

It works. Not pretty. And then we have FX. So FX is a dependency injection framework that's used inside of Kubo. And so you can use that to swap out implementations of different components or add lifecycle hooks

to things. You can also use Kubo as a library. You can fork it. That's a fun one. There are some bespoke extension points, like delegated routing.
But Digg said earlier, piling everything into a single binary adds a lot of complexity.

Once you do this, it's a CLI, right? So you've got to have configuration for it. Every time you add something in there, there's a whole different abstraction layer for how you're going to model it in JSON. And then the output's not Go code. It's a binary. It's like byte code. And then the maintainer is us. We get swamped just trying to maintain all this stuff and make everybody happy. So as Digg said a second ago, only add things in your application you actually need.
So that's where Boxo comes in.

So Boxo is basically we're extracting the components from Kubo and from these different

repositories and consolidating them into a single library. Our goal is to help you folks write applications and implementations as quickly as you can without having to deal with all that stuff I showed you earlier. Part of this is going to be refactoring and simplifying abstractions, adding a bunch of documentation and examples, and having snapshots. Basically since it's all in one place, it's going to have a snapshot of versions that all work together, and you don't have to go through this dependency hell of figuring out which versions work with what. A non-goal of Boxo is to have everything in Boxo.
I don't want to misrepresent what Boxo is. We don't want to have everything in it. We want to focus on the things that are in Kubo for now, that have high leverage, and they're not very useful on their own. They're useful when they're used with other pieces. Hard to use correctly. Those are the types of things we're interested in putting into Boxo.
Sad part of Boxo is the transition period. So we're moving a lot of repos around in order to do this. So that means a lot of the repos, if you have existing code, they might need to be migrated
over to the new Boxo repository. So we have a tool to help you do that if you need to go through that. It basically just adds the right version of Boxo and then updates all your import paths automatically. It tries to find the repositories that have been migrated into Boxo and updates all the paths for you. The old repos that we're moving, we're not going to delete them or anything, but we're going to stop maintaining them. We're leaving them around so that it doesn't break everybody by them disappearing.

So the current state of Boxo, we've extracted Kubo's gateway code into Boxo so that you can run an independent HTTP gateway handler without having to use Kubo.
That's already being used for the IPFS.io gateway. We have some examples for common use cases. We've moved over 25 of those repositories that you just saw into Boxo.
We're starting to move all the tests from Kubo into Boxo. Like I just mentioned, we have that tool that will help you migrate. Coming up, there's more documentation and communication to do about Boxo, more testing

stuff to pull over, release automation, and then there's some feature work we have coming
up around gateways and bit swap and content routing.

You all from the community, if you're building a new application or an implementation, come talk to us in Go because early adopters of this, we want to give some white glove service
to them so that we can work out how we're going to do this.

We'll do some of the work for you if you come talk to us. We'd also love a bunch of feedback on how to make Boxo better and the Go IPFS community
in general. You can find us on GitHub, open an issue in Boxo. We have a Filecoin Slack channel. Once again, don't feel like just because you're building something you have to put it in Boxo. It's not the place for IPFS components. It's just our place. If you want to get started, this is the only repository you need to pull in, which will make Picard and the Go for happy.
That was awesome. Thank you. My question is, what does Boxo do out of the box?

That's a serious question. Right now, not much functionality-wise has changed. We're setting the stage for that because these large refactorings were very difficult to
do when there's 30 repositories. We kept running into situations where we wanted to change things and we're like, it's too expensive. We can't spend weeks doing this. We've already had a few PRs opened up with our cross-cutting changes. Now that we've moved these in here, it's much easier, you just rename function.
There's going to be a lot of changes that are enabled by this, but right now nothing

much has changed other than we've moved stuff into it and we've pulled stuff out of Kubo like the gateway. It's a serious question. Basically, if I take this and compile it, what can I do with it?

It has a lot of the... I'm trying to think of exactly what the list of... I can pull it up, actually. Let me see here. That's a great show, and I can actually repeat it. For the recording, Steve Biglep said, we want to focus on having more of an operator-class
gateway as one of the first goals using Bifrost. Amazing. Fantastic. Obviously, us at Vision, we're like, oh, yeah, we're all of the Picards, so super awesome

to see this. It's the exact kind of thing where, as an example, could we take the CarMirror plugin

and include that and then have a gateway with CarMirror included? That's kind of what you envision? Yeah. I envision you being able to build your own with these pieces, not having to figure out how to write a Kubo plugin or something for it. You just build your own binary from the library and from CarMirror.

That's the vision. I'll pull up this list of stuff we've migrated, because these are the things that it can currently do, but it's not new stuff. These are the things we've moved, like UnixFS and pinners and block stores and all these
things. IPNS, if you didn't know. I'll say it more nicely now. You're going to have the other repos, but you're not going to delete them, but they're

not going to be maintained. What do you think is going to be the best way to be able to direct people to Boxer, given that there's so much history of links and documentation and suggestions to these
other things? Yeah, so we're doing a lot to try to maintain the historical context of these. When we move the code over, we're keeping all the Git history. That's the first thing we're doing. We're going to move over all the issues. Pull requests are not easy to move over, so we're probably going to close them and ask if any open ones. We'll ask the person to reopen them in Boxo, or we'll do it for them.

The plan is to put big notice at the top of the repo when you go to it, like, hey, this
is unmaintained. If you want to pick it up, go for it. Otherwise, this has been moved over here. Then we're going to go through and put deprecated tags on the types so that your compilers will
start warning you. If you have linters or something that look at these, you'll get an error in your linter.
You can ignore it if you want, or you can go to the migration or whatever else you want

to do. That's the plan, is to use the compiler to notify people. The only other thing I'd ask is, have you considered making the old ones that are unmaintained
type aliases over to the Boxo version so that you end up with a single type between the

old one and the one that's been in Boxo? So that if someone's using something and ends up with weird package graph things, they don't end up with suddenly incompatible two different types. Yeah, we've done that with some of them already. By default, I don't know that we're going to do that, but we might do it for some more
of them. I think it depends, because it depends too on who's using the repo and whether or not they want us to do that. If they don't want us to do that, we're just going to leave it alone for them, and they can continue to use the repo on their own. All right. Thank you. Thank you, Patrick.

