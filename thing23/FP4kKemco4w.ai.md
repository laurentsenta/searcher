# DHT Double Hashing Updates & Migration Plan - Yiannis Psaras & Gui

<https://youtube.com/watch?v=FP4kKemco4w>

![image for DHT Double Hashing Updates & Migration Plan - Yiannis Psaras & Gui](/thing23/FP4kKemco4w.jpg)

## Overview

In this talk, Yiannis Psaras and Gui discuss the DHT (Distributed Hash Table) reader privacy plan for IPFS, its benefits, and its migration strategy. The plan aims to improve reader privacy and requires a significant protocol change to IPFS. Specifically, they discuss the "double hashing" approach, its impact on reader privacy, and the challenges associated with the migration plan for transitioning to the new protocol.

## Content

### Benefits of Double Hashing

- The plain CID replay is not possible, which means that it will be more difficult for attackers to associate a client with the content they are requesting from the network.
- Prefix requests enable k-anonymity rules, making it more difficult to identify which content a client is looking for.
- Provide record encryption ensures that intermediate DHT servers cannot associate clients with the content they are requesting.

### Migration Plan

The migration plan focuses on ensuring the majority of nodes on the IPFS network upgrade to the new protocol simultaneously. This entails:

- Orchestrating nodes through a hard-coded IPNS key in a future release of Kubo
- Encouraging content providers to switch to the new DHT
- Allowing DHT clients to use both DHT versions during the transition period
- Implementing the DHT servers to run both DHT versions for a specific period

### Timeline

- Finalize the draft of the migration plan
- Announce the migration plan through blog posts and community updates
- Finish the implementation and testing phases by Q2 of the current year
- Finalize the migration plan, roll out the new Kubo release, and trigger the migration by the beginning of Q3

### Resources

- IPFS [Spec](https://github.com/ipld/specs/blob/master/block-layer/content-addressing/index.md) is the source of truth for understanding double hashing and its effects on reader privacy.
- Gui's [talks](https://www.youtube.com/watch?v=RZWkvcaeCQY) at IPFS Camp and IPFS Thing
- [Migration Plan Discussion](https://discuss.ipfs.io/t/update-to-dht-reader-privacy-migration-plan/12443) on the IPFS discussion forum

For additional discussion and input, participants are encouraged to reach out via the relevant channels on the IPFS Discord server or join the working group session on this topic.