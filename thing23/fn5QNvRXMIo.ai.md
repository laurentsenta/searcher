# Explorations into Decentralized Publishing - David Justice

<https://youtube.com/watch?v=fn5QNvRXMIo>

![image for Explorations into Decentralized Publishing - David Justice](/thing23/fn5QNvRXMIo.jpg)

## Overview

In this video, David Justice introduces their work on building a decentralized team blog using IPFS, FVM, and a smart contract. The project is aimed at providing censorship resistance, content ownership, verifiable authorship, platform resilience, and user agency.

## Content

For the past few years, our team at IPFS has been working on various projects related to decentralized publishing. We needed a place where we could rapidly communicate our work, while also dogfooding the technologies we develop. That's why we decided to build a team blog using IPFS, FVM, and a smart contract.

In this lightning talk, I discuss the first version of our decentralized blog, which consists of a single-page app viewer, a smart contract to store author and post data, and a publishing tool (CLI) to interact with the smart contract.

Our blog is built using the following components:

### Viewer
The viewer is a single-page app that fetches posts from IPFS, contract data, and author information. It stores this data in local storage, allowing for offline viewing and syncing when users come back online.

### Contract
The smart contract stores an array of post CIDs and an array of authors. This contract serves as a single source of truth for our blog, with the data being decentralized and verifiable.

### Publishing Tool
The CLI tool takes in a post CID and updates the smart contract with the new post. In the future, we plan to move this functionality entirely to the browser, allowing authors to edit markdown and publish updates directly.

## Future Improvements

Our team sees this project as a starting point for developing patterns for decentralized applications. Some improvements we're considering include:

- Removing external dependencies
- Decentralized RSS integration
- In-browser content editing
- Metrics and logging for usage patterns
- Improved developer tooling and tutorials

We're excited to continue working on this project and would love to hear your thoughts and suggestions on how we can improve. Please feel free to check out our [repo](https://github.com/multiformats-browser-teamblog) or join us at the Filecoin hack days to discuss further.