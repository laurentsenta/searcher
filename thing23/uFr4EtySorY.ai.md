# Boxo: Build Your Own IPFS Adventure - Gus Eggert

<https://youtube.com/watch?v=uFr4EtySorY>

![image for Boxo: Build Your Own IPFS Adventure - Gus Eggert](/thing23/uFr4EtySorY.jpg)

## Overview

In this video, Gus Eggert introduces Boxo, a project aimed at consolidating and simplifying the components of Kubo to make it easier for developers to build IPFS applications. Eggert presents the challenges faced by developers working in the Go IPFS ecosystem and how Boxo aims to address them.

## The Challenges of the IPFS Go Ecosystem

Developers working with Kubo may run into several issues:

- Complex dependency graphs
- Lack of documentation for reassembling components for specific use cases
- Difficulties in refactoring and staying on top of updates in multiple repositories

These obstacles often lead to frustration and slow progress for developers. Boxo was created to alleviate these difficulties.

## Introducing Boxo

Boxo aims to extract and consolidate components from Kubo and various repositories into a single library, simplifying abstractions, adding documentation and examples, and ensuring compatibility between different versions. As a result, developers can create IPFS applications and implementations more efficiently without having to deal with the cumbersome management of multiple repositories.

However, not everything will be included in Boxo—only components that offer high leverage will be added to the library.

## Transition to Boxo

During the transition period, developers will need to migrate their existing codes to the new Boxo repository. To assist with the migration, a tool has been developed to automatically add the appropriate version of Boxo and update import paths.

Further support will be provided, including additional documentation for Boxo, more testing, and release automation. Developers who become early adopters will receive additional support and guidance.

## Getting Started with Boxo

For developers keen on exploring Boxo and using it in their projects, they can find the Boxo repository on GitHub, provide feedback or ask for assistance through GitHub issues, or join the Filecoin Slack channel.

Remember, the goal of Boxo is to help developers build applications and implementations faster and more efficiently, without getting bogged down in the challenges of managing multiple repositories and dependencies. The adoption of Boxo should ultimately make the development process smoother and more enjoyable.