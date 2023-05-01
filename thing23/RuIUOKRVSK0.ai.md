# Evaluating Tradeoffs When Representing Database State - Carson Farmer

[![](https://i.ytimg.com/vi/RuIUOKRVSK0/maxresdefault.jpg)](https://youtube.com/watch?v=RuIUokRVSK0)

## Overview

In this video, Carson Farmer, from Textile and Tableland, shares valuable insights and lessons learned from designing databases in the IPFS space. The presentation focuses on a narrative that covers findings and trade-offs encountered and addresses the challenges of representing database state in the interplanetary database world.

## Article

For the past few years, my team at Textile has been working on designing and building databases in the Interplanetary File System (IPFS) space. Along the way, we learned several important lessons and discovered various trade-offs that come with representing database state in the IPFS ecosystem. This article shares a few of these insights and our experience building interplanetary databases, like Threads and Tableland.

### Key Takeaways

- **Database users want capabilities, not necessarily features.** Building powerful encryption and conflict resolution mechanisms is great, but if they don't address users' specific needs, they might not be as useful as initially thought. Catering to familiar use cases and access patterns improves the overall database experience.
  
- **Database users want a more traditional database interface that is decentralized, mutable, and queryable from anywhere.** Focusing on providing data, rather than data structures or complex features, can simplify the design and make it more appealing to users.
  
- **Floating point math is hard, and so are distributed systems.** While floating point values offer increased expressiveness and functionality, they can pose problems in distributed systems due to their non-deterministic nature. It is crucial to weigh the trade-offs and select an appropriate data representation.
  
- **Web3 presents unique access patterns and considerations for databases.** Techniques that might work well in traditional databases, such as caching and using B-trees, can be inefficient when dealing with proofs and data synchronization in a Web3 context. Emerging research in data structures and proofs can help address some of these challenges.
  
- **Trade-offs and challenges are inevitable when designing databases for IPFS space.** It is essential to stay open to learning and adapting as new technologies and ideas emerge. This learning process can lead to better solutions and drive improvements for both specific projects and the broader database ecosystem.

Overall, addressing these challenges and understanding trade-offs is crucial for designing better databases that cater to the needs of users in the interplanetary space. As the field continues to evolve, embracing collaboration and knowledge-sharing will not only benefit individual projects but contribute to the vitality of the entire database community.