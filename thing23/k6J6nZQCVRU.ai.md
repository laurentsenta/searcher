# Implementations Showcase: Lassie - A New Golang Implementation - Hannah Howard

<https://youtube.com/watch?v=k6J6nZQCVRU>

![image for Implementations Showcase: Lassie - A New Golang Implementation - Hannah Howard](/thing23/k6J6nZQCVRU.jpg)

## Overview

In this video, Hannah Howard introduces a new IPFS implementation called Lassie, an IPFS implementation in Go designed to help users get their data easily from IPFS and Filecoin networks. Lassie is versatile, with multiple use cases including CLI, an HTTP gateway, and a library for Go applications. However, it does not store or announce data, focusing solely on retrieval of data right.

## Content

Hello everyone, my name's Hannah, I'm a software engineer at Protocol Labs, and I'm here to tell you today about a new IPFS implementation called LASI.

LASI is an IPFS implementation in Go, designed to be like a scalpel, focusing on a single goal of helping users get their data. We built LASI because we believe that downloading your data from IPFS should just work. Today, LASI already supports both BitSwap and GraphSync, and we'll be adding a trustless HTTP protocol transport soon.

In LASI, we find your data for you, speaking to both the IPFS DHT and the Filecoin network through IPNI, the network indexer. We'll keep adding new and better ways to find data as time goes on.

As for its use cases, you can use LASI as:

1. A command-line executable: download and run it immediately to fetch data.
2. A lightweight HTTP gateway to IPFS and Filecoin: serves car files via the HTTP gateway interface.
3. A library for Go applications: seamlessly add retrieval from IPFS and Filecoin to your projects.

However, LASI does not store or announce data, focusing only on data retrieval, no block store or DHT announcements. It is designed to be a client, keeping a lightweight and almost completely stateless design.

LASI is already in use, becoming the primary tool for cache misses in the Saturn network, downloading about 140 million SIDs each week. The development team is focused on ironing out the remaining bugs and improving performance and documentation.

To learn more about LASI, attend the data transfer track starting after the morning keynote or find Hannah Howard at the conference. Use LASI, give feedback through GitHub, Slack, or Twitter, and help improve this new IPFS implementation.