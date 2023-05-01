# Private Data: State of the Art - Ian Preston

<https://youtube.com/watch?v=HVyrVUI2-RA>

![image for Private data: state of the art - Ian Preston](/thing23/HVyrVUI2-RA.jpg)

## Overview

In this video, Ian Preston introduces the state of the art of private data on IPFS, discussing the guiding principle of Pyrgos, a global private file system, and its features such as capability-based access control, BATs for low-level block-level access control, Cryptree Plus, Application Sandbox, concurrent GC, fast-seeking and arbitrarily large files, and Direct S3 access.

## Content

Hello, I'm Ian Preston. I'm going to talk to you about the state of the art of private data on IPFS. 

### Privacy Features in Pyrgos

Pyrgos has several privacy features:

1. Capability-based access control: Three kinds of capabilities - mirror, read, and write, giving precise control over access to data
2. BATs (Block Access Tokens): A low-level block-level access control using S3 v4 signatures for post-quantum protection
3. Cryptree Plus: Improved design from a post-quantum perspective and protecting as much metadata as possible
4. Application Sandbox: Allows running untrusted code over private data without compromising security

### Speed improvements in Pyrgos

Pyrgos also offers various speed optimizations:

1. Concurrent Garbage Collection (GC): Optimized GC makes the process significantly faster without affecting concurrent writes
2. Fast file seeking: Allows quickly seeking within large files improving streaming experience
3. Direct S3 access: Offloads bandwidth to enable direct downloads and uploads to S3 for faster access

## Key Takeaways
- Pyrgos offers a combination of privacy and speed improvements on IPFS
- Capabilities allow precise control over access to data within the system
- BATs provide post-quantum ciphertext level access control
- Cryptree Plus improves privacy by protecting metadata and enforcing access control
- Application Sandbox enables secure execution of untrusted code on private data
- Optimizations such as concurrent GC, fast-seeking, and direct S3 access contribute to Pyrgos' performance improvements