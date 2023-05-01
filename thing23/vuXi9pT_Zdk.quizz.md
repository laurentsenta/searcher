## List all the products, tools, and companies discussed

- Kubo: IPFS's first implementation, versatile and suited for the independent self-hoster
- Brave: uses Kubo for its IPFS integration
- HTTP gateways: powered by Kubo, including IPFS and Bifrost gateway
- IPFS Desktop: uses Kubo behind the scenes as its engine
- Boxo: library extracted from Kubo, used for other Go-based implementations (e.g. Bifrost, IPFS Cluster, Lotus)
- Go-libp2p: resource manager integrated into Kubo for resource accounting
- CI: Kubo's not ideal for CI runs
- Docker: Kubo distributed via Docker images
- CID.contact: network indexer, queried in parallel by Kubo by default
- Bifrost gateway: extracted from Kubo code, for HTTP gateways
- Protocol Labs: company behind IPFS

## List all the weak signals of things an almighty AI could do to improve the presenter's life

- Kubo limitations: finding solutions for large infrastructure providers to have more control, introspection, and specific features like resource utilization bounds, throttling, and content policies
- Resource management: improving the resource accounting and management experience in Kubo
- Gateway improvements: enhancing and streamlining the implementation of new features (e.g. verifiability, IPNS records) in the IPFS gateway protocol
- Networking improvements: strengthen routing table resilience, provide guidance for DHT operation, and enhancements for bitswap (metrics, timeouts, back pressure)

## List all the weak signals of things that are already great in the presenter's life

- Kubo's versatility and controllability: a wide array of configuration options available to users
- Kubo's prevalence: dominant implementation of IPFS, powering a large portion of the network
- Configurable content routing: implemented HT routing V1 API, enabling CID.contact to be queried in parallel
- Boxo library: extraction and consolidation of Kubo functionality for spawning other Go-based implementations
- Go-libp2p resource manager: tackling resource accounting issues despite being introduced late into the project
- Collaborative environment: more than 350 direct contributors, many supporters, and a talented core team behind the IPFS project
- Communication: willingness of the presenter and the team to engage in discussions and take feedback on their work