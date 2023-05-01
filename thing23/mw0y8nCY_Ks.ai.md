# IPFS War Stories - Mohsin Zaidi

<https://youtube.com/watch?v=mw0y8nCY_Ks>

![IPFS War Stories - Mohsin Zaidi](/thing23/mw0y8nCY_Ks.jpg)

## Overview

In this video, Mohsin Zaidi discusses the struggles faced while using IPFS in the Ceramic Network's production systems. The insights provided highlight issues, improvements, and lessons learned while making use of various components of IPFS.

## Content

For the past few years, the Ceramic Network has been using IPFS for its decentralized and advanced streaming protocol with hash-linked commits stored as IPLD data structures. Despite receiving a lot of support from Protocol Labs, there have been challenges in the utilization of IPFS components, especially when it comes to memory management, performance, and ease of use.

Some of the key challenges faced include:

- Memory utilization
- Go routines resulting in out-of-memory crashes
- Timeouts without any apparent reason
- Difficulty scaling and maintaining high availability using PubSub
- Migrating from JS-IPFS to Kubo

Throughout the talk, Mohsin shares the solutions they've implemented to address some of these issues and the lessons they've learned from their experiences. One takeaway is to focus on building production databases on top of vanilla IPFS, even though it can be tricky. Another important takeaway is to appreciate the support and cooperation from the community as everyone is aware of the shortcomings and strives to improve and fix them.

## Key Takeaways

- Utilizing IPFS in production systems can be challenging, but it's essential to focus on building production databases on top of vanilla IPFS.
- Collaboration and support from the IPFS community, including Protocol Labs, have been instrumental in addressing issues and improving the overall experience.
- As IPFS components continue to evolve, users should focus on picking the best components that suit their needs rather than expecting a one-size-fits-all solution.
- Sharing lessons learned and best practices within the IPFS community can help others overcome similar challenges and improve the overall development experience.