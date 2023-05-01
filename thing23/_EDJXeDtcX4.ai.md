# IPNI: The InterPlanetary Network Indexer - Masih Derkani

<https://youtube.com/watch?v=_EDJXeDtcX4>

![image for IPNI: the InterPlanetary Network Indexer - Masih Derkani](/thing23/_EDJXeDtcX4.jpg)

## Overview

In this video, Masih Derkani introduces IPNI (InterPlanetary Network Indexer), an alternative routing system designed to provide SIDs by the billions while maintaining sub-10-millisecond latencies. Masih discusses the need for IPNI, design decisions, recent developments, and the future roadmap for the project.

## Content

### Why IPNI?

- Existing routing systems face scaling issues
- Need for a faster lookup, especially for large-scale content providers
- Storage prices continuously decrease, allowing increased replication
- Allows for a content-addressed network without CDNs (Content Delivery Networks)

### Design Decisions

- Advertisements point to immutable data, allowing for efficient handling of content
- IPLD data structure is efficient for advertising and storing content
- Network indexer can infer if a CID has been seen before, preventing duplicate requests
- Advertisement chains and entries can help maintain the integrity of the indexing system

### Recent Advances

- Extended provider families provide multiple node support and retrieval through different protocols
- Reader privacy makes retrieval requests private, adds a security layer
- Advertisement mirroring allows fast catch-up of new indexer nodes

### Streaming Responses

- HTTP accept header, application x-ndjson, enables streaming of multihash lookups
- Useful for quickly finding content, scaling to match network growth

### Future Roadmap

- Private lookups by default, leveraging reader privacy
- More adoption - integration with existing systems, lower barriers to adoption
- Federation - consistency across multiple IPNI instances
- Advertisement mirroring, caching
- User-configurable options for providers

## Key Takeaways

- IPNI addresses limitations in current content routing systems, aiming to provide fast lookups and support large-scale content providers
- The protocol supports efficient content advertising and management
- Multiple extensions have been added to improve performance and functionality
- Future goals include increased privacy, user flexibility, and wider adoption