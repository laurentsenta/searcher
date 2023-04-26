
# Starmaps: a cross-team roadmapping tool - Bastien Dehaynin

<https://youtube.com/watch?v=_HoLDQreF28>

![image for Starmaps: a cross-team roadmapping tool - Bastien Dehaynin](/thing23/_HoLDQreF28.jpg)

## Content

Hello everyone, I'm Bastien, I'm working as a Program Manager at Fission and today I'm

going to talk to you about StarMaps. So it's a roadmapping tool developed by our protocol labs. So first I'd like to talk about the benefits of having a roadmap for your project.
So of course it's going to help you clarify your goals and objectives for your project
and it's kind of important because most of the time the people in production really don't
like the high-level vision of the project because they're so focused on their work.

And so it's going to help you communicate your objectives and collaborate inside the

team because you're going to engage your developers more and obviously you're going to increase

your visibility towards external audience. And since we are in a network we have a lot of interdependency with each other so it's very important in my opinion to have a roadmap to communicate where you are currently in your project. And so doing this work of roadmapping your project is going to force you to set timelines

by estimating your work from low level to high level. You're going to define your milestones more precisely because you're going to decide how

the quantity of work that you're going to put in there, the features and your objectives
for each of these. And we also have to look at the dependencies that you have with other projects. So again doing so will allow you to have much more information when you're looking at your

project. So that's going to help you to make better decisions when you will need to change the
way you're, like the direction you're going if you have to at some point. And you're going to be able to identify roadblocks much earlier into the process of working on

your projects. So that's going to save you time and resources for sure. So yeah, so now you must really want to have a roadmap. Let's look at StarMap. So it was developed and still being developed by Protocol Labs with the specific needs of

the network in mind. It's a very flexible tool that can be used to render all kinds of roadmap because we

in the network we have so many different team sizes and the kind of projects we have people
that are still doing research. We have people that are in very different production cycles. And it all renders from GitHub issues. So that's very handy because you don't have to duplicate your tasks to just put them in a roadmap outside of the GitHub cycle. So with that said, I'm just going to show what we have currently at Fission in terms

of roadmap. So this is the roadmap for the project at Fission. So for now we have three, but we are currently working to adding more of these.

So this is the standard view for StarMaps. You have a timeline. You can see all of the project issues are displayed on the timeline based on their ETA,
which you can see at the bottom left of each card.

I'm going to just click on the IPVM project issue to show you what's inside. So we have all of the milestones for IPVM here for Q1 2033 to future developments.

If I click on the right here on the date, you can see where we are currently. So that gives you a nice view of where we are currently on the project.

We have three view modes in StarMaps.
Currently we are using the overview one, but if we go in the detailed view, it's going to display all of the child issues for each of these milestones in the StarMap.

So there you are. So here you have the first milestone and all of the low-level issues that are inside. If I scroll down, you can see the same for all of the other ones. You can note that some tasks have a progress bar, but when they are completed, they are

filled in green. And that's reflected as well in the milestones.

You can see here this one is partially filled. Well, we are not quite there yet, but it's because we closed the first one in advance.

But yeah, so if I click into this milestone, you can see basically what we just saw in

the detailed view. And these issues are all stacked on top of each other because they all share the same
ETA, which is the same as the milestone they are in, but you can customize it if you want, if you'd like to order your task in a specific way you want to work on them.

And yeah, so that's for the basic view of StarMaps. If I go back to my Fission project node, so here I can view the issue in GitHub.

So I'm going to show you how it is formatted inside GitHub.
Right, so for the Fission project GitHub issue, so this is a container.
It doesn't have any work to be done. This is really made to contain all of the projects that we have at Fission, and the same goes for the milestone issues. So all you need to do to have your issues rendered in StarMap is to add a children list.

So basically a blood point list with the URLs of your issues to be done, and an ETA, and

that's it. So if I click here on the IPVM project issue, we're going to see the milestones, right?

So the children of the project are the milestones. Again we have an ETA here. I've added some resources for people who are consulting the project to find.
And if I go again lower one level, right, so we have the objectives, the features that we want to bring with this milestone, so that, you know, an effort of communication towards
the people who are interested in the project and want to learn more about it. Again we have the ETA, and here we have all of the children that need to be done to complete the milestone. So StarMap supports a GitHub task list, so that's actually pretty handy because when all of your GitHub issues share the same ETA, we can just reorder them with a drag and drop,

and it's going to be reflected in StarMap as well. So I just lowered the metering. If I refresh StarMap, it's going to be lowered as well in the list. So you can prioritize your tasks this way pretty easily. And yeah, it's very easy to add dependencies because, again, you just have to grab the

URL of a GitHub issue and add it in the description, like here in this list, to show the dependency.

So if I'm waiting for a tech to be released from another team, you know, complete other

projects, I can just grab the URL of their tasks on myself, and I can just put it in

here, and I can create as many levels of deepness as I want.
Like currently at Vision, we are only using project, milestone, and GitHub issues for
lower level. But if I want to use epics, user stories, any kind of container, I can.
I can just create a new GitHub issue, label it the way I want, and I just throw URLs in
there, and I'm good. So just to show you a bit what you can do with that, so let's say we have the Protocol

Labs network as a root node that's going to encapsulate all of the Protocol Labs network
projects. So we have company one that has two projects. Project A is working with epics and user stories. Each user story has several issues, and project B is only using milestones and GitHub issues.
It really depends on the way you want to organize yourself. But you can create dependencies any way you want to, from milestone to epic, from epics
to milestone, issue to issue. And you can do the same with completely other projects from other repos.

Just to separate and avoid to put noise in repos, I've created a specific repo for the

Vision star map for all tasks that are used as containers. So here we have the project issues and milestone issues. Right now we have 26 issues open, so we put them in there to avoid polluting the other

repos. And yeah, that's pretty much it for the star map tour.
Now if you're interested in building your own star map, you can go to starmap.site.

They have a guide over there and templates for you to use for your star map.
There's also repos for the star map software, and we have our own repo for star map at Vision.

So feel free to look at these for reference. If you need any help, don't hesitate to poke the team working on this on the file on Slack.
They have a channel for this. They are very helpful. They'll be happy to help you with any issue you may have. And I will as well, so you can find me on Slack or the Vision Discord if you need any
help. And so the QR code is for the Protocol Labs Network star map that I created, hoping that
someday we'll have many, many projects from the network in there. Just to show you how it's looking right now, we have two companies' projects in there,

so maybe someday dozens of these. I hope you'll as well. Okay, that's it for star maps. Thank you. Any questions? Hi. Thanks for showing star map.

That's something I help maintain. I'm really interested what features you are looking in, what features you would like to

see added the most first, like additional features or support. I know there's some issues we need to address. I've been struggling with ETAs because I figured that we don't necessarily need an ETA for

every issue because sometimes you have containers that don't need an ETA and you will still

have to put one in there. So the issue is how you will render it in the timeline if it doesn't have an ETA.

But yes, sometimes you just want to put placeholders somewhere, but it's going to give you an error instead. So yeah, there's that. But I had a whole bunch of features that I wanted to request regarding star map, but

I can definitely talk to you about it. But we can chat about it right after.

I'm curious what you're finding this the most useful for. What are maybe the three things that you do with your star maps on a weekly or monthly

basis, and whether it's mostly within team or with stakeholders or with other groups

that you're using it? Because I feel like it's a new useful tool. It was created because we couldn't find anything else open source that did the things that we felt like we needed. But I think we'd love to see it have a wider community of maintainers and filling that

niche for other groups that want an open source planning tool, not having to put everything
inside of some silo that only works within their organization. I'm curious, what are the common use cases that we can then maybe share with others and
build up more of an open source community around maintaining this? Right now we've been mostly using it internally to communicate how we are doing on our projects,

especially IPVM, but also new ones like GoCamera that we've been recently planning.

So it's been very useful in that regard. But also when you have dependencies with other teams, it's very easy to send them just the link of your star map and then they can see, okay, so you need this from me.
That's cool. I'm going to focus on that and help you there. So that's most of our uses with that at the moment. Makes total sense. I will also answer it. So Bastien is our first technical program manager at Vision. And I said, you need to be an ecosystem level TPM. Star Maps is brand new. We're going to help evangelize it because we think it's going to help us long term. We need to do roadmaps in some format. Let's do it this way. We've noticed that there are some teams who are putting protocols that we work on in their

backlogs, not yet star map enabled. And so the next thing we'd like is generally more community evangelism and bootstrapping of using it by default. And then we're trying to leave, we're like, what is the right method of like, I'm like, okay, Bastien, I saw WinFS mentioned on this slide, go track down the team and figure out

if they've committed engineering resources to doing it so that we can put it on the overall
map. So I think we're in this boot up phase where I'd really like us all to just do that.
And then ideally it's a very useful tool where we can then work back and be like, oh, there's three teams pointing at needing to do X. Oh, that could be a small library or something else that like all three teams work on together.
So that's our like, hopes start network wide. Like, and it's a super useful tool. Right. I think like we've, we've found more like there isn't anything else like this. We run a bunch of orgs. We're not in just one GitHub org. So it solves problems for us already, which is a really great starting point. So thank you for, thank you for building it. Yeah. I have my own whole list of feature requests. I feel like we should start tracking them somewhere so that whoever feels strongly about them can a help prioritize, be help solve. But as someone who uses it across many different teams in many different orgs and you want
every group to be responsible for their own milestones, but then you want them all to roll up into one view where you can then take a screenshot of it and like present it of like, Hey, this is what everyone is doing. That's what I found it most useful for personally. It might be a really interesting starting point for even like turning the crank even more of about a few other things. Like, you know, do we create an open collective for star maps? I'm like, great. I'll throw 500 bucks a month into it. And then we can fund independence doing a backlog of tasks along it or something like

that. Right. I love that. I think that would take part of those 500 bucks and are random humans who would just start building stuff.

I'm curious because I would assume some people might ask this question. Like how did you find using star maps compared to some of GitHub's more recent roadmapping projects, roadmapping tools? I think they don't have the same use case. Like it, and projects is really more to track your issues on a daily basis.
You, the milestones don't work the same way as well. It's also, it's not made to reflect several projects at a time.
I mean, you can do that, but it's not optimal, at least not from a roadmap perspective.

I feel like it's much easier to flag dependencies using, using star maps in general.

And it also makes more sense in the, in the, in the, like the general layout of the tool.

So yeah. And in a way you can also create so many different layers for your roadmap.

Like I mentioned, you can create any kind of container and you can't really do that in, in the GitHub projects. Like you really have to walk in a waterfall. It's like a Kanban. It's a, it's like a Trello, but in, but in GitHub, whereas here you are really creating,
you can create a peaks and user stories and really child issues if you, as much as you
want. So it's not really the same use case in my opinion. So I think it makes more sense to use a star map for, for roadmaps. Yeah. Cool. Thanks. Maybe just one quick follow up. Maybe this is Molly and Boris. Like what is the, how do we get going on something like an open collective to drive some more I'm not familiar with how we, how we do that. We haven't done it before, but you're in literally the right track for it.
Join the working group, working group. I'll put it in the hack MD. And I think even star mats itself being a tool that working groups are familiar with,
right? Like it's another, a kit of tools, right? Yeah. I mean, we should have stomped for specs. Right. You want to say, Oh, I think Steve just asked it. It was like, yeah, for a team that's coming at it from a cold start, what resources do we have? And do you need a Guinea pig to help document the process?
Yeah. Well, you have the whole guide on star map that site that explains everything, like how to read your star map from, from scratch. All you need to do is do the road mapping process for yourself.
So you, you look at, you look at your spec, you look at the features you want to produce
in which order you, how you, you set your timelines and you, you create your GitHub issues based on, based on that. And you just have to format them to be rendered in there. And after that, once you have your root node for your star map, which is basically like

the content container for all of your projects in your team, you just copy and paste the
URL, sorry, the URL in star maps, and it's going to be rendered automatically from there.

The site is like no BS. It starts straight into like the docs of how to get started. So that's like. I have a, just given that we are in the community governance track, I'm curious what we think

about creating like project level star maps or things that aggregate over top of various

different working groups or highlight major like milestones for the IPFS project as a

whole, which then, you know, should be looking across the fission star maps and the IP steward
star maps and the IRO star maps if they have one and the content routing privacy working

group star maps and, you know, each one of those has their own star map, ideally, which,
you know.

focus is on the contributions that are being made by that group, but then having something at a broader level that's able to surface the top level, most important milestones that everyone can celebrate. I remember from my days doing project leadership stuff in IPFS that there's always that yearly request of like, what are the like 10 most important things that are happening this year that I should really be excited about and maybe help work on? And so I'm curious if there's interest, challenges,

like, you know, what would need to be true in order to make that possible? I think one huge part of it ends up being how much of is a roadmap that has committed

engineering resources? How do we surface? We would, how do we surface a signal that
says we would really like X, we absolutely can't build it. And that might be technical

skill, engineering bandwidth, or funding. So there's a couple of other things that currently

isn't really mapped in here, but somewhere along the way we could use similar techniques,

right? So as an example, GitHub repos have a funding.yaml. And so that would actually

surface if a particular code repo had an attached way to pay it already. And that could be everything

from an open collective to a Patreon to a Filecoin address. So those might be some other

things. I think I'd love to see, like, I think the missing thing here actually is the hopes,

wishes, and dreams that are like a superset of engineering resources, right? Like what
happens before you write the roadmap almost? Maybe?

Yeah I think also surfacing visibility to people who don't have IPFS as their day job
but are interested. I know, like, I remember being on the other side of that was like,
hey, this is super interesting what's going on and feels like you're opening a cupboard and like the entire universe is falling on you. And so having a way of summarizing things
at a high level for a broader community of interested people I think is something that
would be extremely valuable.

I think all of that feeds really neatly into the opening conversation that you two kicked

off this morning. And a lot of those questions would be answered really neatly by a meta
working group for the IPFS project.

We should have done that on the slide. All right. Thank you.

