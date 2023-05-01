# WNFS: Versioned and Encrypted Data on IPFS - Philipp Krüger

<https://youtube.com/watch?v=LBMyRp4Ywew>

![image for WNFS: Versioned and Encrypted Data on IPFS - Philipp Krüger](/thing23/LBMyRp4Ywew.jpg)

## Overview

In this video, Philipp Krüger introduces the Web Native File System (WNFS), a versioned and encrypted file system built on IPFS. He discusses the challenges of working with encryption and data storage in browser environments and goes on to explain how WNFS addresses these issues, enabling fine-grained access control and concurrent write reconciliation. The talk also covers the current state of the WNFS project and encourages participation from the community.

## WNFS: A Data Backpack for Encrypted User Data

WNFS aims to create a "data backpack" for users, allowing them to control access to their data across multiple devices and applications. This involves putting encrypted data onto IPFS from browsers, maintaining user agency over encryption keys, and dealing with the hostile environment of browsers through the use of the WebCrypt API and non-extractable key pairs.

Key requirements for WNFS include:

- Fine-grained access control
- Storing data with untrusted peers (minimizing leaked information)
- Verifying valid writes without read access
- Handling concurrent writes

## The WNFS Implementation

The current WNFS implementation involves:

- Encrypting all files with different keys
- Organizing keys in hierarchies using skip ratchets and crypt trees
- Employing RSA accumulators for meaningful labels on ciphertexts
- Placing IPFS CIDs on top of a flat data structure (called a Private Forest)

These features work together to minimize metadata leakage, provide fine-grained access control, enable verified writes without read access, and allow concurrent write reconciliation.

## The Current State and Future of WNFS

The WNFS project is currently at a stage where it has implemented private file sharing and is working on improvements like conflict reconciliation and support for large data sets. The project is community-driven and aims to be highly portable so that it can be used on different platforms.

## Conclusion and Participation

WNFS aims to provide a secure, user-controlled encrypted file system built on top of IPFS. The project is open for community involvement and has already seen successful collaborations with projects like FunctionLand and Banyan. If you are interested in getting involved or learning more, you can check out the WNFS GitHub repository or join the discussion in the Web3 Storage Working Group.