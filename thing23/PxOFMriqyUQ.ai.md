# AquaVM: pi-calculus based distributed algorithms - Mike Voronov

<https://youtube.com/watch?v=PxOFMriqyUQ>

![image for AquaVM: pi-calculus based distributed algorithms - Mike Voronov](/thing23/PxOFMriqyUQ.jpg)

## Overview

In this video, Mike Voronov introduces AquaVM, a pi-calculus based approach for coordinating and solving complex Web3 computing workloads. AquaVM enables ease of orchestration and execution of distributed algorithms. Topics discussed include how AquaVM works, its instructions, data types, implementation, incentivization layer, and motivation for creating the Distributed Choreography and Composition (DCC) working group.

## AquaVM Concept and Motivation

AquaVM aims to address the challenges in creating and executing compositions of functions located on different peers in a distributed environment. The approach is based on pi-calculus, which provides a robust theory for representing distributed algorithms, and a high-level language named Aqua for simplifying complex tasks.

The pi-calculus based model enables AquaVM to express and orchestrate decentralized computing. It supports executing multiple tasks simultaneously and efficiently handling call results using a pure computation model.

## AquaVM Instructions

AquaVM includes 14 instructions, which can express almost any distributed algorithm. Key instructions include:

- Call instruction: For calling functions on particular peers
- Sequential execution (seq)
- Parallel execution (par)
- Execution with errors (alt)
- Branching (match and mismatch)
- Fixed combinator instructions (forall and next)
- Instructions inspired by category theory (apply)

## AquaVM Data Types

AquaVM supports three primary data types:

- Scalars: Fully consistent values
- CRDT-like structures (streams and maps): Commutative (non-idempotent) data types supporting reads and writes
- Canonicalized types: A hybrid between scalars and streams with mixed algebra

## AquaVM Implementation

AquaVM runs on a multi-layered peer structure, including a network layer (libp2p), a core layer (manager of queues), and two pools, which are the Aqua VMs and services. This setup allows AquaVM to be run as a pure function, enabling asynchronous service execution and a fixed time for execution. 

The Fluence network is built on this basis, allowing for permissionless, incentivized, auditable, provable, and decentralized coordination of requests.

## Distributed Choreography and Composition (DCC) Working Group

Motivated by the goal to build great tools, solve complex problems, and push progress further, the DCC working group aims to unite researchers, developers, and enthusiasts interested in distributed choreography and composition. The group will focus on challenges and opportunities related to distributed algorithms and AquaVM.

## Key Takeaways

- AquaVM is a pi-calculus based approach for coordinating and executing complex distributed algorithms in web3 environments.
- It allows for expressive and dynamic distributed algorithm orchestration through its 14 instructions.
- The primary data types supported are scalars, CRDT-like structures, and canonicalized types.
- AquaVM runs on a multi-layered peer structure to enable decentralization and collaboration.
- The Distributed Choreography and Composition (DCC) working group aims to bring together experts and enthusiasts to further explore distributed algorithms and AquaVM.