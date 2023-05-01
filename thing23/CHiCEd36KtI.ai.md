# The Name Name Service - Blaine Cook

<https://youtube.com/watch?v=CHiCEd36KtI>

![image for The Name Name Service - Blaine Cook](/thing23/CHiCEd36KtI.jpg)

## Overview

In this video, Blaine Cook introduces the concept of the Name Name Service (NNS), an experimental system designed to securely map names to data. NNS can potentially help in finding keys, enabling encrypted communication, and providing contact or content information in a decentralized manner. Touching on the use of Decentralized Identifiers (DIDs) and Distributed Hash Tables (DHTs), Cook discusses the technical challenges and questions surrounding the development of NNS.

## Content

### What is NNS?

NNS is a system that securely maps names to data. Some potential use cases for NNS include:

- Looking up keys without knowing the DID method
- Enabling encrypted communication through channels like Signal
- Finding where a user shares content (e.g., Instagram photos, Mastodon posts)
- Discovering information at a URL on IPFS
- Package naming with WebAssembly (Wasm) code

Names in the context of NNS are publicly unique and verifiable, such as DNS hosts and domains, email addresses, social media handles, HTTP URLs, and ISBNs. DIDs, despite being central to the concept, are not considered names themselves in this context.

### How does NNS work?

- Generate a YouCan that delegates from a name to some data, authenticated by a DID that implies control of a name
- Upload the YouCan to the NNS DHT using the name as a key, which will succeed only if the signing key matches the issuer and the name matches the issuer
- Fetch the YouCan from the NNS DHT using the name as a key

### Technical Challenges and Questions

Some technical challenges and questions surrounding NNS include:

- Addressing latency and locality in using a DHT
- Allowing authorities to opt into hosting parts of the namespace
- Determining what consistency looks like for different names and use cases

### What NNS is not

- A way to prove ownership of a name
- A new namespace
- Guaranteed to be globally consistent
- A blockchain

### Questions and Discussion

Some key points raised during the Q&A section include:

- Considering the use of URI schemes for name disambiguation
- Comparing NNS with projects like Keybase and Keyoxide
- Leaning on DIDs for name self-description and avoiding clashes in named spaces

To learn more about the Name Name Service, watch the video and join the discussion.