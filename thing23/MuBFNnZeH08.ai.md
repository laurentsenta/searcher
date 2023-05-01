# Is IPFS Ready for Decentralized Video Streaming? - Zhengyu Wu

<https://youtube.com/watch?v=MuBFNnZeH08>

![image for Is IPFS Ready for Decentralized Video Streaming? - Zhengyu Wu](/thing23/MuBFNnZeH08.jpg)

## Overview

In this video, Zhengyu Wu introduces their work on understanding and improving video streaming on IPFS. He first provides a background on IPFS and its popularity, and then investigates if it could support truly decentralized video streaming. To analyze this, Wu conducts a measurement study on video performance in IPFS and then introduces Telescope, a system designed to enhance video streaming on IPFS. Finally, he evaluates Telescope's performance compared to traditional streaming methods.

## Article

Hello everyone, my name is Zhenyu, I am a 2nd year PhD student at Stony Brook University. Today, I will be discussing our work, which will appear in the Web Conference 2023, on video streaming on IPFS, and asking the question: Is IPFS ready for decentralized streaming?

As we all know, IPFS is a decentralized storage and delivery network, built on top of a peer-to-peer network structure and using content addressing. With IPFS gaining popularity over time, and numerous apps being built around it, we wanted to explore if video streaming could utilize the benefits of IPFS to achieve a better playback experience.

However, upon our study, we noticed that one major problem with existing video streaming platforms like DTube is that they use their own private cluster, meaning that the video cannot be retrieved from the public IPFS network. Thus, we were unsure if truly decentralized video streaming was possible on IPFS.

To answer this question, we conducted a measurement study to analyze how video performs on IPFS currently. Then, we introduced Telescope, a system that aims to improve video streaming on IPFS, and evaluated its performance against traditional streaming methods.

Our measurement setup involved collecting video hash (CID) from IPFS Search and attempting to retrieve the video while measuring video resolution, video stall, and round-trip time (RTT) to the provider. We evaluated our data over 39,000 unique CIDs, successfully downloading over 28,000 of them.

Our findings showed that both high RTT and single encoding of the video were the main reasons for poor video streaming on IPFS. Adaptive Bit Rate (ABR) streaming, an alternative streaming method, also fell short in its performance with IPFS due to the P2P nature of IPFS. Therefore, we introduced Telescope.

Telescope serves as a proxy between the client and the IPFS gateway, helping the ABR client make better decisions by incorporating cache and throughput information in its decision-making process. We tested the performance of Telescope under different caching scenarios and found that it significantly outperformed traditional ABR and direct IPFS streaming, both in terms of reducing video stall and improving overall video quality.

In conclusion, we have shown that video streaming on IPFS currently performs poorly due to high RTT and single encoding. Existing video streaming solutions like ABR also perform poorly with IPFS. However, our work with Telescope has demonstrated that it is possible to achieve high quality video streaming on IPFS by incorporating caching and throughput information into the decision-making process.

For more information, please refer to our published paper on [Zhengyu Wu's website](http://www3.cs.stonybrook.edu/~zhenyuwu/).

## Key Takeaways

- IPFS is gaining popularity, but its performance in video streaming is currently poor.
- Telescope is a system designed to improve video streaming on IPFS by incorporating caching and throughput information for better decision-making.
- The evaluation of Telescope shows significantly better performance than traditional ABR and direct IPFS streaming in terms of video stall and overall video quality.