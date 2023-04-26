
# Explorations into Decentralized Publishing - David Justice

<https://youtube.com/watch?v=fn5QNvRXMIo>

![image for Explorations into Decentralized Publishing - David Justice](/thing23/fn5QNvRXMIo.jpg)

## Content

I actually changed the title of this talk because we're not announcing the blog today.

It's still in flux and I actually got some updates with Dietrich yesterday. We went through it. V2 is sitting on that notepad right there. So I'm going to go ahead and walk through our V1 of the talk and then some of the ideas we're coming out with after that. So that's why the talk title is a little bit different here. So as you saw this morning in Dietrich's opening talk, we work on a lot of different stuff
in a lot of different places and we need a place to rapidly communicate that work. We don't always want to push every single thing to the IPFS blog. It might not be appropriate there. We want to communicate really rapidly. That's why we need a team blog. On top of that, we want to dog food the IPFS, FVM, any like related technologies that we want to just work with it day to day. So we can improve our understanding and then, you know, just see if we can make the tools better, of course. We also wanted to enable builders to use these tools that are available now.
And maybe we can use this blog to come up with patterns that we can build for other
users to build for end users. So why didn't we just use a regular blog? We wanted something that's censorship resistant. Not really a problem for us specifically, but in general, that's a, you know, good thing for decentralized publishing. We want content ownership. We want verifiable authorship. Verifiable content with the CIDs, right, since we're publishing on IPFS. And we want resilience to platform instability. And we want user agency for the end users who are going to be consuming this information. So how's the blog built? So version one, we have basically a viewer, which is a single page app.
And then we have a smart contract, which is currently only holding two arrays. It's a list of the blog CIDs, which is just markdown files, and then a list of the authors
who can actually push to that smart contract. And then finally, we have a publishing tool, which is currently just a CLI that all it does is just interacts with that smart contract. Basically you upload your markdown file to IPFS, you get the CID back, you pass that

into the CLI tool, and that'll go ahead and update that smart contract. This kind of just goes over the viewer again. One of the cool things about the viewer is we have all this information, right? We have the transaction hash, we have the wallet info, and we have the CIDs. So we can display all that for the users so they can themselves go and verify this.

We also are currently storing this stuff in local storage. So we want this to be, the end version of this, to be offline-friendly so that people

can pull it up. If they're offline, they can still view all the stuff. And then once they get online, they can sync and get the newest posts.
Contract same thing. List of post CIDs, list of authors. One of the reasons we wanted to go with the contract initially was that we could separate
all these CIDs and everything. Instead of doing just a statically generated blog where all the data's living in that and being pushed around IPFS, I thought it'd be interesting to have it available to maybe
data aggregators, people who want to pull this stuff in, make it searchable, or back to the user agency. If people want to have their own viewers, they can just pull this content down with all the information, who the author is, everything, and view it on their own terms or in their own read or whatever. We've talked a little bit about doing some decentralized RSS work. Maybe something we're going to integrate with Durin one of these days, maybe with this blog. So this was kind of like why we originally went with the contract idea, was that separation
of concerns. And then the publishing tool mostly covered this, but takes in that CID, super duper simple,

and it just talks to the smart contract. The next version of this, we want to basically just do everything in browser so that the authors, the people on our team, can just log in, edit the markdown right there, go ahead and use MetaMask or whatever, maybe the Brave wallet, and just sign that transaction and it automatically updates. So after looking at this blog, I was thinking, we already have the basic static file hosting,
and if we have the ability to add and edit dynamic content with the smart contract, and on top of that, in addition to all that, we can add a transparency, verifiability with
the smart contract and all this. What else could we build with this pattern? I feel that we could probably replace a lot of Web 2 things right now with this pattern, if we just kind of get the pattern right. So that's what we're trying to figure out right now. We have some improvements that we've been talking about this week. I'm going to keep hacking on it. The hack days, I'm definitely going to be working on this. If you want to talk to me about it, we can definitely do it. Part of the thing we want to do is we want to remove external dependencies, probably get rid of the APIs to talk to the smart contract. I think that we have some interesting ways to pass around configs and just sign that kind of after the fact so that we don't have to be pulling from this chain directly. It'll just be there to verify transactions at a later date.
The RSS subscribing, that's an interesting thing we want to check out. We want to instrument this stuff so that I can get some metrics, see how people are taking
in this pattern. We're going to keep blogging on it, documenting it. I'm going to be using the new JavaScript library, IPFS, in this publishing tool to try and bring

this all into one piece so we don't have to use that CLI tool. Again this is a pattern, it's a work in progress. I would love your input, love to chat about it. We're in browsers and platforms on Filecoin Slack. That is the repo. If you want to poke around in there or put issues in or tell me why it's not actually decentralized enough, I'm happy to hear all that stuff. I'll be at these hack days. So yeah, come by. Thank you. Yeah, is there any questions?

That's a lightning talk. I didn't want to keep you all from the happy hour. So just to clarify, there's an ETH backend and does that do, just as, wait, you mentioned

there's something with blockchain, or did I imagine that? Currently there is, and now it's kind of funny, like literally during these talks, like yesterday and today I was talking to Dietrich and I was like, I think we can kind of cut that out. But yes, currently there is. And what's on there right now is a smart contract that stores an array of the CIDs, the post
CIDs, and then an array of the authors. And that's all that's on there right now. Yeah. All right. I had another question, but then I forgot about it.

Oh, and so, okay, why do something like that over IPNS or just because...

Originally we were saying, well, it seems simpler at the time to just get this thing going basically. And we really liked the idea of me and like the contractors working with, we really liked the idea of separating out the data from that. We were looking at like, are we going to have to regenerate the viewer page, basically like the viewer page every single time. And it's every time someone goes to the blog, are they going to have to pull this whole thing down? And we kind of liked the idea of like having this offline thing and then just fetching those posts behind. But after what we were just talking about, I think we can just put that config on IPFS and then link to it from IPNS and it should be fine. And then we'll just sign that transaction linking to that CID so that we still have that like authorship there. Where that config specifies the SIDS that compose like the posts for the blog?
Correct. And the authors, but you wouldn't, you know. Yeah. So at some point you have to update a CID somewhere and publish it and verify it. Yeah. Yeah. And that's what you would update is that config instead of the smart contract is what we're thinking. Yeah. Yeah. Yeah. Right. It's less of a question, more of a comment.

I think this is a great demo project for like hackathons and stuff because it uses a lot of technologies that we like really care about. And curious if you can at some point also turn this into like a tutorial on how you build it. But that's for later. Yeah. I think it's still like very, very early in this process. You know, we kind of just, and it's been sort of like a back burner project. So but I'm happy to share it now just so I can get like input, you know. But definitely we want to build out these tutorials and I really want to just establish what's like a couple of different patterns of this. Because I think that like a lot of the tooling is already here to start building this stuff, like actually decentralized apps like this in pretty simple ways. So I just wanted to, yeah, I don't want to build a whole framework. I don't want to do that. I just want to put some examples out there and really like establish these patterns.
Can you update it from Durin? Not yet, but eventually that'd be very cool. Yeah. Yeah. I think that'd be a great example of doing that sort of thing like on the fly and thinking

about what might go in the Durin code base to like support that and go back and forth and see if some of us can also like lean into the fact that you have Durin as a specialized

client, you know, again, maybe in beta mode or something like that. Right. So it would be amazing if you could take advantage of that, like take a picture, go to Durin,
immediately upload it and very simply go like, oh, that goes on the browsers and platforms
photo blog or something like that. Totally. I can vote for new features all day long. I can put that in the issue queue. Yeah. Put it in there. I like as many ideas as I can get. I love them. So I think I understand where you're coming from with the offline client as a single page
app that knows how to pull the data structures. Yeah. But as a tutorial, like for the mission of telling patterns to people, you might just blow a bunch of web developers minds if you do like a static site build right then and have people serve HTML. Sure. Sure. Yeah. If you're going to do one of the most basic patterns, then we can just keep building off of that. Yeah. Another very basic question. You may have mentioned this. Where do you publish the SID for the config that describes the blog post? So that would be, what do you mean publish? So you have this, the config can detail CIDs, which lists blogs, which you actually use

to pull content. But how does the user know what is the right config for the current state of the blog?
That would be on IPNS. So they'll be pulling, they'll check that and then that'll have the most up to date SID. And then that config, but with the offline thing, the offline thing was going to like store that config and then like check that. But that's now I'm seeing as I'm talking about not super necessary to explain the pattern.
That's just an extra, but yeah, so that's all it is right there. But maybe we can use the name name service. Like I have to hear about that today. That'd be pretty cool. Yeah. There you go.

