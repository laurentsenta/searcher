# RAPIDE: Speeding Up IPFS Downloads with @Jorropo

[YouTube Link](https://youtube.com/watch?v=Cv01ePa0G58)

![RAPIDE - @Jorropo](/thing23/Cv01ePa0G58.jpg)

## Overview

In this presentation, @Jorropo or Hugo, a team member at Protocol Labs shares his work on RAPIDE, a solution created to improve download speeds for IPFS file downloads. In the video, Hugo presents a detailed explanation behind the algorithm, the current situation of RAPIDE and the future goals for this project.

## The RAPIDE Algorithm

RAPIDE is designed to speed up IPFS downloads by downloading separate parts from multiple nodes simultaneously. Here's a summarized explanation of how the RAPIDE algorithm works:

1. Start at the root of the DAG.
2. Move through the DAG, calculating a metric for each block. Lower metrics indicate less competition for that block.
3. Download blocks by choosing those with the lowest metric.
4. If duplicate data is received, kill the connection and restart elsewhere in the graph.
5. If a worker encounters a node that is already fully downloaded, backtrack up the graph and download the remaining connected nodes.

## Current Features and Future Goals

The current RAPIDE implementation lacks some essential features such as content routing, strong order requests, and block store caching. However, it works well for IPFS get and IPFS pin commands. 

For the future development of RAPIDE, Hugo envisions the following goals:

- Speed up multi-peer and single-peer group downloads by optimizing the underlying protocols
- Improve the time-to-first-byte for content routing
- Expand RAPIDE's support to more applications and systems
- Port RAPIDE to other languages like GoWasm, Rust, or JavaScript
- Develop a specification for RAPIDE, making it easier for others to understand and implement

## Key Takeaways

- RAPIDE is a solution that aims to improve IPFS download speeds by downloading separate parts of files from multiple sources at once.
- The RAPIDE algorithm uses a metric system to determine which nodes to download from, minimizing duplicate data and maximizing bandwidth utilization.
- While RAPIDE's functionality is currently limited, future goals include speeding up IPFS downloads further, optimizing underlying protocols, and expanding support for more applications and systems.