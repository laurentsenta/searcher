# Decentralizing IPFS Gateways with Project Rhea - @willscott

<https://youtube.com/watch?v=0eJd2aqqSy8>

![image for Decentralizing IPFS Gateways with Project Rhea - @willscott](/thing23/0eJd2aqqSy8.jpg)

## Overview

In this video, Will Scott discusses the challenges of decentralizing IPFS gateways and introduces Project Rhea. The goal of the project is to improve IPFS gateways, validate Saturn as a CDN, and reduce centralization. Through various stages of implementation, Project Rhea aims to refine HTTP specs and leverage Saturn's thousands of points of presence. Continue reading to learn more about how Project Rhea benefits the IPFS community.

## Challenges and Opportunities in Decentralizing IPFS Gateways

IPFS gateways are currently operated by centralized operators. With the goal to improve this situation by decentralizing the gateways, we need to identify opportunities for optimization. One of the main challenges includes increasing the speed and efficiency of the gateways. Currently, there are only a few locations where gateways exist, resulting in higher latency for users. To address this issue, we can either increase the number of points of presence or leverage existing resources.

Protocol Labs has already built a CDN, Project Saturn, that has almost 2,000 nodes to quickly serve content. Project Rhea aims to utilize Saturn's infrastructure to improve IPFS gateways and ensure it works effectively as a decentralized CDN.

## Project Rhea: Objectives and Outcomes

There are three main objectives of Project Rhea:

1. Retrieve content from both IPFS and Filecoin.
2. Validate Saturn as a CDN for serving traffic.
3. Reduce the centralization of traffic through Protocol Labs-run entities.

There are two expected outcomes from Project Rhea that will benefit the IPFS community:

1. A shift in the trust model: Encourage clients to perform validation locally, leading to increased flexibility and improved experiences.
2. Development of new solutions: Bifrost Gateway and LASI.

## Bifrost Gateway and LASI

Bifrost Gateway is a new piece of software that decouples the parsing and interpretation of client requests from the actual retrieval of data. It enables a more semantically meaningful request for the data it needs to serve clients and defines what a trustless HTTP spec should look like for gateways serving content.

LASI (Lightweight Announcements Search Index) is a project that handles the fetching part of the process. It retrieves, constructs, and returns the content requested by the clients. By embedding LASI into Saturn, it is possible to leverage Saturn's extensive network of nodes and encourage users to interact with it directly.

## Conclusion

Project Rhea represents an exciting step forward in decentralizing IPFS gateways, promoting a lighter and more efficient system. By leveraging existing projects like Saturn and developing new solutions like Bifrost Gateway and LASI, Project Rhea is set to improve gateway latency, reduce centralization, and build a more resilient network for the IPFS community.