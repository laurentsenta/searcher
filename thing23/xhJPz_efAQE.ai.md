# Self-hosting IPFS Gateway with Bifrost-Gateway - lidel

<https://youtube.com/watch?v=xhJPz_efAQE>

![image for Self-hosting IPFS Gateway with Bifrost-Gateway - lidel](/thing23/xhJPz_efAQE.jpg)

## Overview

In this video, lidel from Protocol Labs introduces Bifrost Gateway, a new approach to IPFS gateway infrastructure aimed at simplifying self-hosting and reducing costs. The presentation covers the design decisions behind Bifrost Gateway, its use in Project RIA, and how to set up Bifrost Gateway for self-hosting.

## Content

Lidel starts by explaining the need for self-hosting and the importance of gateways in IPFS. Bifrost Gateway follows a design pattern that separates functionalities and optimizes for HTTP scaling, enabling cost-effective scaling and simplifying self-hosting.

One of the main design decisions behind Bifrost Gateway is not to have a peer-to-peer stack. Instead, it focuses on HTTP services, delegating content routing and data transfer/storage to other components in the system.

### Key Takeaways

- Bifrost Gateway, as part of Project RIA, aims to simplify self-hosting and reduce costs by separating functionalities and optimizing for HTTP scaling.
- Bifrost Gateway focuses on HTTP services and delegates content routing and data transfer/storage to other components in the system.
- It provides a single binary with a strict set of responsibilities, making it easy to integrate with existing infrastructures.
- Bifrost Gateway can act as a user agent for users who cannot run a full-fledged node on their device, providing integrity guarantees that come with content addressing.
- The project is still in early development, with several works in progress and future plans.