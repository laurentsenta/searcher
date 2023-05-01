# InterPlanetary Consensus (IPC): Adding a consensus layer wherever is needed - Alfonso De la Rocha

<https://youtube.com/watch?v=qPLhqPGDZyk>

![image for InterPlanetary Consensus (IPC): Adding a consensus layer wherever is needed - Alfonso De la Rocha](/thing23/qPLhqPGDZyk.jpg)

## Overview

In this video, Alfonso De la Rocha introduces IPC, a framework for scalable consensus in existing distributed systems, focusing on Filecoin. The talk provides a high-level overview of IPC and its current status, including the [Spacenet testnet](https://github.com/ConsensusLabs/spacenet) and [APC agent](https://github.com/ConsensusLabs/apc-agent). 

## Video Content

Alfonso explains that IPC is meant to accommodate the scale of the Web2 model while providing more efficient consensus layers. The IPC framework provides horizontal scaling, allowing users to deploy different blockchain and consensus layers that they need. IPC can be thought of as a Layer 2 or Layer 3 solution for blockchain applications, anchoring security to upper-layer networks like Filecoin.

He presents a high-level overview of how IPC works by creating a hierarchy of different blockchains that anchor their security and interact seamlessly with each other. This allows consensus algorithms to be fine-tuned to fit the needs of specific applications, and Alfonso cites some use cases where IPC can be beneficial, such as faster finality, higher throughput, and secure global finality.

The IPC agent, an off-chain process that handles interaction with different subnets, is introduced as the key entry point to experimenting with IPC. The agent abstracts the complexity of interacting with various blockchains and provides developers with a straightforward method for implementing IPC solutions in their applications. Alfonso stresses that both the documentation and technology are still new and may contain potential issues, but he welcomes user feedback to improve the system.

IPC is currently deployed in the Spacenet testnet, which can be used to test various deployments options. Alfonso mentions plans to deploy IPC to Filecoin mainnet in the second quarter of 2021.

## Key Takeaways

- IPC is a framework for adding scalable consensus to existing distributed systems.
- The system provides horizontal scalability and allows for fine-tuning of consensus algorithms to fit specific application needs.
- Layer 2 and Layer 3 solutions provide security and seamless interaction with other networks.
- IPC agent is the key entry point for users to experiment with the technology.
- IPC is currently deployed in the Spacenet testnet, with a planned deployment to Filecoin mainnet in Q2 2021.