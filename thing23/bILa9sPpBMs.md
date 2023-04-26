
# Automating Kubo's Development Process - @galargh

<https://youtube.com/watch?v=bILa9sPpBMs>

![image for Automating Kubo's Development Process - @galargh](/thing23/bILa9sPpBMs.jpg)

## Content

My name is Piotr, I work at Protocol Labs in the IPDX team, that stands for Interplanetary

Developer Experience, and our main goal is to improve developer experience within the

IP Stewards team, because we do need them in a lot of places and they don't have that
much time. So in this talk I'm going to talk a bit about how we're making it happen through automation.

I'm going to touch on how we automated Kubo release process, how that connects to our

migration from CircleCI to GitHub Actions, how we're making GitHub Actions work for us
so that it's fast enough and reliable enough and available, and finally how we're monitoring

all of that so that we know where to go next. So let's get started, first, I guess that doesn't need that much introduction, you probably
already are familiar with Kubo, but for the purpose of this talk I want to just mention that Kubo is a Go-based IPFS implementation, that it's maintained by IP Stewards and they

do have a great deal of knowledge that's needed in various places around our ecosystems, so

whatever we can do to free their time up for use in more important stuff than the burden

of going through a release process, for example, that has a tremendous impact.

And while we are on the release process, which I'm going to talk more about just for the

record, in Kubo it follows a monthly cadence, we do have RC releases before each final release,

there are some patch releases in between as well, sometimes there are more than one RC
per release, so as you can imagine, it all adds up and it's time from IP Stewards time.

So why do we want to automate it? First of all, with a process as complex as Kubo release processes, which doesn't really

entail only releasing Kubo binaries, but also touching on many different other projects

like upgrading Kubo in different areas or promoting the release via multiple channels,

it's really complex, so there is a pretty big potential for error and we do want to
reduce that. We also want to make it more reliable. As I mentioned before, our main goal is for IP Stewards to have a better experience,

but also to have more time for more important tasks.
We also want to make the process more scalable, because as you can imagine, if it's all manual

and with time, it's only getting more and more complex, and that doesn't scale very

well. So what were we starting with? We are starting from a process that was already over 150 manual steps.

When I first did it, I certainly did miss some steps and didn't realize only days or

weeks after, so the tree does have a huge room for error, which we do want to minimize.

It's really lengthy, and a day to go through a release and not being able to work on anything

else, that's way too much, especially given that a release is not a single event, there

are many stages of the release. It took way too much time from IP Stewards, and also it was only going to get worse with

time, because we are adding new stuff to it, we are making it more complex and longer.
So what did we come up with? The problem was that we do have to interact with many, many different services during
Kuber release, so we weren't able to use just any ready-made tool that already is out there

that deals with release automation. Instead, we had to come up with something that pulls all of those services that we need

to interact with into a single coherent package, and that's how Kuber Releaser came to be.

We decided to write it in Go, because we wanted Kuber engineers to have an easy entry to contributing

to the project. Kuber Releaser, what it really is, under the surface, it just calls APIs of different services

that are involved in the Kuber release, and makes sure that the steps that were previously manual are now automated.
It also changes the interface that the release driver has to interact with, because now all

the commands are grouped together into logical groups.

For example, you don't have to do 30 different actions to promote release on Discord, Discourse,

Matrix, and Reddit. Instead you just tell Kuber Releaser that now we are at the stage where we want to promote the release, please do all those things. It's way nicer experience for the person driving the release.
Some principles that we followed when developing it. We did make sure that everything happens in isolation, so there is minimal dependency
on release driver's system. It all runs inside the Docker container, it does all the calls and checkouts in there,
it has all the dependencies there, so you don't have to worry that the state of your

system is going to mess up with the release you want to make. So that's a really nice feature. It's also written in a way that makes sure that the reruns are safe, so we can run the same command over and over again, and it's not going to create a hundred releases for
you. Whenever it can't do something because it's not smart enough just yet, it will interact
with you and it will prompt you for help. It's also not tied to your machine, so we do support multiple people coming together

to drive the release from different machines, which is pretty nice to introduce someone

to the release process, for example. It also tries to provide very clear documentation, it tries to be very verbose, both in code
and in line. We do think that all the steps that come into the release process should be well documented,

because who knows where we might go with that yet. Maybe we'll want to get rid of kubereleaser at some point, so it will be nice to have
it all documented. But also when executing the commands, it tries to be very verbose about what it's doing,
so that kuboengineers driving the release don't lose the knowledge of how the release

is done, because we don't want to just lose this expertise for a while to realize later
that we might actually need it. So I think that's really important, and it enables, for example, kuboengineers to contribute
further to how kubereleaser works.
So the end product of what we have is a completely new release process that instead of over 150

steps has only 15, so that's a pretty nice reduction in just the number of things you

have to do. All those commands, as I mentioned, are really logical groupings of things that happen underneath,

so we are much better suited to deal with further-grown complexity, because usually

when we want to add something new to the release, it's something to do with those commands we
already have. If you want to, for example, promote a release in yet another channel, it's not really a new command for someone to run, it's just a new thing for the command to do.
So it doesn't make it harder for the release driver to go through the release process.

It drives the duration of the release process down a lot.

The last time I did the release using kubereleaser, it took me way less than two hours, and I

was already one foot out the door, traveling home for holidays.

I didn't encounter any troubles, so I think that speaks to the improvement to where we

started with. And visually, it looks something like that. So the thing you see on the left is where we began, and the thing on the right is where

we are right now. It doesn't have to be the end of the road, but even just by looking at that, you can clearly see that the improvement is staggering. The list on the right, you can probably even just look at it and have a vague idea what

it's going to take to do a release.
You can build up context for it. If you look at the things on the left for the first time, you just get scared and want

to run away. But, leaving the release process in the back for a bit, I want to move on to our migration

to GitHub Actions from CircleCI. So it is a bit connected, because when we started working on automation of the release

process, we realized that it's going to be a lot simpler if we can interact mainly with

GitHub API to complete the release steps.

So one less service to integrate. So that was great. That's how it connects to the previous topic. But that isn't really the main reason why we set out to migrate from CircleCI to GitHub

Actions. In my view, the biggest benefit of using GitHub Actions over CircleCI in our case was that

the experience of using GitHub Actions is much more integrated into where our developers

already are, which is GitHub. That's where they spend most of the time. So it's a nice feature that they don't have to leave the platform to, for example, inspect CI failures. It really makes it much more natural to interact with all the parts of the CI system.

And for me, that's the biggest selling point that there is when it comes to GitHub Actions.

But as I mentioned, it did also help us simplify what Kubernetes Releaser became.

It is easier to manage, but that's a personal preference.

And also, we do have more expertise with GitHub Actions, so it's easier from the perspective

of our team to deal with. And as we found out, it was much more cost-effective for us, because most of the things we did

in CircleCI in GitHub Actions we could achieve on three runners.

So how did we do that? That process was mainly manual for us. We did go workflow by workflow, read what it was doing, and tried to translate it to

what GitHub Actions can understand. Right now, it doesn't have to be, because GitHub came out with a GitHub Actions Migrator
tool. So if you want to do the same, it's probably going to be a lot easier. Though even doing it manually wasn't that bad, because the concepts between the two

aren't that different, so it was pretty straightforward. But we really wanted to ensure that the well-tested workflows that we had running in CircleCI

for years are behaving at least the same way in GitHub Actions, in terms of both correctness

and performance. So we ran both CI systems, both CircleCI and GitHub Actions, in tandem, next to each other,
for around two months. We gathered some stats that we heard about, so that we could compare them exhaustively

and be able to back our claims with data that, in fact, GitHub Actions is a better choice

for us. So the benefits we expected were faster builds and reduced wait times, because we wouldn't

wait. Because, as we found out, there was quite a bit of upfront costs to waiting for CircleCI

runners. As I already mentioned, we did find out that we did save money with this migration.

What also happened was that suddenly our workflows were a bit more stable and reliable.

So we weren't able to pinpoint the exact source for that improvement, because the configuration

of the hostage runners in CircleCI and GitHub Actions, to some extent, you can't know everything

about it. But from practice, as it turned out, GitHub Actions runners were more reliable.

And the metrics we looked at to compare the two were the duration of our workflows, the
success rate of them, and price that we had to pay.
So by the end of this experiment, that's pretty much what we were presented with.

This is an extract from a week-long monitoring of the two.
So as we can see, GitHub Actions did outperform CircleCI pretty much everywhere in our case,

especially the pricing column. That's a big difference. There are two instances where CircleCI is seemingly a bit better than GitHub Actions,

but if you look closely at the numbers, they're pretty much on par. So that's really nothing we worried about, and we were confident enough to shut down

CircleCI after seeing those numbers. But if you also look at the column that presents the cost of the two solutions, one interesting

thing that you'll notice is that we are not only using free solutions from GitHub Actions,

and we are on a free plan, so we don't have access to pay large runners in GitHub, but

we did need faster runners. And that's why we set up self-hosted runners. And that's exactly to get faster builds, because we needed more powerful machines, and we've

self-hosted runners, we could get them, and that would speed up the builds.
But also it would reduce the time our workflows had to wait for an available machine, because

you can use up to 20 GitHub-hosted runners per organization at any given time.

So with us promoting GitHub Actions internally everywhere, we started seeing that sometimes

we do run out of GitHub-hosted machines, so self-hosted runners is also a great fit for

that. It can provide better availability, also because of this reason, we can control how many machines

we have within our fleet of self-hosted runners, so we can make them available readily to be

picked up. We could customize them, though we don't do much of that, because we do believe that it's
a nice feature to have workflows that can work both on self-hosted and hosted runners.

For example, in a scenario where you fork your repository, it would be nice if you could run it on GitHub Actions' hosted runner, because you wouldn't have access to our self-hosted

runner fleet. And also, not really a reason why, but I think it's worth mentioning here that we do run
our self-hosted runners in ephemeral mode, so they only ever do one job and then just

disappear, and that's for multiple reasons. It does make us sleep better at night, because that way we know that a malicious job can't

just leave something on the runner that would mess with subsequent jobs.
Also, it doesn't have to be malicious, it can't leave some trash behind you to break

future jobs, so that's a really nice feature, too.
And how we set them up, we used a really brilliant project developed by Philips Labs.

They use it internally, it's called Terraform AWS GitHub Runner.
It has great documentation, it's a really nice piece of software that pretty much covers

everything that we needed to do to set up self-hosted runners on AWS.

We did modify it a bit in a few areas, because for once, back when we started working on

self-hosted runners, the project from Philips Labs didn't support starting up runners from

different instance families, and we did want to have that level of customization, that

we could have machines of different sizes for different needs. Now I think it does, but I haven't checked it out in detail just yet. And also, we wanted to have more control over what requests for self-hosted runners.

﷕But we didn't have enough self-hosted Runners we accept, so we wanted additional filtering level that checks whether the request is coming from the repository that we allow to have self-hosted Runners from our fleet.

When it comes to configuration, there are some obvious things we do care about and we do look at to decide on what kind of instance to assign to a specific workflow.

CPU, memory, network, bandwidth, and probably the most important part, the disk speed, the IOPS and throughput on the disk. That can really change a lot when it comes to building code.

To give you some stats, this is the number of self-hosted Runners that we brought up in the last 30 days only for workflows in Kubo.

We had almost 1000 runs on self-hosted Runners and we only had one issue with it. It's running pretty smoothly and the issue was that GitHub itself was having a bit of trouble and it forgot to send us a webhook.

So we didn't bring up the machine that we needed. But it's a shame that we had to wait for someone to report it to us.

We didn't ask us as IPDX, we didn't know about it until we heard about it from our developers and that doesn't seem like an acceptable solution in the long run.

So we started thinking more about getting more insights into what's going on in our GitHub Actions and that's why we developed yet another thing of our own, which is monitoring GitHub Actions.

Because unfortunately, GitHub lacks a bit in this area. It doesn't give you a great deal of insights into what's going on in GitHub Actions. We had to go and do it ourselves.

So we did it. We created a GitHub app, we configured it to receive webhooks on the events related to GitHub Actions, but that's really a detail. It could receive any events like the database standard beyond just monitoring GitHub Actions events.

So we have a service running that listens for those webhooks. Then when we receive an event, we just store it as is in the raw format in PostgreSQL database because, to be quite frank, when we started doing that, we had no idea what exactly we wanted to look for in this data.

So we thought, oh, yeah, let's just store our data and figure it out later. And as it turns out, that was a great decision. And then in Grafana, we just query this data directly and build up graphs that tell us many different interesting things about the posture of our GitHub Actions solution.

They help us debug issues with CICD, it helps us make decisions about where to focus our efforts next, and that's a real superpower because there's only two of us in the team.

So to have this extra piece of information that tells us which project may need us the most at any given time, that's amazing. And it also helps us optimize our current workflows.

So here's how it all comes together. Some of the things that we look at right now for our monitoring solution are, for example, durations of workflows and jobs per day.

We look at how long they wait in queue for a runner to be assigned to a workflow. Even here in the description I took a couple of days ago, you can see that something weird is going on with the sharness workflow in the Kuber repository.

It's suddenly spiked up to take around 20 minutes per job, and that is a bit worrying, especially if you also notice extra bit of information that sharness workflow in Kuber has a timeout set for 20 minutes.

So if we were to scroll down here, we would probably see a dramatic drop in the success rate of sharness workflow there. So as you can imagine, that really gives us a lot of insight, and it gives us information that otherwise we would have to rely on user reports to find some of this stuff out.

So overall, what did we learn through all of these projects and more? One important point that I wanted to make is that I believe that it is really useful not to hide expertise from developers.

So for example, with the release process, I think that was a right decision and it is going to pay off in the long run that we did spend so much time to make sure that while using the new automation tool, the engineers won't lose the knowledge of what's going on.

So that's one. When we also were developing Kuberleaser, I got to use the Go Git library that I really liked, so I wanted to give it a shoutout.

It's surprisingly complete, so there is I think one operation that I needed to do during the Kuberleaser process that it didn't support, but for any other needs, it was there. So it's really great.

It was a breeze working with Matrix API. It's so much nicer to get started than, for example, Slack or Discord.

When it comes to self-hosted runners, we did run into a few roadblocks for a second.

So apparently some third-party services don't always like too many requests coming in from behind a single NAT gateway, but it's nothing that's a single caching proxy in between configs, so that was nice.

It was especially troubling for Docker Hub because its rate limits are quite small, so to deal with that in our self-hosted runners infrastructure, we just set up a proxy that's now between our runners and Docker Hub

that basically acts as a read-through cache and stores Docker image layers in S3.

What else? Oh yeah, collecting raw data for authentices. That was a really smart choice, and I would recommend that if you're not yet sure what you're looking for.

And also, basing your decision on this data, that's also something I would definitely recommend.

Some things in the future for us that we would like to explore.

Fully automating the release process of Kubo, because now we do have pretty much all of it scripted, so we're just one step away from closing the gap to full automation.

We would like to implement alerts from our dashboards that monitor GitHub actions, because right now it's still a manual process to look through the graphs and try to spot potential things that went wrong with GitHub actions,

but I guess that will come with time as we learn more patterns of what the graded state of GitHub actions for us looks like.

We also want to get better at distinguishing between what's running on self-hosted and what's running on hosted runners within our monitoring solution,

because right now it's all tangled together, so just by looking at some graphs it's hard to tell whether it's GitHub having trouble or whether it's something on our side.

We don't want to open source our dashboards for monitoring GitHub actions.

There's nothing really stopping us from doing that other than time.
We just have to export JSONs and put them in a publicly available space, but it's more of a to-do for myself.

And lastly, we do want to enhance monitoring self-hosted instances in particular, because we would like to be able to fine-tune the usage of our self-hosted runners more.

We would like to be able to tell whether some resources on machines that we use are underutilized or overutilized and adjust accordingly.

We don't have insight to that yet, but we do have all the data available, so hopefully that's coming sometime soon.

And that's it from me. Thank you.
