# Fetch Content Like A Border Collie: Introducing Lassie - Hannah Howard

<https://youtube.com/watch?v=d5SzSm8NkUU>

![image for Fetch Content Like A Border Collie: Introducing Lassie - Hannah Howard](/thing23/d5SzSm8NkUU.jpg)

## Overview

In this video, Hannah Howard introduces LASI, a new multi-protocol IPFS implementation for coordinating retrievals among protocols that already exist in a simple, efficient, and modular manner. Hannah discusses the architectural design of LASI, its use in Project Raya, and the process behind the development.

## Content

### The Coordination Problem

The core problem LASI intends to address is the coordination of retrievals between protocols that already exist, such as BitSwap and GraphSync. LASI aims to make this retrieval process efficient and easy to use.

### Building LASI

LASI started as a Filecoin retrieval client, focused on making Filecoin retrieval easy and solving problems caused by serving different network protocols. However, the project evolved to accommodate multiple protocols and integrate them into its architecture.

### LASI Architecture

The top level of LASI's architecture includes a CLI interface, an HTTP server, a retriever interface, and a client for the network indexer. The candidate stream, retrieved from the network indexer, is sent to a protocol splitter, which divides candidates among different protocols. The coordination between protocols is managed by a series of retrievers that utilize channels for communication.

### Features and Advancements

LASI separates content routing and data transfer, utilizing network indexer services for improved response times and simplified handling of candidate streams. This separation enables LASI to provide advanced features such as manual peer lists, peer filtering, and protocol forcing.

### Integrating BitSwap

BitSwap was integrated into LASI by patching the existing GoBitswap library. However, the team is considering improving the BitSwap architecture and providing a lower-level interface to enhance the integration with LASI.

### LASI HTTP Interface

LASI's HTTP server exposes a single path URL interface, enabling the retrieval of car files (content-addressed archives) with various scopes such as "all," "file," and "block." The interface allows users to verify their data and supports byte ranges.

### Metrics and Improvements

To improve performance, LASI uses metrics and experiments to gather data and make adjustments to its architecture. The team plans to enhance LASI's decision-making process when selecting the right peers to retrieve data from, optimizing the speed and success of retrievals.

## Key Takeaways

- LASI addresses the coordination of retrievals among existing protocols, including BitSwap and GraphSync, making the retrieval process efficient and easy to use.
- The architectural design of LASI provides a modular and efficient approach to handling retrievals while supporting advanced features such as manual peer lists, peer filtering, and protocol forcing.
- LASI's HTTP interface enables users to verify their data, supports byte ranges, and may accommodate additional protocols in the future.
- The use of metrics and experiments helps optimize LASI's performance, and future improvements aim to further improve peer selection and retrieval success.