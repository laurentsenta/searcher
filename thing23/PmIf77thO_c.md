
# Testing Your IPFS Gateway Implementation: A Step-by-Step Guide - @galargh

<https://youtube.com/watch?v=PmIf77thO_c>

![image for Testing Your IPFS Gateway Implementation: A Step-by-Step Guide - @galargh](/thing23/PmIf77thO_c.jpg)

## Content

Okay, so my name is Peter, I work at Protocol Apps in the IPDX team, that stands for Interplanetary

Developer Experience, and our main goal is to improve developer experience within IPsecure's

team. So you might wonder what am I doing here? Oh, closer. I thought I was being a bit too loud. So that's the other way around. All right, so one thing that we noticed is that recently a lot more HTTP gateway implementation

started springing up, and they started taking quite a lot of time from IP stewards to actually

help with their development. So we saw a place for us to help with that, and that was the testing space.

So first we had to answer the question, what are HTTP gateways? And luckily, they are quite well specced out in the IPFS spec, so that was really helpful.

They are just the HTTP access point to IPFS network, and that was good enough of an explanation

for us to get started. And the things that are currently part of the specification, and things like what does

it mean to be a path or a trustless gateway? What does it mean to implement a DNS link or subdomain features or redirects files?

So ideally, we'll have a way to test all of those and even more once we expand the spec.

So the question that we put in front of ourselves is how to ensure that my implementation confirms

to the specification that we have written down. For the time being, the answer was, well, you're on your own. You have to implement your own tests. And that's what happened in Kuba, for example. We did have quite an extensive test suite, but it was impossible to reuse it in any other
place. When we had a new implementation coming up, they would have to do this exact same exercise
from scratch. And that seemed wasteful because we already did this work. And in Kuba, we already found plenty of edge cases that might not think of the first time

they get to writing the test. But as we thought more and more about it, we realized that that's not really the only
question that we like answering, because it might be also useful to make sure that your

implementation of the gateway confirms to the specification. Me as a user, I might be interested in actually confirming it myself, whether what you're
telling me is true. So that's another thing that we do keep in mind when working on the testing tool for

the gateways. And that's how the gateway conformance testing tool came to be.
And we set one and only one goal for it, which is to ensure compliance with the specification.

Yay! So we knew what we wanted to do, and that was only the easy part of how to do it.
Luckily, we are dealing with HTTP gateway, so we have some expertise in how to make HTTP

requests and verify the responses. So we thought that's something we can implement.
So without further ado, let me just show you how a test in our testing tool looks like.

First of all, the testing tool is written in Go. However, we do expect contributions from people coming from many different language ecosystems.

So when we are writing this tool, we try to keep that in mind and make sure that the way

we expect people to write the tests in the tool isn't really that language specific.

We wanted it to be readable to anyone familiar with programming.
So let's maybe analyze this one particular test case that takes car support in gateways.

So first of all, we do have static fixtures in our testing tool.
So that means that we can have a car file, which is our fixture that defines all the

data that we will be requesting from the gateway. And as long as your gateway provides this data, the test should pass.

What's more, we can reuse this fixture when we write our test case.
So for example, here, when you look a bit lower, like 22 data, we are not really hardcoding

CIDs anymore. We are just reading the same car file that should be available in your gateway and extracting

root CID in this case. Next part, you just define a name for the test case. So that's pretty straightforward, but that's not all of the repository that you get here. You can even write more verbose hints associated with each specific test case.

Also, not only test cases, you can assign hints to any check that you might make for

the response, and that's really helpful for debugging. So when something goes wrong, you not only get the name of the test that failed, but also what it was trying to do, a bit more context on that, and also context on the specific
check. For example, here, why are we even expecting the specific content type or specific content

length? And that is really useful when debugging test failures.

So then we come to the meat of the tests. You have to define what kind of requests to the gateway you want to make, so in this part,
you just specify anything you might need to specify for an HTTP request.

And finally, you have multiple response validators. So here we are validating the status codes and some headers.
And our goal was to make it sort of read close to how natural language does, so you're making

a request for a path where at the root CID of our static fixture, we are requesting sub
dir and ASCII text. We make a request, we then accept header for application vnd.ipod.car, and we expect it

to come back to us with 200 status codes and a content type header and an empty contents

length header. So that was the goal, that's how it looks like. And now I'm going to show you how to run it, because the test case itself is not that interesting

if we can't run it. So what do we do first? First we have to start our gateway. In this case, I'm going to start a Kubo gateway in the offline mode.

It comes up pretty quickly when it's just a screenshot. And it also told us that it's already listening, or it also started a gateway at port 8080.

That's a really useful information because that's what we are going to need for our test

tool to know where to make those requests. So then, since we're running Kubo gateway in offline mode, it won't be able to get fixtures

from anywhere on the network. We do actually have to explicitly load the fixtures into the offline gateway.

So we are just using the command that Kubo provides, but it might be different for different
gateway implementations. So that worked as well. And finally, we're all set to run the test case.

The gateway-conformance-testing tool is a CLI tool that comes under the name of gateway-conformance.

One of the commands it provides is test. Then you just have to provide a gateway URL of the gateway that you want to test.
And here in particular, we're also providing some additional arguments because we only want to run a single test rather than all of them. Some other arguments that you could provide is specific features that you might want to

test. For example, if you were only interested in testing the subdomain gateway part of the

spec that's supported by the CLI tool, you could provide it with an argument that says

only test the subdomain gateway spec. But here we're just running one test. So let's see what happens. And let's open the results in VS Code.
And it didn't quite work, unfortunately. So some things did, but not all of them. When we look a little bit more closely at the results, we see that the check for the
content type header did work. So that's cool. However, unfortunately, when we are checking the content length header, it blew up.
So we get a detailed report of what happened. First, we get the name of the test within which the failure happened, then the more

insightful description of the contents of the test. And finally, the core of the issue, which is the error that we encountered. So here it says that our content length header check was expecting an empty value, but in

fact it got the actual content length. But here we get a helpful tip that we were expecting the response to be streamed, so we weren't expecting an exact length in the response. So what was going on there? So I can tell you that the test itself is correct. The spec does make sense. And Kubo is implemented as it should really, but as we found out, the HTTP package in the

code language library always sets content length header. So we did trace that with GoFolks, but we didn't hear back about it yet.

But that's how an example of an error looks like in our testing tool.

But I wouldn't be myself if I didn't also tell you how to automate this and not have
to run it by hand. So let's get straight into that and analyze the workflow definition for GitHub Action

that would do that for you on, for example, every PR or every push.
And it's pretty straightforward. Like, we do provide a GitHub Action that lets you download the picture, so all the car files that you might want to import to your gateway if you want to run in an offline mode that
don't get it from the network. Then there is the part of starting the gateway, so that's up to each implementation. Like, we can't really impose the way in which you should be bringing your gateway up.

So in this case, we're also starting a Kubo gateway.
Then importing fixtures. So that's also gateway-specific. And finally, we're running tests. So we do provide another GitHub Actions that's responsible for running the tests.
It takes, again, just the gateway URL as the argument, but also a couple of fine names

for reports it can create. So it can create report JSON, XML, HTML, and Markdown reports. So yeah, whatever you prefer, there is a report for that.

They have slightly different formats. All may be useful, as we're going to look in a second into two. But it also takes additional arguments. So here, for example, in case of Kubo, we are skipping this particular test case, the

context-length header check in the gateway car test, because we know there is no way

it's going to work, but we would like all the other testing to happen. So we can request that from our testing tool. And finally, we just share the results by uploading them to artifacts and setting the
summary for the job. So the summary looks a little bit like that for a success case. It just creates a summary of how many tests run, how many were skipped, and that's all.
If there were failures, it would show inline reports for those failures.

So you wouldn't even have to download anything. You could just inspect it directly there in the job, which is quite nice. But if you want a bit more details, we also the HTML reports might be your thing, because

here you can click through everything, get more specific data on what kind of outputs

each test case produced, how long it took, when it started, when it finished, what exactly
was run. So where are we right now? We are already running this testing tool in three projects. We are testing Kubo gateway. We are testing car gateway that lives in the Boxer repository. And we're also running this on PRs in Bifrost gateway.
So for a project that's quite young, that's pretty cool that we're already there and already

helping developers. We purchased around 30% of the tests that currently live in Kubo to the new testing

tool, but we aim to get to 100% at some point.
We don't have a date set, but we're making quite good progress on these fronts.

So what's next for us apart from getting to migrating 100% of the tests from Kubo to the

new testing tool? We'd also like to make sure that we do cover the entire spec. We haven't really written that out yet, and so can't tell you exactly what it entails

and how long it's going to take, but that is our overarching goal.

We think it would also be amazing if we could integrate more closely with the spec contribution
process. So think of like when you propose a new spec, it will be great if at the same time you could

write tests for it, maybe some small implementation of them. That would give everyone more context about what the change is really about. Does it actually work? Is it feasible to implement? So that's something we do have in mind. And a little bit closer to today, we are also running a Gateway Conformance Testing Workshop

on Wednesday. It's mainly targeted at Kubo maintainers, but if you're interested in testing out the tool and seeing how implementing new tests works, we really want feedback.

So please come. It's going to be, we won't give you that much instructions in this workshop because we want

to see how developers react to being presented with it, but we're going to be there to gather

feedback and help you along the way. So it should be fun. And also one more thing that me and Russell got to talking, we think it would be really cool if we could adapt it somehow to be able to test also the Service Worker Gateway, but

we're not there just yet. And that's it. Here is the repo. It's ipfs.gateway.conformance. Thank you.

So how many bugs have you found? One in go, so that's one. Definitely a couple already. Laura would be better suited to answer that because he was closer to actually to the work

of migrating the test from Kubo to our new testing tool.
We definitely seen at least a dozen of cases where we're in Kubo tests.

We are actually copy pasting the same tests and changing the description of the test, but not really what it was doing. So that's another area of being tested that we already discovered.

But even for that, it's been beneficial to go through this work and have a more structured

way to write the test than through Bash and Sharness.
Cool. And is there thoughts on adding randomness to the tests as well?
Adding randomness or generating random test cases? Not yet. We didn't get it. So right now we do focus on trying to make sure that we don't lose any of the expertise
that we built up in Kubo testing suite and we want to preserve it in the new testing.

So when it comes to development or new novel ideas for testing, we'll get to that after.

I know Laurent is not here, but I had a conversation about this with him. So maybe like channeling Laurent is that he has a, I think he has a proof of concept where

he's testing false positives by mutating a percentage of responses, introducing random

bytes to headers or bodies just to make sure we don't have tests which always pass.

And that's been the bane of existence in Kubo because for a while we had, oh, we have everything

passing, but that someone forgot to add like a double ampersand somewhere in the Bash.

And it was always passing. So the killer feature of this test suite is that we will know when it's actually, where's

this regression? It will be like self-testing itself. And I have like an actual question for myself. But before you begin, I just want to ask that, I did forget about that, but it's really clever
that what Laurent came up with, like basically generates like random payloads for HTTP requests.

And then we run like each test case with this random payloads couple of times.

And if it passes on all the locations, then we really hope that something's coming from

it because we are not testing anything. Yeah. You expect, you know, it should fail. If it passes, you know, there's something wrong with the tests. And that's like the nice inversion of this. Run your tests twice. I should like test twice. Yeah. Yeah. Yeah. Yeah. Yeah. Yeah. I have a question because I really like your slide that you suggested that this is not test just for implementers, but also for users. There may be like multiple commercial or non commercial gateways that you may trust. They may give you like a free tier, but then you maybe you want to verify what's the value of the product, right? But then I think another demographic that could benefit from this is developers who wants to build on IPFS. Maybe they want to use the IPFS. Maybe they want to use the IPFS.

Maybe they want to integrate a gateway library in a specific language into their code. Did you have any like initial thoughts about what should be the approach? I understand that this is like a tool that people building a library could run on their CI, but is there like the idea of creating something like web platform test website where project libraries and projects could sign in to show that they are conformant?

As like a marketing strategy for developers? Yeah, definitely. And that's also why we're working closely with Robin on that.

So he's the perfect person to lead us through how to make it a reality because of his previous work with web platform test.

So yeah, that's definitely a plan at some point in the future, but not quite yet.

