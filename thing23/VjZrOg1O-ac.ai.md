# Data Transfer Batching Techniques featuring Blake3, CAR Mirror, and more - Philipp Krüger

<https://youtube.com/watch?v=VjZrOg1O-ac>

![image for Data Transfer batching Techniques featuring Blake3, CAR Mirror, and more - Philipp Krüger](/thing23/VjZrOg1O-ac.jpg)

## Overview

In this video, Philipp Krüger from Fission presents a comparison of different data transfer protocols and shares ideas on how to improve them. He discusses design goals like reducing roundtrips, maintaining incremental verifiability, and supporting different types of queries. He also explores how these ideas can be implemented with IPFS pin (BitSwap), car file transfer, CarMirror, GraphSync, and the Blake3/BAU method.

## Content

### Data transfer protocols and design goals

Philipp compares data transfer protocols like IPFS pin (BitSwap), car file transfer, CarMirror, GraphSync, and the Blake3/BAU method across their design goals, namely:

- Few round trips
- Incremental verifiability
- Query support
- Deduplication
- DAG layout flexibility

He then explores how one protocol, Blake3/BAU, achieves incremental verifiability and high speed, and suggests transferring some ideas from this protocol or other protocols like inlining hashes to achieve maximum structural sharing and efficient cold calls.

### Inlining hashes, queries as code, and deduplication

Philipp extends the concept of inlining hashes to IPFS and demonstrates an idea for CarV3. He envisions a process where peers can decide on the level of incremental verifiability needed for each interaction by inlining hashes according to predetermined buffer sizes.

Additionally, Philipp suggests implementing queries as WASM code, potentially moving toward using IPVM and tackling challenges such as query complexity and resource hogging. By viewing deduplication as a query, data transfer could be split into a preprocessing phase with WASM support and a transfer phase.

## Key Takeaways

- The design goals of data transfer protocols can greatly influence their performance and suitability for different use cases.
- Inlining hashes can help achieve maximum structural sharing, efficient cold calls, and incremental verifiability on IPLD.
- Implementing queries as code and deduplication as a query could provide more flexibility and efficiency in data transfer.
- Adopting WASM code for queries, IPVM support, and using preprocessing and transfer phases could provide future improvements to data transfer protocols.