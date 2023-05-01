# ODD.js, a technical overview - icidasset

<https://youtube.com/watch?v=ByQbY3lNAck>

![image for ODD.js, a technical overview - icidasset](/thing23/ByQbY3lNAck.jpg)

## Overview

In this video, the presenter gives a technical overview of ODD.js, also known as the Auth SDK. He delves into how the SDK builds around Vision's WinFS and UCan protocols which allow users to create web applications without backends. The discussion focuses on the file system, DIDs, and components and layers in the SDK, as well as a live demo of how the system works. [Watch the video here.](https://youtube.com/watch?v=ByQbY3lNAck)

## Content

For the past few years, we have been developing the Auth SDK to make it easy for developers to create web applications without a backend. With the use of Vision's protocols, such as WinFS and UCan, we have not only made this possible but have also added features like decentralized authorization and DeaDentifiers (DIDs) to allow for greater user control and security. 

### File System

At the core of Auth SDK is the file system, which includes at least three partitions: public data, encrypted data, and a compatibility layer (UnixFS). The user remains in full control of their file system, with the private forest and "dark forest" features hiding encrypted IPLD blocks for added security and privacy. This results in an immutable top-level SID.

### DIDs and Accounts

With Auth SDK, there are two types of accounts: first-class vision accounts and app accounts. Users can register an agent DID from their device, which becomes the root DID and is used for creating accounts. The DID management and account creation process are facilitated by the WebCrypto API, which uses RSA or Edwards curve cryptography.

### Components and Layers

The Auth SDK is organized into components and layers, allowing for customizability in a way that is easy to test and integrate with other ecosystems. This includes managing how and where IPLD blocks are stored, handling DNS lookups, and controlling where keys are stored. Additionally, there are predefined compositions called plugins that can extend Auth SDK's functionality.

### Live Demo

In the live demo, the presenter showcased updating the file system and syncing changes across devices. This demonstrated the ease of creating web apps without a backend using Auth SDK, and how Plugins like WalletOdds can interact with MetaMask for user authentication and to sync data with the file system.

## Key Takeaways

- Auth SDK allows developers to create web apps without backends, relying on Vision's WinFS and UCan protocols for decentralized authorization and security.
- The SDK revolves around a user-centric file system, with public and encrypted data partitions, and the UnixFS compatibility layer.
- DeaDentifiers, or DIDs, play a crucial role in user authentication and account creation, enabling two diverse types of accounts.
- The Auth SDK is organized into components and layers, providing developers with the flexibility to customize and build upon the toolkit.
- The live demo showcased the versatility and ease of creating web apps using the Auth SDK.