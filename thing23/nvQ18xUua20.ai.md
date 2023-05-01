# Measuring IPFS: ProbeLab & Data-driven Protocol Design - Yiannis Psaras

<https://youtube.com/watch?v=nvQ18xUua20>

![image for Measuring IPFS - Yiannis Psaras](/thing23/nvQ18xUua20.jpg)

## Overview

In this video, Yiannis Psaras introduces ProbeLab and its role in data-driven protocol design and optimization within the IPFS ecosystem. He provides insights into the tools and methodologies they use to analyze the performance of various content routing subsystems and how to access the data.

## Article

At ProbeLab, our primary focus is on collecting and analyzing protocol data that make up all the components of the IPFS network. Our goal is to explore the details of their performance and fine-tune them based on data-driven insights.

We have developed a range of tools to dig into the details and uncover how the protocols perform. The results, along with the context and details of our experiments, can be found on probelab.io. Here, you can also access links to the GitHub repositories that host the tools we use, giving you a complete view of our work.

In addition to our in-depth analysis, we produce weekly reports that offer snapshots of the performance and health of the network. As part of this effort, we monitor the time to first byte for several websites from different regions and compare the performance of IPFS through Kubo and HTTP. More often than not, content through IPFS loads faster, which could be attributed to content addressing and universal caching.

Currently, we are focusing on a few content routing subsystems, primarily the IPFS public DHT, bitswap, and hydras. As we expand our scope, we hope to collaborate with others working on different content routing systems and showcase their brilliant work.

Right now, there are two places to access our findings:

1. stats.ipfs.network: This platform points to the weekly reports, offering snapshots of the performance and health of the network.
2. probelab.io: This site provides more in-depth reports, detailing protocol performance and the impact of parameter changes.

In the future, we plan to bundle everything under stats.ipfs.network for easier navigation.

We also host a measuring IPFS track where we discuss our work in greater depth, addressing questions and concerns from the community.

## Key Takeaways

- ProbeLab focuses on data-driven protocol design and optimization within the IPFS ecosystem.
- They have developed tools to analyze the performance of various content routing subsystems.
- ProbeLab's main platforms currently are stats.ipfs.network and probelab.io.
- Collaboration with others working on content routing systems is encouraged.
- IPFS often loads content faster than HTTP, indicating the potential benefits of content addressing and universal caching.