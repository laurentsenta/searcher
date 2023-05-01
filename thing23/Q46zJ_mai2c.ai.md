# Double Hashing in IPNI: Reader Privacy at Scale - Ivan Schasny

<https://youtube.com/watch?v=Q46zJ_mai2c>

![Double Hashing in IPNI: Reader Privacy at scale - Ivan Schasny](/thing23/Q46zJ_mai2c.jpg)

## Overview

In this video, Ivan Schasny, a software engineer in the IPNI team, explains how they added double hashing to IPNI, a content routing system utilized in the IPFS and Filecoin networks, to ensure reader privacy at scale without causing any service disruption to the users. He also discusses IPNI's architecture, the challenges they faced, and the improvements they made to support increased traffic and requests.

## Double Hashing in IPNI

IPNI's main goal is to offer reader privacy by ensuring that neither IPNI nor a passive observer can understand which data users are looking for. This is achieved by double hashing the multi-hash and encrypting the provider identities with a key derived from the original multi-hash. IPNI features two indexes: one with the hashed multi-hash mapped to the encrypted peer IDs and another with the hash over the peer ID concatenated with the context ID mapped to the encrypted metadata.

IPNI's main components are:

- The storage providers
- The store the index instances (backed by key-value data stores)
- The assigner service
- The index star (a proxy server that scatters requests and gathers results)

In order to improve scalability and support double hashing, the team re-architected the system with the following changes:

- Introducing DHStore (Double Hash Store) as a remote service to store encrypted records
- Hooking DHStore directly to index star for faster lookups
- Introducing DHFind (Double Hash Find) to support regular queries until their complete retirement

The new architecture's goal is to have store the index backed by DHStore and utilize index star to scatter requests across multiple DHStore instances.

## Challenges and Improvements

The team faced several challenges while adding double hashing to IPNI:

- Improving database performance: They switched to Pebble, a key-value store used as the underlying engine for CockroachDB, for its simplicity and performance.
- Implementing S3 mirroring to speed up the ingestion process for rebuilding indices.
- Introducing nd.json to support faster access and decryption of records as they come in.
- Ensuring accurate index counts.

## Future Plans

The team plans to fully connect the new system to production, continuously improve performance and ensure reader privacy at scale. They also hope to make the transition easier for other index providers and operators.

## Key Takeaways

- The IPNI team added double hashing to ensure reader privacy at scale and redesigned the system to support increased traffic and requests.
- Challenges were related to database performance, ingestion speed, and record access.
- The new architecture relies on DHStore, index star, and DHFind, with future plans to entirely retire non-double-hashed paths.