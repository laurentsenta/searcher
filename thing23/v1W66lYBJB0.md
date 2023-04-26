
# Are We Interplanetary Yet? Designing IPFS for Space - Ryan Plauche

<https://youtube.com/watch?v=v1W66lYBJB0>

![image for Are We Interplanetary Yet? Designing IPFS for Space - Ryan Plauche](/thing23/v1W66lYBJB0.jpg)

## Content

My name is Ryan. I work with Little Bear Labs, and Dietrich volunteered me to attempt to answer

this question of, are we interplanetary yet? Which, of course, is referring to the effort

to put IPFS in space and live up to this name of the interplanetary file system, not the

intraplanetary file system. So, like a true engineer, I'm not going to promise anything

I can't deliver, and I'm going to talk around the answer to this question and talk about what we're doing right now, and probably eventually let Dietrich actually answer this question.
So, first, I want to introduce MySelli. So this is the new IPFS implementation. I'm thinking

of it more as a building block for deploying IPFS into space. I'm sorry I don't have any
cool logo right now. If someone wants to contribute a logo of fungus in space, that would be great.

Design isn't really my thing, but if someone else out there has some great ideas, I would love to have a logo contributed to this. So, IPFS in space. Obviously, this cannot be your

cloud server's IPFS. Whatever works here on Earth in our high-performance servers with

all these connections isn't going to work well on some satellite that maybe has an unreliable

connection. Maybe in the case of my hardware in my office, like a 60-byte MTU isn't going

to work for the software that we use here on Earth with our mobile phones and laptops and all that. So, kind of have to go back to first principles. In some ways, ask, like, what even is IPFS? And I appreciate Robin bringing that up and helping give us the flexibility

to reimagine what these implementations mean, what they do. In our case, what this means
is starting out with just content-addressable data, finding some way to move that across
the radio link from satellite to a ground station, and maybe calling that good enough for now to say, okay, yes, this is IPFS. We're shipping SIDS into space or from space to

the ground. But, you know, as we dig into this, as we look at this, we've realized maybe

satellites are a little more like cloud servers than we've realized. Or maybe we can think
of them a little more like cloud servers. So, one of my goals with building this is

I want to give you all a platform where you can iterate on things like a better data transfer
protocol or maybe solving the peer-to-peer problem in space without worrying about the
hardware constraints of satellites or the specific details there. So, attempting to use network layers as an abstraction to push away specificity of hardware and maybe emulate

or approximate some of those constraints so we can build a more general solution that
we can test on the ground and still run in space. But we'll let the system integrators,
they worry about the radio, and we just worry about building the protocol and thinking about how it's going to work. So, how do we actually become interplanetary?

So at some point later this year, this software will be running in a satellite in an orbit

near you. I can't say exactly when that will happen. I'm going to let Dietrich be the one
to announce that once we actually know. I don't think we actually know right now, though, when that's going to happen, and that's okay. That's like the reality of space, that things change, schedules shift, especially when you're like a little tag-along like this mission
will be. But the reality is, as FogCoin announced, we are planning a demonstration mission, and
what this will look like is transmitting data from a single satellite to a single ground station, so shipping some blocks back and forth, and maybe even seeing those blocks
go from satellite to ground station to Qubo to the broader public IPFS network. I think

that's like the gold star that we're reaching for in terms of demos, so really seeing a whole integration of space to big IPFS. So if you want to hear more details about that
and see a live demo of how it's working, I'm giving a talk on Monday in the integrating IPFS track. Functionally, this software is essentially complete. We can show shipping

bytes or shipping blocks from a satellite over a radio to a ground station and then
to Qubo and resolving that as a complete DAG and a file that you can do something with.
So I'll be showing that off. If you know someone who could use this, if you know of a space

opportunity, please come talk to me or Dietrich. We would love to hear about other people that
can use this and try to publicize it to them and get more usage here. And if you want to

take a look at the code, see how it works, maybe give it a try. We have the space repo

in the IPFS shipyard organization and also we hang out in the space channel on the Falcoin

Slack. So we would love to hear from anyone who's interested or please come talk to me.
I'll be here the whole time. Let's talk about space and IPFS and the different challenges there and what we can do with that. Thank you.

