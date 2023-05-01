# Opening the DHT to Large Content Providers - Gui

<https://youtube.com/watch?v=bXaL64fp55c>

![Opening the DHT to Large Content Providers](/thing23/bXaL64fp55c.jpg)

## Overview

In this video, Gui introduces improvements to the DHT with a focus on optimizing the reprovide process for large content providers. The proposed solution aims to minimize the number of connections and messages required to provide content, leading to a more resource-efficient IPFS network. This will allow large content providers to use the DHT and reduce reliance on BitSwap as a content distribution method.

## Introduction

Distributed Hash Tables (DHT) are a crucial component of IPFS, allowing content to be advertised efficiently across the network. However, the DHT has not been particularly friendly to large content providers due to the inefficiency of the provide process. As a result, large content providers often resort to using BitSwap, which is known for its chattiness and spam across the network. This video discusses a proposed solution for optimizing the DHT provide process and making it more efficient for large content providers.

## Optimizing the DHT Reprovide Process

The proposed solution involves grouping Content Identifiers (CIDs) together by XOR distance in order to allocate them to the same DHT server. This allows for minimizing the number of connections and messages needed for providing content. To do this, the key space is swept and the CIDs are sequentially allocated to the DHT server. This minimizes the number of open connections required and reduces the overall network traffic generated.

The new reprovide mechanism focuses on:

- Grouping CIDs by XOR distance
- Sweeping the key space to allocate CIDs sequentially
- Utilizing temporary key-value stores to map peer IDs to keys
- Scheduling reprovides to avoid rush hours and manage resources efficiently

## Performance Evaluation

The performance gains achieved by this optimization method are significant. For large content providers providing 1 billion CIDs every 22 hours, the number of connections to be opened drops from 35 billion to 28,000. The number of messages to be sent to achieve successful reprovide operations significantly reduces from 70 billion to approximately 20 million.

## Implementation and Rollout Plan

The proposed optimizations are set to be implemented and integrated into the double hash DHT later this year. Some additional changes to the interfaces are required, allowing for a more modular approach to content routing systems. This will ultimately lead to a more efficient IPFS network while making it more approachable for large content providers looking to engage with DHT.

## Key Takeaways

- Optimizing the DHT reprovide process allows large content providers to use the DHT and move away from BitSwap.
- Grouping CIDs by XOR distance and sweeping the key space allows for minimizing open connections and reducing network traffic.
- Implementing and integrating these optimizations is planned for later this year, in line with the double hash DHT.
- Improved scheduling allows for efficient resource management and optimal reproviding intervals.

For additional information, check out the [Notion page](https://www.notion.so/pl-ipfs/Gui-Ibarra-6eadaac9dce9417eae52bbced809f826).