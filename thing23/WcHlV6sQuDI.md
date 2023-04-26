
# Spec IPIPs Year in Review - lidel

<https://youtube.com/watch?v=WcHlV6sQuDI>

![image for Spec IPIPs Year in Review - lidel](/thing23/WcHlV6sQuDI.jpg)

## Content

I'm Lajdel, part of IPFS2 at Protocol Labs and I lost my USB dongle so I'm on a borrowed laptop.

This will be a rapid fire year in review of specification IPIPs.

IPIP is a four-letter acronym for Interplanetary Improvement Proposal.

We had to come up with something that's four letters so that's it. And the last year in Iceland we announced IPIP process to the community and asked if there is a spec, propose a delta, propose improvement proposal using this new process.

If there's no spec, write it. So if there is something you want to change in a spec, open pull request, it does not have to be perfect, there was a template just to get the conversation rolling.

And this talk will go over some highlights of the IPIPs that happened in between that moment when we announced the possibility of engaging with the specs and today.

So right now we have a project board where we try to roughly track the lifecycle of IPIPs.

Things that are in progress are softly accepted for the continued conversation and once the spec gets reference implementation, once we agree on the shape,

we then discuss how we would test that, what types of pictures would you include and at the end of the pipeline when we have a spec that people are fairly happy,

we have an ad hoc group of spec stewards and we try to at least check in with people from outside of PL who maintain other implementations like IRO to be in the loop to raise any concerns about long term possibility of supporting those things.

Even if the implementations today don't plan to adapt a thing, we are planning ahead like in five years from now if it turns out it's something you would use, would you be happy with the spec?

And then once it's ratified and ships with at least one reference implementation, that's usually kubo, now the box library, we move that to ratified column.

So we had a bunch of proposals and I started with ones that we did not ratify.

We actually had a very lengthy discussion and then discussion ended, but this is still a very useful artifact for the community.

So for example, the first one we had discussion about exporting DNS sec proofs for DNS link records.

It was proposed by CloudFlare, but at the end of the, during the conversation we realized actually it's not IPFS specific, this should be part of DNS over HTTPS and should not be tied to IPFS records, you should be able to fetch proof of any record.

Similarly, we had a very lengthy discussion about CIDV2 and at the end of the day, people realized they can do the thing that they thought they need to introduce CIDV2 by way simpler means without impacting wider ecosystem.

And if we did not have IPIP process, and I will like stick my neck out probably by saying this, we could have a problem because someone could ship CIDV2 and then we need to live with this.

Every IPFS implementation have to implement CIDV2 now because one team had a need and other people did not really care or were not in the loop.

So I think the system works and the fact that we had a very lengthy conversation is like dozens of back and forth on this one.

And at the end, it was, everyone was happy with resolution. So now things that shipped. Mostly we have a gateway specs, so most of the things that shipped through IP process were gateway related, one support for redirects files, which enabled people to slowly shift website hosting to gateways.

That was submitted from the outside of PL, that was community driven IP, one of the first ones, and then people started using it and noticed there are some discrepancies with other parts of the gateway spec, namely the subdomain behaviors.

So we have follow up IPs that improve and build on top of that. And gateway redirects file is supported since Kubo 16. I tried to dig in information about when we shipped some things. Additional things that landed, ability to fetch a bigger directory trees as a tar stream in the serialized form, Kubo 17, JSON and C-BOR in there, also like their DAG C-BOR and DAG JSON representation of IPLD data model.

Now you're able to request UnixFS directory as the JSON that shipped in Kubo 18.

In Kubo 19, we closed the gap around verifiability in IPFS ecosystem on gateways.

In the past, you were able to fetch a block or a car, but you had to trust someone to resolve IPNS for you.

Now you are able to fetch a signed IPNS record from gateway without running entire P2P stack, without having to run DHT or leaking your browser history to some centralized indexer.

Now you leak that to a gateway you effectively trust or not trust, but you can control that by having multiple gateways to choose from.

So that's Kubo 19. As a part of the project RIA, we are working on a thing that we've been open for a while.

We have a support for retrieving multiple blocks in a single car archive, but that was very limited.

It always gave you the entire subgraph. And if you are fetching Wikipedia, that's hundreds of gigabytes, and maybe you just wanted a single directory to enumerate items there.

So as a part of a project RIA, we realized we need some sort of a partial car support, partial car export.

We call that graph API. And I believe that's like yesterday, this week, we have IPAP opened to add additional parameter, additional scope selectors to the car export.

Being able to limit the depth, logical depth, and also support translate HTTP byte range requests to block requests on IPFS, which will effectively enable people to write more efficient light clients.

And piggyback, going back to one of our existential threats, bad bits and deny lists are never a priority until there's an incident.

And that is a priority for a week, and that is no longer a priority. But over time, people are pushing forward. So we have Cloudflare starting that conversation, proposing the format.
Then we have people who run operations within PL and outside, providing feedback based on that.

Identifying that the first proposal was very limited, and we need more holistic view of how the list would be created, how list would be parsed and maintained, and what are the performance limitations around that.

And then how would we build governance around maintaining those lists?

It should not be just PL maintaining a single list. There could be organizations being able to use the same format, maybe building lists from other lists for specific jurisdictions, maybe implementations, automatically enabling lists depending on which country you are running your node.

So we are on that path. The latest IPAP 383 proposes compact denialist format, which covers a lot of use cases.

And it's already supported by the Bad Bits, which is the fact of the current thing that we have.

You can give it a try. There's also implementation in Go. In the past year, we cleaned up our delegated routing story.
We removed the experimental reframe protocol, and we replaced that with way simpler HTTP API for us.

Just give me some providers of this CID without having to run specialized clients.

It should be very, very easy. IPAP 387 started us on that path. It's implemented by CID.contact IPNI indexer, and support for that is in Kubo18.

And we have a bunch of follow-up work and ongoing work around this.

Currently, we only have delegated content routing, but there are other types of routing. There is peer routing, being able to resolve IPNS records currently.

Those gateways that return your signed records, they should not be forced to use DHT as well.
They should be able to delegate that to some other box. We are working on both adding other types of routing to that HTTP spec,

but also making it possible to not only get data, but also publish over HTTP.

The fact of being able to announce your provider records over HTTP, or announce your IPNS record over HTTP instead of running full DHT client,
kind of grows the pie. It's not like we are replacing Kubo. We are just filling the niche where people would not be able to run things like Kubo.
Streaming is important, especially if you are doing DHT proxying through this delegated API.

You don't want to wait multiple minutes before you get a response back.
You want to get records as they arrive from DHT crawling, hopefully later this year.

Finally, I mentioned there are huge gaps in our specs. One of them is IPFS DHT specification. There is a proposal to add double hashing DHT to our ecosystem,
which helps with reader privacy. But to do that, we need to define the current state of things.

That's very positive. One, we will have a spec of DHT, and two, we'll have a better DHT as a process.

There's a long tail of other things that we want to do. Cleaning up IPNS spec, improving the way UnixFS and gateways interact with each other around content types.

UnixFS specification is probably the biggest thing to land.

When we have time, things like data onboarding over HTTP post. And finally, the indexer story. It's still in the flux. We need to figure out how to discover them. What's the mechanism for that? Probably not centralized HTTP endpoint. There are better protocols for doing that. Yeah, so probably I ran more than I should.
But this is a small set of highlights just to show that the process is working.

And I'd say keep them coming. If there's no spec, write spec. If you have no time to write spec, open an issue highlighting there's this gap in the specs.

And if there is a spec and you want to improve it or propose a change, open IPAP.
It does not have to be perfect. There's a template. We are in the process of moving IPAPs into this specs IPFS tech website to kind of like showcase them.

The ones that we ratified and also the process. So it's easier for people to discover not only the GitHub history of the spec changes,

but also understand the rationale behind them, the rationale behind every spec change.
This like audit log, our community memory. Why did we make some decisions? Why did we make some changes to the specs? That's effectively the value added by the IPAP process. So keep them coming. Thank you. Anyone want to write a spec?

Sorry, any questions?

Besides writing specs, which is great and makes sense, is there any other way for people to be able to help? So there are multiple ways you can help. One, look at the process. If something seems too complicated or outdated, let us know.

If you see a way we could improve it, let us know.

It's like things with principles. It's like a living process. So we are looking for feedback. In general, when it comes to spec improvements, it's not just technical.

It's very important and very valuable for people to read specs,

and if there are gaps, if there are mistakes, or something could be very confusing,

especially for people who are not native speakers. We run into this trap where we describe fairly not that complex things

using too complex language, and that introduces artificial barriers.
So even people who don't feel like they could contribute technical things,

proofreading specs and raising questions, that makes specs better.

Because the specs should be written in a very, very simple language.
We are building complex systems. We should not make things more complex than they have to be. You cannot make the complex thing because the debugging will be way more complex
and you run out of your cognitive capacity to understand the system, so the specs should be very, very easy to comprehend. And you don't have to file an IPIP to improve the language, right? Yeah, so the IPIP is specifically the process to create this audit trail,

why did we make some technical decisions. If you see a gap in the specs, there's no spec. You don't need IPIP for that. Just open PR, filling the gap. If there is a typo or if you want to rephrase a sentence, that's an editorial change.

Editorial changes do not require IPIP. If there is a technical change, then you need IPIP. So we wrote all the specs we needed. That's good. Who's validating that the specs are being implemented? So that's why we don't need conformance tests next to specs.

Are we writing specs in code? Yeah, so for example, the one I showed...

Yeah, so for example, for this IPIP, when the redirect support was introduced,

there was a reference implementation in Kubo, and the IPIP itself included test fixtures section with CIDs.

It's very nice because you just put CID. With CIDs that include all the important edge cases or the important use cases,

examples of important use cases for this specific thing. And then we have the gateway conformance test suite. Now, if we had it back then, it would be part of this IPIP. The idea is that before it's merged, you have test fixtures or at least explanation how to create them. And in case of gateway IPIPs, test fixtures are part of IPIP,

and the same test fixtures, exactly the same CID, exactly the same cars are part of the conformance tests, and there are physical tests that demonstrate how things should work.

And then implementations like Kubo or someone else need to pass them.
And the nice property of this process is that different implementations

use the same test fixtures. So we are testing against the same thing instead of writing our own tests
which pass our own code, and then we argue that this edge case is not a bug

or is a feature. No, at least we can agree that the fixture from the IPIP are the baseline that we should pass. And that can also raise the bar for people who are reviewing IPIPs
to focus on the test fixtures. Are the test fixtures present in the IPIP cover all the edge cases?

So it's kind of like exercising threat modeling of sorts, but it pays benefit long term because people have more confidence.
Oh, I don't need to think about tests. I just use existing fixtures. Yeah, Nat, one thing is to bless you, Dietrich. One thing is tomorrow, I think at 11, there's a non-conference workshop on

don't cite me on the exact time, I'll have to check it out, but there's a non-conference workshop on tests on the test suite. So that is definitely an important part of the infrastructure to go with this.
And eventually we would like to get test results for all implementations
on the spec site so that when you're reading a spec, you can see which implementations pass how many tests, and you can get a sense of the back and forth between testing and specs.
11 a.m. 11 a.m. Yeah, that was even remembered it right. Thank you. Yeah, I'm just curious, Lyle, obviously a lot of this happens on GitHub, which is great. Where does some of the verbal conversation and decision making occur?

Yeah, it's a very good question. So there are actually two questions here. One, where's the place? And two, who are stakeholders? Who are the people participating in the process? So currently the home for IP process is in IPFS implementers working group.

We have biweekly, I believe, call.
We have people from various implementations,
usually Kubobox or we have folks from number zero on that occasion.

.:And we have during that call, we have IP IP corner where at least we try to not spend too much time at the same time.

But if there's a new IP IP that I or other stewards sees or proposes, we bring that to attention of the community saying, hey, this is this thing.

We are planning to land that within that many weeks, let's say, like in Cuba.
And there's the time to review it. And we will plan to have like a soft vote on ratifying that within like one or two cycles of this meeting.

So that's going back to the first problem I mentioned. Who are the stakeholders?

Currently, we have our thin on spec stewards of sorts. I think that group should be like way more diverse than just people from IPFS stewards group.

This group. But I think we can improve on both fronts.

One, growing that group and making sure it's like diverse enough. And two, if we start squatting too much time on that call, we may nucleate out of that.

But for now, that's IPFS implementers call. I believe it's in IPFS community calendar. Yeah. Yeah.

