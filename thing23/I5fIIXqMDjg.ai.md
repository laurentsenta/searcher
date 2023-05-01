# Implementations Showcase: Iroh - IPFS Reimagined - dignifiedquire

<https://youtube.com/watch?v=I5fIIXqMDjg>

![image for Implementations Showcase: Iroh - IPFS Reimagined - dignifiedquire](/thing23/I5fIIXqMDjg.jpg)

## Overview

In this video, dignifiedquire introduces their work on Iroh, a reimagining of IPFS. They discuss the core values that drive the project, its development progress, and how they measure and improve its performance. Additionally, dignifiedquire announces the integration of Iroh into Delta Chat.

## Iroh: IPFS Reimagined

After IPFS camp, we evaluated the challenges and opportunities we had with the existing version of Iroh. This led to our refocusing on four core values:

- Reliability
- Performance
- Efficiency
- Peer-to-peer

These values are non-negotiable when shipping an IPFS implementation, which is why we rewrote Iroh from the ground up.

## Roadmap and Progress

We're tracking four high-level parts in our roadmap:

1. Data structures
2. Data transfer
3. Connectivity
4. Content routing

We're now in the test and improve phase for both data structures and data transfer. We've also spent significant time working on connectivity, and will soon begin testing our results. As for content routing, we have a good overall understanding but haven't yet started implementation.

Since our refocus, we've shipped three releases up to version 0.4.1. These releases represent our progress, and also act as the foundation for our partners.

## Integrating Iroh with Delta Chat

In collaboration with the Delta Chat team, we integrated Iroh to deliver a backup transfer solution for users. This allows Delta Chat users to move data from one device to another in a verified, encrypted, and fast way. This integration is live today in the latest releases of Delta Chat on all major mobile and desktop platforms.

## Performance Metrics

We built perf.iro.computer, a continuous measurement platform for Iroh that records the performance of every commit. This allows us to track and follow Iroh's development closely. Our benchmarks include network tests with simulated NATs, latencies, and packet loss, and real-world-oriented tests like transferring the source code of the Linux kernel.

## Comparing to Web 2.0 Technologies

Iroh now approaches transfer speeds similar to curl when transferring data on a local machine, but with added data verification every step of the way. This means that users don't have to compromise on performance to gain these new benefits.

## The Road Ahead

We will continue to work through our roadmap, focusing on deploying Iroh for real users at every stage. The Iro team at number zero is available to discuss further details on various aspects of Iroh, and they will be presenting talks on topics such as reimagining IPFS, integrating Iroh into Delta Chat, measuring performance, and advancing data transfer technology with BAU and Blake 3.