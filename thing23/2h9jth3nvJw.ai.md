# libp2p Performance - Max (mxinden) & Marco Munizaga

<https://youtube.com/watch?v=2h9jth3nvJw>

![libp2p performance presentation image](/thing23/2h9jth3nvJw.jpg)

## Overview

In this video, Max (mxinden) and Marco Munizaga discuss the performance concerns and optimizations regarding libp2p, a networking protocol library. They delve into various advanced use cases of libp2p, such as BitSwap requests and Cademlia, the importance of latency and throughput, and benchmarking in realistic environments.

## Content

### Establishing Connections

The TCP connection establishment process within libp2p entails negotiating security protocols and establishing multiplexers before negotiating application protocols. Max and Marco discuss the possibility of optimizing this process by being more optimistic and assumptive during the negotiation.

### QUIC Protocol

The QUIC protocol can condense the entire connection process into just one round-trip, significantly speeding up the connection establishment process.

### Performance Metrics

The key performance metrics Max and Marco mention for libp2p are:

- Latency
- Throughput
- Connections per second

These metrics can be tested using a simple, client-driven perf protocol for libp2p. The examples mentioned include:

- Latency test by a client requesting a single byte to measure a ping-bong exchange.
- Throughput measurement by the client sending/receiving data and measuring its speed.
- Connections per second testing by repeating the latency test using multiple connections simultaneously.

It is important to note that this is a test protocol and should never be run in production.

### Benchmarking Setup

Benchmarking is done using two AWS nodes across the US continent on large machines running Go, Rust, Zig, and HTTPS implementations. Max and Marco discuss some potential improvements to the existing setup, such as using jumbo frames for large data packet transfer and establishing a more realistic data packet testing environment.

### Future Enhancements

Max and Marco imagine an ideal testing environment that replays real network traffic to simulate more realistic networking environments. However, this dream setup is not yet attainable, and the focus remains on improving the current system.

### Demo and Updates

Marco provides a live demonstration of the benchmarking process utilized. Please follow [libp2p's main repository](https://github.com/libp2p) to keep track of the testing and performance measurements of the project. PR163 is the specific pull request for this work.

## Key Takeaways

- Performance metrics, such as latency, throughput, and connections per second, are important for measuring libp2p's effectiveness in different use cases.
- Optimizing connection establishment and moving to QUIC protocol can help improve processing speeds.
- Keeping benchmarking realistic and focused on reproducibility is the current and future focus of the project.
