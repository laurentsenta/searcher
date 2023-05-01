# What is Rhea? - @willscott

<https://youtube.com/watch?v=p89i9_AskIw>

![image for What is Rhea? - @willscott](/thing23/p89i9_AskIw.jpg)

## Overview

In this video, William Scott introduces Project Rhea, an effort to improve and update the current IPFS gateways by incorporating the Filecoin decentralized CDN called Saturn. The purpose of Rhea is to optimize trust model, file retrievals, and latency, while reducing the cost of IPFS gateways.

## Content

### Project Rhea

Project Rhea is an effort that people around Particle Labs started in January to take another iteration at the current ipfs.io gateways. The goal of Rhea is to:

- Make Filecoin retrievals a first-class citizen, equivalent to IPFS retrievals.
- Validate Saturn as a CDN by making it handle real traffic at scale.
- Reduce the cost of running high-bandwidth Equinix deployments for ipfs.io.

The trustless gateway specification aims to allow getting content address data from an HTTP gateway without trusting the gateway. The new Saturn-based system is currently handling around 30% of the production traffic, validating its ability to handle the scale of the request.

### Trust Model

Current clients have a lot of implicit trust in the gateways, as the gateways are responsible for rendering correctly and providing the requested data. A new system using trustless gateway specification and car files is being developed to provide better performance and offload implicit trust from the gateways.

### Rhea Components

Components being developed as part of Project Rhea include:

- Bifrost Gateway: A lightweight proxy, which serves as the entry point for requests.
- Boxo: A package that handles HTTP semantics for rendering files and getting data from Saturn.
- Caboose: A library to choose the Saturn node for fetching content from.
- Lassie: A higher level scheduler to manage fetching content from IPFS and Filecoin nodes.

## Key Takeaways

- Project Rhea aims to improve upon the current IPFS gateways by integrating the Filecoin decentralized CDN called Saturn, which optimizes trust models, file retrievals, and latency.
- The trustless gateway specification allows clients to get content address data directly from the HTTP gateways and verify them independently.
- Components being developed for Project Rhea include Bifrost Gateway, Boxo, Caboose, and Lassie, which work together in handling requests, rendering, and fetching content from Saturn nodes.