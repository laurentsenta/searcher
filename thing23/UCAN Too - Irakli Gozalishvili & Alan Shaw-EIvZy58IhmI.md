
# UCAN Too - Irakli Gozalishvili & Alan Shaw

<https://youtube.com/watch?v=EIvZy58IhmI>

![image for UCAN Too - Irakli Gozalishvili & Alan Shaw](./UCAN Too - Irakli Gozalishvili & Alan Shaw-EIvZy58IhmI.jpg)

## Content

Hi, welcome to the workshop. This is the workshop for UCANs and you can learn how to do UCANs too.

I'm Alan, I'm an engineer at DAG House and Irakli is just down there. He is an engineer also.

If you need help during the workshop then we can try. I'm going to do a little bit of an intro and explainer before we get stuck into the workshop.

Let's just start with the first thing that we should probably cover. What even is a UCAN?

UCAN stands for user-controlled authorization networks.
The key thing here is that they are user-controlled. UCANs are a way of doing authorization where the user is fully in control.

They started out as an extension to JWT. You can think of them as a token that you pass somewhere to gain access to stuff.

They are actually really different to a traditional API key or something. Everything that a user is allowed to do is captured directly in the token.

There is no all-powerful authorization server. There is no authorization server at all.

I'm just going to dive straight in. This is what the inside payload of a UCAN looks like.

Let's have a quick look through the fields and I'll try and explain them as best I can.
The issuer field is the identity of who issued or created the UCAN.

You can think of this as who the UCAN is from. The audience field is the identity of who the UCAN is actually intended for.

It's like the recipient of the delegation, the UCAN. Expiry is a time stamp that specifies when the UCAN stops being valid.

The big important one is the attenuations.
They are basically a list of capabilities that have been delegated to the audience, the person who is receiving the UCAN, by the issuer.

Let's look into them a bit more. In this particular case, the capability called store slash add has been delegated.

The with field here is the resource the delegated capability applies to.

For example, in web3.storage, in our new APIs, when we delegate store add, the resource is typically like a space, what we call a space, a place where you store things.

The proof field, which I haven't filled in here, but it's there to, it's a list of nested UCANs that prove the issuer is able to delegate those capabilities to the audience.

And then the whole thing is like signed using the private key, so you can verify that it was sent by the issuer.

That's it. There's not loads to it. You've seen the internals now, so you might be getting an idea of what is possible with UCAN.

Let's just go through them, or enumerate them at least.
You can delegate capabilities, specific capabilities like you saw in the example store add, but you can also delegate a wildcard, like all capabilities, or a wildcard underneath a namespace.

So you could delegate store slash star to get all the store capabilities that are available, for instance.

And then you can delegate multiple capabilities. You saw that the ATT field is an array, so you can delegate multiple things in a single UCAN, or just one.

When you create the UCAN, you specify who the intended audience is, like who's going to be receiving that UCAN.

And that means even if someone intercepts that UCAN, it's not really useful to them unless they have the private key of the person that it's intended for.

So one field we didn't cover in the deep dive is caveats.

Basically speaking, every delegated capability, you can optionally specify these caveats in this field called NB, and this can be used to narrow the scope of the delegated capability.

So for this, as an example, if you had a capability to delete something from a collection, you might use the caveats field to restrict the content types of things you can delete, or maybe specify file name suffixes that you're allowed to delete, for instance.

It has expiry, as you saw. The expiry field allows you to create really short-lived delegations for time-bounded operations.

It also means that you can have really short-lived delegations or UCANs that you don't need to revoke because they only apply for a little amount of time and it doesn't matter so much.

But what you can do also is specify this not before field, which means you can actually create UCANs that will become valid in the future, which is super cool.

So that's what can you do with UCANs.

And then the really big one of things that you can do with UCANs is you can use them for invocations, and the idea is that when you receive a delegation, you actually want to use that capability that it provides you with.

So there's a whole spec for this, and it's being used in IPVM as well as other places. We're using it in Web Refuter Storage as well.

And this is what we'll be working with primarily today in invocations. It's UCAN RPC, Remote Procedure Call with UCANs as authorizations.

And so that segues nicely into the UCAN Working Group organization. UCAN Working Group is on GitHub. It's got a ton of resources there.

A good place to ask questions, read up on the various UCAN specs, obviously the spec for UCAN themselves as well as the invocation spec can be found here.

That's UCAN-WG on GitHub.
We have an implementation of UCAN in IPLD. It's currently JS only.

The Fission guys drew this cool avatar for us, mascot, I guess. It's kind of like the IPLD logo, but upside down. It's like a jellyfish.

Anyway, so you might want to use this if you decide to build an IPFS-enabled UCAN application.

There's also a bunch of other libraries for working with UCANs in other languages, which you can go to and use.

But the most important library that we want to introduce to you today is this library which we call UCANto.

It's a UCAN RPC library. It's written in JS. And it conforms to the UCAN invocation spec.

And we use it in the new web3.storage APIs, which we're calling w3ups. It's currently in beta, but you can access it and use it right now.

What we're going to do is use UCANto in the workshop today quite extensively.

UCANto provides nice client and server abstractions, allowing you to perform invocations with delegated capabilities.

On the server you can provide handler functions to deal with the requests as they come in.
You don't have to worry about validating delegation chains and stuff like that, because the library does that all for you.

It's good. And there's a bunch of other cool tooling which I have not mentioned here.
UCANto! If you remember nothing else, UCANto is the thing.
So, workshop. Are we ready to do a workshop?

A couple of people at least. It's an observable. We also have a leaderboard. I'm going to pull up the leaderboard as well.

It's a competition. It's a game. A competition. We're going to play with the observable to create delegations, enter the competition, and you have the chance to win chocolate gold.

If you are top of the leaderboard at the end of the workshop, you will win a chocolate coin to signify your ability to do things well.

There's a prize for the participant that gets the most points.
We have more than one of these just in case more than one person gets the same amount of points.
Right now, it's time for you to open up your laptops, go to this URL.

You might need to register for observable, I guess. You don't have to register. It might be an idea to fork the observable. I don't think you need to do that either. If you want to keep it, I think you need to fork it. And the observable basically has the instructions for the workshop, what you need to do to win, I guess.

Essentially, you follow the instructions and you will make invocations to our Yucanto server and you will gain points for doing the things in the right way.

You will see yourself on the leaderboard. I will pull it up now.
If you have questions or problems, then just grab me or Akeli.
We will be around. Pull it up. See how far you get.
Go wild!
When you open up the URL, it should look something like this. We have got agent key pair. The website is going to be your agent in this workshop. You need the key pair. We generated one for you here. Your identity is this DID that gets listed here.

Then we have got the provider address. This is like the server side of things.

This is just connecting your agent to the server. Then we can get the connection to tell us exactly what kind of RPC endpoints are available.

It will show you that in the IPLD schema syntax.

You can see what you are able to do. You don't need this just yet. Once you get to here, entering the challenge should be as simple as heading on down to this button that says enter workshop.

If you click on enter workshop, you should see a DID end up in this list just down below.

Which is your DID. You just submitted a request or invocation to the server to say I am here.

This is my public key. We have got some people in there. We can see their score as well. We also have...

This is the current leaderboard. There are some people who have entered.
I will leave that up. Once you have entered the workshop, continue on the adventure.

Give us a shout if you have problems.
In the observable thing, is it easy to see the imports?

What code is being used? Just wondering. Down there.

Here they are. Cool. That's nice. I just wanted to see what is being used. Cool. That's an important point. In observable, it is this reactive programming thing where you have variables and things that have been defined.

If they get updated, any code referencing that same variable will get rerun.

It is constantly running and rerunning whenever you make changes.
Everything is like a block. On the left-hand side, there are arrows. Sometimes we have hidden most of the ones you don't need to see. But pinned things open where you do need to see it. We could have messed up. There are arrows on the left-hand side where you can drop down and see anything that is hidden away.
Be on the lookout for that. Largely, it should be as you need it. As you need to work with it, the code will be there for you to edit and run.

Once you have made changes, there is a blue play icon in the top right of each block.

You should press to get those changes, to save those changes for that block.

Oftentimes, when there is a button, if you have made changes to the thing, press the blue thing, then press the button.

That is something we have run into a few times. I see we have some names up there now. Which is great. The points you get, it is also like a time-based thing.
If you are the first person to do a particular task, you will get more points.
When you come to delegating to other people, it is a choose your own adventure.

You either get to export a file with that delegation in it.
You have to somehow send that to someone out of band over IPFS or something.

Or you can use this special thing, which we created, where you can actually stash that delegation in the server for the other person to then claim.

Just be aware, when you are delegating to other people, there are these two options for doing things.

You will delegate to them. They need to somehow claim it or receive it. There is a big or in bold, but it is maybe not big enough.

Or is just pin over pin. The capabilities are set up so only that person can claim it. Incidentally, the access claim and access delegate capabilities are also some things that Web3 Storage implements.

We have a spec for it. The idea is that it is pretty general. When you delegate to someone, only the audience can claim it.
Or we will give it only to the audience. But even if somebody gets to have it, they won't have a key, so there is very little interest to doing that.

The person who has the capability, the person who controls that audience, just shows up and says, do you have any messages for me?

Or do they have to say, I am trying to claim a specific thing? No, you just say, you just invoke access claim with your DID.

And anything that had been delegated to the DID, you will get it. I think we frame it 10 at a time in case there are too many. But then you can do it. Also, once you claim it, we will remove it from the server. So it is not going to stay there. Yeah, well, I coded it last night, so maybe it crashed. I am sorry. And now everyone gets to try it again. And maybe a few. Yeah, I am sorry. We also have a server that you can run yourself. And in observables, there is a switch to use your local host version if you want to do that.
And there is also a few things you can do. You can write loops and make a disco background on your avatar if you want to.
Yeah. And there are a bunch of things you can do. Like you can abuse the colors and we don't escape it.

So you can mess with HTML if you want to. So, yeah, if you are looking for a challenge, that could be fun.
One thing that you should maybe watch out for is that any delegation you make can expire.

So if you don't use it quite quickly, you might find you are not able to use it.

There is another fun exercise you can do if you want to. So when you define the schemas or when you do the delegations, there is a type union.
So you can say, well, you can change my background color to yellow or red, but only those two.

And delegate that. So be creative. Don't just do what observable does. The Ucons that you delegate, I think they expire really quickly. I think it is 30 seconds or so. But then you can set expiration when you do that to set to whatever.
So you can do that here. Although it is better to have short-lived. So that's why it is default. If you do the longer delegation chains, you will get more points.
Like you can delegate to someone and they can redelegate to someone else, et cetera.

All right. We are running out of time now. Let's do should we call it quits? Whoever is at the top. It is okay.
All right. So burrito. Am I going to throw this? Is that a good idea? Ready? Yes. Second is ICID asset.

Yes. Good job. Are you ready for this? All right. Nice. Third place is free can. Nice. All right. Yes. I have got loads more for everyone else. The smaller chocolates. If you are on the leaderboard and you feel you deserve a chocolate, come and grab one.

Otherwise, feel free to keep, we have got ten minutes left. Feel free to keep hacking. You can keep hacking on this afterwards as well. We will try to fix the server at some point. Sorry about that. But you know, some of it worked. Yes. Well done, everyone who came and did a thing. You all did really well. A lot better than I thought you would. Thanks. Thank you.

