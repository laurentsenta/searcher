# Measuring on the Fast Track - Asmir Avdicevic

<https://www.youtube.com/watch?v=tZmcNktfoxw>

![Measure On Fast Track](/etherpadthing31_fast_track.jpg)

## Overview

In this video, Asmir Avdicevic discusses his experience as an engineer at Number Zero, working on the IRO project, and how he adapted existing methodologies to better measure performance and functionality. Using custom-built tools and continuous benchmarking, Asmir provides valuable insights into the development process and shares takeaways for improving engineering work.

## Article

My name is Asmir Avdicevic, and I am an engineer at Number Zero, primarily working on the IRO project - an implementation of InterPlanetary File System (IPFS). My role involves a heavy focus on metrics and performance measurement, helping our team better understand and improve the functionality of the systems we develop.

Over the past year, we built various tools to measure different aspects of our work - including latency, data transfer speeds, and overall system performance. I wanted to share our experiences, some key learnings, and takeaways with others to help them improve their engineering efforts.

### Building and Implementing Metrics

Initially, our metrics infrastructure was basic – it involved simple counters for information such as requests, bytes served, and bytes received. However, as our requirements evolved, we needed more advanced tools to keep up with our development pace.

One of our early performance measurement tools, the Gateway Checker, tested various aspects of our systems by sending requests through gateways and testing against real-world use cases. This turned out to be invaluable, exposing issues in latency and overall performance that guided our optimization efforts.

Another vital tool in our arsenal was NetSim, a homebrew solution for simulating network connectivity between nodes. It helped us immensely in testing our systems in a controlled environment, uncovering issues with connectivity and communication.

### Key Takeaways

1. **Continuous Benchmarking is Essential:** Regularly testing performance and functionality helps maintain a high standard of engineering work and uncovers issues before they become critical.

2. **Account for Bias in Metrics:** All measurements have inherent biases. It is crucial to be aware of these biases and make adjustments to ensure a fair comparison.

3. **Test Against Real-World Scenarios:** Synthetic benchmarks often provide inaccurate data compared to real-world testing. It is crucial to test in a live environment and consider production use cases.

4. **Dedicated Hardware is Invaluable:** If possible, use dedicated hardware for benchmarking to ensure accurate and consistent results.

5. **More Metrics Leads to Better Development:** Having a wealth of metrics and data at hand can better inform decisions and drive performance improvements.

In conclusion, our work at Number Zero has demonstrated the significance of continuous benchmarking, real-world testing, and dedicated hardware in maintaining and improving our software systems. By sharing our experiences and learnings, we hope to help others optimize their engineering processes and achieve better results. 

## Key Takeaways

- Continuous benchmarking is essential
- Account for bias in metrics
- Test against real-world scenarios
- Dedicated hardware is invaluable
- More metrics lead to better development
