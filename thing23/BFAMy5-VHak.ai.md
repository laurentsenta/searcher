# Foundations for Open-World Compute: Homestar, an IPVM Tale - zeeshanlakhani

<https://youtube.com/watch?v=BFAMy5-VHak>

![image for Foundations for Open-World Compute: Homestar, an IPVM Tale - zeeshanlakhani](/thing23/BFAMy5-VHak.jpg)

## Overview

In this video, Zeeshan Lakhani discusses the foundations for open-world compute, with a focus on Homestar, an implementation of IPVM that aims to provide content-addressable execution for content-addressable data in IPFS. He talks about various aspects of the system, sharing his experience of working on workflows, WASM and WIT (WebAssembly Interface Types), and challenges faced while building a plug-in substrate.

## Foundations for Open-World Compute

Homestar, an IPVM implementation, is being developed to provide:

- Content addressed execution to content-addressed data in IPFS
- WASM and IPFS
- Declarative invocation
- Captured results or receipts
- A distributed scheduler for determining nodes for running specific computes
- Memoization and adaptive optimization
- Managed effects

The goal of Homestar is to build a system for distributed computing that allows users to experience failure-oblivious code and virtual resiliency.

## Addressing Challenges in Distributed Computing

Addressing challenges in distributed computing, such as idempotency, determinism, workflows, and execution, is a key focus of the Homestar project. The team aims to enforce certain properties, so users can get specific guarantees when running compute operations.

## Workflow Engines and Execution Timelines

The Homestar project is built around various technologies, such as:

1. Workflow engines for handling the complexity of distributed computing
2. Promise pipelining to ensure the proper execution of tasks
3. Receipt storage for providing accountability and traceability
4. Integrating WASM with Interface Types for enabling cross-language composition

Homestar includes a plugin substrate that allows developers to create custom solutions and encourages collaboration.

## Conclusion

As the first step towards realizing open-world compute in IPFS, Homestar is focused on providing a framework that allows for distributed computing and failure-oblivious code. With continual development, Homestar seeks to improve upon current distributed-computing systems and make it easier for users to run compute operations in a decentralized manner.