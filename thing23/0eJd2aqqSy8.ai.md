# Decentralizing IPFS Gateways with Project Rhea - @willscott

<https://youtube.com/watch?v=0eJd2aqqSy8>

![Decentralizing IPFS Gateways with Project Rhea - @willscott](/thing23/0eJd2aqqSy8.jpg)

## Overview

In his talk, Will Scott discusses the HTTP gateways of IPFS and the centralization of operators that currently exist. He then introduces Project Rhea, a PL initiative to decentralize and improve Internet Protocol File System (IPFS) gateways. The project aims to retrieve data from both IPFS and Filecoin and reduce the centralization of IPFS gateways. The talk also looks at two key outcomes of the project, which includes a trust model shift and the development of new software -  Bifrost Gateway and LASI. 

## Content

Will Scott's talk revolves around IPFS gateways and how they function currently. He points out that the HTTP gateways currently in use are operated by a centralized set of operators. Moreover, depending on the URL, users will be directed to a specific operator. To address this issue, Scott introduces Project Rhea, which aims to decentralize and improve IPFS gateways.

When accessing a gateway, users make requests of the form IPFS/SID/path to access files or a collection of files. These requests initially go to a load balancer which then sends it to an IPFS node that looks like the one on your computer. However, it is configured differently and tuned differently from a regular IPFS node. This IPFS node retrieves the content only when it doesn't already have it from the broad public IPFS network.

To address the issue of centralization, Scott proposes that we need more points of presence to terminate connections closer to users. However, this would require servers in various locations to ensure faster retrieval of IPFS data. 

The good news for the decentralized CDN is that Protocol Labs already has something like that. The project Saturn, that last fall, launched has almost 2000 nodes currently. By leveraging the available nodes, the project aims to perform this bridge faster for users and provide a better experience.

Project Rhea has three concurrent goals. The first is to retrieve not just from IPFS, but also from Filecoin, broadening the range of retrievals of content address data. The second aim is to validate Saturn as a CDN and validating that this decentralized CDN is an effective way to serve the traffic. The third and most crucial aim of Project Rhea is to reduce the centralization that has occurred in most IPFS gateways' traffic.

Two significant outcomes of Project Rhea that Scott discusses are a series of iterations of shifting the trust model and developing new software -  Bifrost Gateway and LASI. Bifrost Gateway is a refactoring of the current gateway code and can make semantically meaningful requests for data that it needs to serve HTTP requests. Requests from the load balancer will go to Bifrost Gateway instead of a Kubo node. On the other hand, LASI will fetch IPFS and Filecoin network and be able to retrieve, construct, and return the content being asked. 

Overall, Project Rhea is an exciting initiative that can lead to faster retrieval of IPFS data and a better user experience by reducing the centralization of IPFS gateways.