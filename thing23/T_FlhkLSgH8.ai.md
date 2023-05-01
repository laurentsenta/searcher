# Hello Helia - achingbrain

<https://youtube.com/watch?v=T_FlhkLSgH8>

![image for Hello Helia - achingbrain](/thing23/T_FlhkLSgH8.jpg)

## Overview

In this video, Alex Potts-Seedys introduces Hello Helia, a new, lightweight and more extensible IPFS implementation in JavaScript. He discusses the motivations behind creating a replacement for js-ipfs and dives into the minimal API it offers. Additionally, Alex talks about the progress of libp2p, the networking layer of IPFS, and showcases various demos of Helia in action.

## Content

### Hello Helia: A new IPFS implementation in JavaScript

Hello Helia is a new IPFS implementation in JavaScript that aims to be:

- Smaller
- More lightweight
- More extensible
- More observable
- Faster

### Motivations for replacing js-ipfs

Some of the reasons for replacing js-ipfs include duplicative features, unnecessary bundling of code, and an API that does not cater to the strengths of JavaScript. Helia aims to improve the user experience and increase maintainability.

### Key Components of Helia

Helia offers a simple API with four main components:

1. **Block store:** A store for putting and getting blocks, with support for bitswap and other data transfer protocols
2. **Data store:** A key-value database with implementations for IndexedDB, LevelDB, and file systems
3. **Pinning and garbage collection:** APIs for pinning blocks and DAGs and efficiently deleting unpinned blocks
4. **libp2p:** Networking layer providing peer discovery, data transports, and more.

Helia also allows the use of various file systems, such as UnixFS and other custom file systems, promoting flexibility and innovation.

### Observability and progress events

Helia offers progress events for various tasks, providing insight into the workings of the system and making debugging easier for users.

### Getting involved with Helia

Users are encouraged to:

- Port their apps to work with Helia
- Contribute to the examples repository
- Connect with the development team for assistance, feedback, or sharing their use cases

### Future developments

Upcoming developments in Helia include exploring additional file systems and integrations, running bootstrapper nodes, and supporting the latest versions of libp2p.

### What's the future of js-ipfs?

JS-IPFS development will no longer continue, but any emergency security fixes might be backported if necessary. Future work will focus on Helia, which offers equivalent functionality.

## Key Takeaways

- Helia is a new, lightweight, and extensible IPFS implementation in JavaScript, aiming to replace js-ipfs.
- The main components of Helia are block store, data store, pinning and garbage collection, and libp2p.
- Helia offers built-in progress events for increased observability and ease of debugging.
- The development team encourages users to get involved by porting their apps, contributing to examples, and sharing their use cases.
- Future developments for Helia involve expanding file system support, working with the latest libp2p versions, and improving overall functionality.