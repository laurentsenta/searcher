# Boxo: Build Your Own IPFS Adventure - Gus Eggert

<https://youtube.com/watch?v=uFr4EtySorY>

![image for Boxo: Build Your Own IPFS Adventure - Gus Eggert](/thing23/uFr4EtySorY.jpg)

## Overview

In this video, Gus Eggert presents Boxo, a library that simplifies the process of writing IPFS applications by consolidating components from Kubo and other repositories into a single library. This article will explore the challenges that Boxo aims to solve, its goals and limitations, and its current state.

## Article

Gus Eggert starts by explaining some of the challenges that developers face when working with the Go ecosystem. One of the scenarios he describes involves embedding a Kubo node in an application and customizing it, which exposes the complexity of the dependency graph landscape. Each box represents a Go module, and each circle represents a version. As maintainers of these libraries, it is crucial to consider the different versions that might be resolved and used by other dependencies. The lack of documentation for these scenarios also complicates the process, making it challenging to browse GitHub and figure out how to reassemble components.

Another challenge that Gus addresses is when Kubo does not perform well for a particular use case, and developers need to reassemble components to improve performance. With no documentation, developers must browse through repos on GitHub to figure out how to reassemble them.

Gus explains the challenges of maintaining Kubo repositories and staying on top of security updates, user requests, pull requests, CI failures, and Dependabot. While GitHub has tools for managing individual repositories, managing multiple repositories poses additional challenges.

To address these challenges, Gus and his team created Boxo, which aims to help developers quickly write IPFS applications without dealing with the complexities of the dependency graph. Boxo extracts components from Kubo and other repositories and consolidates them into a single library. Additionally, Boxo simplifies abstractions, adds documentation and examples, and provides snapshots of versions that all work together. Developers can add bespoke extension points and swap out different implementations of components using a dependency injection framework called FX.

Boxo aims to focus on the components that have high leverage and are useful when used with other pieces but are hard to use correctly. Boxo's goal is not to include everything in one place but to focus on what developers need to build applications quickly.

However, the transition period can be challenging for developers. The team is moving many repositories and providing a tool to migrate code to the new Boxo repository. The team plans to put notices on the top of repositories, keep Git history, move over all issues, close pull requests, and add deprecated tags on types. 

Boxo's current state includes extracting Kubo's gateway code, migrating over 25 repositories, moving tests from Kubo to Boxo, and providing examples for common use cases. However, nothing much has changed functionality-wise, and Boxo is not the place for IPFS components. Developers can find more information on Boxo on GitHub or the Filecoin Slack channel.

Gus also highlights that Boxo can be compiled and used to build custom applications with IPFS gateway handlers, among other things. Early adopters can benefit from white-glove service, and the team is open to feedback to make Boxo and the Go IPFS community better. 

In conclusion, Boxo is a promising initiative that aims to simplify the process of building IPFS applications by consolidating components into a single library. By addressing the complexity of dependencies, documentation, and maintenance, developers can focus on building applications quickly without worrying about the underlying complexity.