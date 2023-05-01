# Moving the Bytes with BAU - Rüdiger Klaehn

<https://youtube.com/watch?v=bK9KDJxCfzI>

![image for Moving the bytes with bau - Rüdiger Klaehn](/thing23/bK9KDJxCfzI.jpg)

## Overview

In this video, Rüdiger Klaehn introduces BAU, a mix of hashing algorithm and data transfer protocol that is related to the Blake 3 hashing algorithm. He explains the simplicity, efficiency, and potential applications of BAU, as well as its implications for IPFS.

## Content

Arrow team's goal was to keep things extremely simple due to being a small team and wanting to put things into production as quickly as possible. The project started earlier this year and is used in DeltaJet. Arrow offers two primitives: blobs and collections.

- **Blobs** (Binary Large Objects): Can be of arbitrary size, from one byte to one terabyte. Uses the file store pattern by default.
- **Collections**: A blob containing a sequence of links with metadata. Also can be as big and contain as many links as required.

The main focus of Arrow's data transfer protocol is **verified streaming** — sending content over the wire only if it was requested due to a hash, and never sending data that does not match the hash. The goal is to detect wrong data immediately, validate on send and receive, and support a variety of data transfer scenarios.

Some possible applications for BAU include:

- Database sync on mobile platforms
- Machine learning datasets
- Game assets
- Directory trees
- Large file storage
- Disk images

Future plans include:

- Support for appending data
- Arbitrary writes to files
- Live database synchronization

Some limitations include:

- BAU does not work well with a deep and highly dynamic DAG (Directed Acyclic Graph)
- No support for Rabin chunking
- Complex integration with content-defined chunking

Despite these limitations, Rüdiger argues that BAU is very much in the spirit of IPFS due to its strong focus on content addressability and fine-grained incremental verification.

## Key Takeaways

- BAU is a mix of hashing algorithm and data transfer protocol related to the Blake 3 hashing algorithm
- It aims to be extremely simple and efficient
- BAU offers verified streaming and flexible data transfer scenarios
- There are potential applications in various fields, such as machine learning datasets, game assets, and disk images
- There are some limitations, such as no support for Rabin chunking and complexities with content-defined chunking