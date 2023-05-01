# JavaScript performance - how to wring the most out of your Helia deployment - achingbrain

<https://youtube.com/watch?v=zPeLYosZ3Ak>

![image for JavaScript performance - how to wring the most out of your Helia deployment - achingbrain](/thing23/zPeLYosZ3Ak.jpg)

## Overview

In this video, achingbrain explains how to optimize JavaScript performance for Helia deployments. Topics covered include CPU performance, asynchronous operations, storage considerations, DAG structures, and more. By understanding the variety of factors that impact JavaScript performance, users can create more optimized and efficient applications for their specific needs.

## Article

JavaScript can be slow, especially when dealing with certain tasks like CPU-intensive work and asynchronous operations. However, understanding how to harness the power of JavaScript for your Helia deployment can ensure that your applications are optimized and efficient.

### Async operations

It is crucial to remember that async operations are not free. Adding the async keyword and 'await' can significantly slow down your code, so it is essential to use them only when necessary. Benchmark your code to determine when to use async operations properly.

_Async performance comparison:_
- Synchronous pipeline: 132,000 operations per second
- Asynchronous pipeline: 81,000 operations per second

### Storage considerations

Choosing the right storage solution for your application is critical. Be aware of bottlenecks and measure everything before making storage decisions. Some factors to keep in mind include:

- Network and I/O performance
- Block store choices such as S3, Mount Datastore, Tid Datastore, and more.
- Content reading options like IPFS, IPNI, etc.
- Pinning and garbage collection strategies

### Working with DAGs

DAGs, or Directed Acyclic Graphs, are an essential part of how Helia works. Properly balancing the size of your DAG can help reduce time spent transferring data between nodes.

_Optimized DAG performance comparison:_
- Helia default 256 KiB block size: Reduction from 10 seconds to 7 seconds (100 MB transfer)
- Kubo default 256 KiB block size: Reduction from 8 seconds to 2 seconds (100 MB transfer)

By considering these various factors, you can make the most out of your Helia deployment and create optimized and efficient applications.

## Key Takeaways

- Be aware that async operations are not free; only use them when necessary.
- Consider storage options and measure their impact on performance.
- Optimize your DAG structures to reduce data transfer times.
- Always benchmark and iterate on your code to ensure optimal performance in Helia deployments.