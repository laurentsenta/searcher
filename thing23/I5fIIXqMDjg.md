
# Implementations Showcase: Iroh - IPFS Reimagined - dignifiedquire

<https://youtube.com/watch?v=I5fIIXqMDjg>

![image for Implementations Showcase: Iroh - IPFS Reimagined - dignifiedquire](/thing23/I5fIIXqMDjg.jpg)

## Content

All right. Hey, everyone. I'm excited to be on this stage at an amazing time for IPFS.

For those who don't know me, I'm Digg. I work at number zero on this cool thing they call

IRO. A lot has happened since last I talked about it. So let's take a quick look. After

IPFS camp, you might have noticed, we had to look at the challenges and opportunities we had

with the dead existing version of IRO. This led us to refocusing on these four core values

with IRO. Reliability, performance, efficiency, and peer-to-peer. In our opinion, these are

non-negotiable when shipping an IPFS implementation. And so we went and rewrote IRO from the ground
up. Looking at our roadmap, you can see we're tracking four high-level parts. Data structures,

data transfer, connectivity, and content routing. For both data structures and data transfer,
we are now actually in the test and improve phase. So we actually get to test, do our
theories apply to the real world. The last few months, we have also spent a lot of time
working on connectivity. So we'll be starting to test our results soon. But for content
routing, we have a good overall understanding only so far, with some initial research done.

But implementation had to wait a little. We weren't lazy, so we've shipped three releases

since our refocus, including the current 0.4.1. And they both present the headway that we've
actually made into our roadmap, as well as forming the foundation that our partners have
started to build and rely on. Because we want IRO to solve real problems every step of the

way, we have worked with the amazing Delta Chat team to deliver an integrated backup transfer solution, which you can see here. With this, IRO allows users of Delta Chat to move their
data from one device to another in a verified, encrypted, and fast way, content-addressed,
as you all expected. And we're excited to announce that this is actually live today

in the latest releases of Delta Chat, meaning it ships to hundreds of thousands of devices covering all major mobile and desktop platforms. So let's talk about performance. This is how

it has to go, right? So a long time ago, folks realized how critical it is to measure what

you want to improve. So we've actually been true to our word and built a continuous measurement platform for IRO, at least, at perf.iro.computer. Shout out, Asmir. This records the performance

of every commit we merge and gives us and all of you an easy way to track and follow the performance of IRO as it develops. Today, we use a small but growing set of benchmarks,

which include network tests with simulated NATs and latencies and packet loss. We don't

only use automated benchmarks, but also try real-world-oriented tests, like transferring
the source code of Linux kernel on your local machine. You do that every day, right? Which, for IRO, takes now 23 seconds on the current development machine and puts us actually into
a realm where we want to be in. But the real comparisons that we need to draw are comparing

to Web 2.0 technologies. And so we're pretty proud that IRO now actually approaches transfer speeds similar to those of curl when transferring data on a local machine. This means not only
are we transferring the data, but also verifying every step along the way, giving users more than they had before without compromising. So what's next for us? We're going to work

through our roadmap, focusing on deploying IRO for real users every step of the way,
so we can be sure to deliver new superpowers into the hands of all the great humans out there. This was very brief, so if you want to know anything more about these pieces,

almost a full team of number zeroes around, find us on the hallway or come to one of these
cool talks. I'm going to talk about the details of reimagining IPFS later today. Floris is

going to talk about the integration of IRO into DeltaJet. Azmir about measuring all the things. And Rudiger was going to talk about moving actual bytes with BAU and Blake 3 without

block limits. Thanks.

