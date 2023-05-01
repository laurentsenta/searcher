# Efficient P2P Databases with IPLD Prolly Trees - Mauve Signweaver

<https://youtube.com/watch?v=TblRt1NA39U>

![image for Efficient P2P Databases with IPLD Prolly Trees - Mauve Signweaver](/thing23/TblRt1NA39U.jpg)

## Overview

In this video, Mauve Signweaver introduces peer-to-peer (P2P) databases and shows how to build them on top of IPLD Prolly trees. They also explain efficient indexes and how Prolly trees can be leveraged for various use cases, including spatial indexes in augmented reality.

## Content

### How Regular Databases Work
- Query layer on top of collections of data
- Indexes over the data for faster searches
- Indexes sort data for efficient calculation and streaming results
- B+ trees often used for storing indexes

### Prolly Trees
- Like B+ trees but utilize content addressing (IPLD) for nodes
- Boundaries based on content address rather than insertion order
- Schema Root, Tree Node, Is Leaf structure
- Construction and updating the tree through batching and bottom-up approach

### Keyspace
- Top-level prefix for grouping
- Longer prefix for individual documents
- Separate prefixes for indexes
- Allows for efficient querying on indexed data

### Network as a Database
- Load data with block exchange, GraphSync, or other replication protocols
- Only load data needed for the specific query
- Cache and merge datasets to improve query times

### Trade-offs and Application Layer Considerations
- Larger chunks updated more often
- Smaller chunks need more round trips
- More indexes lead to more duplicate data, but are necessary for efficient queries
- Various merge strategies can be used for merging data from multiple sources

### Potential Applications and Future Work
- Spatial indexing in augmented reality
- Vector indexing for artificial intelligence use cases
- Improved merging capabilities for larger shared datasets
- Custom replication protocols and caching strategies to improve performance

## Key Takeaways

- IPLD Prolly trees provide a potential solution for efficient P2P databases
- Indexing and query performance can be significantly improved by properly utilizing Prolly trees and efficient keyspace
- IPLD Prolly trees can be applied to various use cases, including spatial indexing in augmented reality
- Future work in networking, caching, and replication strategies can continue to improve the performance and capabilities of P2P databases built with IPLD Prolly trees