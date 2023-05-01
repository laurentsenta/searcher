# Effectiveness of Bitswap Discovery Process - Gui

<https://youtube.com/watch?v=zppddk2O9UQ>

![image for Effectiveness of Bitswap Discovery Process - Gui](/thing23/zppddk2O9UQ.jpg)

## Overview

In this video, Gui talks about a measurement run at ProbeLabs to assess the effectiveness of the Bitswap discovery process in IPFS. The focus is on content discovery and routing rather than data transfer. The goal is to analyze whether the static provider search delay of 1 second can be optimized for better content routing efficiency.

## Motivation and Method

The motivation behind this measurement is to evaluate the effectiveness of the 1-second search delay in Bitswap's content discovery process. To do this, Gui and his team used a list of uniquely requested CIDs collected by listening to the Bitswap traffic. They allowed Bitswap to search for and retrieve a block for 15 seconds without utilizing the DHT. If DHT queries were needed in cases where content wasn't discovered, the team ensured the content was actually reachable.

## Results and Observations

- More than 98% of content discoveries were successful, meaning Bitswap is highly efficient in discovering content.
- For each request, Bitswap solicited over 800 peers, which indicates a substantial amount of network traffic.
- Top 20 providers served almost 75% of the requested CIDs.

Regarding latencies, most content is discovered within 1 second. However, some content takes up to 15 seconds to be discovered. Around 80% of the content is obtained within 200 milliseconds, suggesting that nearby content can be provided swiftly.

## Recent Developments and Key Takeaways

Some recent developments include upgrades to the connection manager, which now limits inbound connections, making the broadcast process less efficient yet reducing network spam. The team also experimented with removing the provider search delay but found that it did not improve performance due to side effects or bugs in Bitswap's session handling.

The key takeaways from this study are:

- Bitswap is highly effective at content discovery but is very inefficient in terms of the network traffic generated.
- The content discovery mechanism does not scale linearly as the network grows.
- Top providers, such as NFT.storage, serve a large portion of the requested content.
- The use of more efficient content routing mechanisms like IPNI or DHT is recommended, and efforts should be directed towards improving Bitswap performance by reducing the necessity for broad broadcasts.

For more detailed information, refer to the [full report on GitHub](https://github.com/protocol/network-measurements).

## Questions and Answers

**Q: Where did the metrics of CID availability come from?**

A: The metrics were collected by passively listening to Bitswap traffic and ensuring that each CID was unique.

**Q: What are the suggested next steps?**

A: One suggestion is to fix Bitswap for a provider search delay of zero and perform a parallel DHT query. This would allow Bitswap and Kubo to be less spammy and make use of more efficient content routing mechanisms like DHT or IPNI.

**Q: Have the experiments been run from remote locations?**

A: The experiments shared were run from within the European Union. Although it's anticipated that performance in North America would be similar due to the prevalence of NFT.storage nodes, remote locations could shift the graphs, affecting latency timings.